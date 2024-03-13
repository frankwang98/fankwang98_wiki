








#### 文章目录


* + [1. 技术原理](#1__1)
	+ [2. 代码实现](#2__18)




### 1. 技术原理


MPC，即Model Predictive Control（模型预测控制），是一种基于动态模型的控制算法。MPC算法通过建立系统的数学模型，根据当前状态和一定时间内的预测，优化未来的控制输入，从而实现对系统的控制。


MPC算法主要分为以下几个步骤：



```
1. 建立数学模型：根据系统的物理特性，建立状态空间模型或者传递函数模型。
2. 预测状态：根据当前状态，利用建立的数学模型对未来一段时间内的状态进行预测。
3. 生成控制输入：根据预测的状态和控制目标，利用最优化算法生成控制输入。
4. 执行控制：根据生成的控制输入，执行控制。
5. 更新状态：根据执行的控制输入，更新系统状态，并进入下一次预测和控制循环。

```

基于模型预测控制的轨迹跟踪算法对未来轨迹的预测和处理多目标约束条件的能力较强。主要体现在：能够考虑系统的非线性和时变性，适用于各种复杂系统的控制；能够考虑多个控制目标，并在它们之间进行平衡和优化；能够对约束条件进行有效的处理，例如系统的输入和输出限制、状态变量的可行性等。


MPC算法可以用于实现车辆的路径跟踪和速度控制。具体地，利用车辆的动态模型，预测未来一段时间内的车辆状态（例如位置、速度、加速度等），并根据预测结果生成最优的车辆控制输入（例如方向盘转角、油门踏板位置、刹车踏板位置等），从而实现对车辆的精确控制。MPC算法还可以考虑车辆与周围环境的交互，例如与其他车辆、行人和路标的交互，从而实现更加安全和高效的自动驾驶。


### 2. 代码实现


在Autoware中，MPC算法主要实现在mpc\_follower节点中。该节点接收`/vehicle_status、/vehicle_cmd和/trajectory`等消息，其中`/vehicle_status`消息包括车辆状态信息（例如位置、速度、方向等），`/vehicle_cmd`消息包括车辆控制指令（例如方向盘转角、油门踏板位置、刹车踏板位置等），`/trajectory`消息包括规划的车辆轨迹。通过对这些消息的处理，mpc\_follower节点可以计算出最优的车辆控制指令，并将其发送给`/vehicle_cmd`话题，从而实现对车辆的控制。


在实现MPC控制的过程中，需要定义车辆的动态模型、代价函数以及约束条件等。可以通过编辑`mpc_param.yaml`文件来配置MPC控制的参数。


![在这里插入图片描述](https://img-blog.csdnimg.cn/948f9d190d674a80834528f56b2c11df.png)


mpc\_follower\_core.h



```
#include <vector>
#include <iostream>
#include <limits>
#include <chrono>
#include <unistd.h>
#include <deque>

#include <ros/ros.h>
#include <std\_msgs/Float64.h>
#include <std\_msgs/Float32.h>
#include <std\_msgs/Float64MultiArray.h>
#include <geometry\_msgs/PoseStamped.h>
#include <geometry\_msgs/TwistStamped.h>
#include <visualization\_msgs/MarkerArray.h>
#include <visualization\_msgs/Marker.h>
#include <tf2/utils.h>

#include <eigen3/Eigen/Core>
#include <eigen3/Eigen/LU>

#include <autoware\_msgs/ControlCommandStamped.h>
#include <autoware\_msgs/Lane.h>
#include <autoware\_msgs/VehicleStatus.h>

#include "mpc\_follower/mpc\_utils.h"
#include "mpc\_follower/mpc\_trajectory.h"
#include "mpc\_follower/lowpass\_filter.h"
#include "mpc\_follower/vehicle\_model/vehicle\_model\_bicycle\_kinematics.h"
#include "mpc\_follower/vehicle\_model/vehicle\_model\_bicycle\_dynamics.h"
#include "mpc\_follower/vehicle\_model/vehicle\_model\_bicycle\_kinematics\_no\_delay.h"
#include "mpc\_follower/qp\_solver/qp\_solver\_unconstr.h"
#include "mpc\_follower/qp\_solver/qp\_solver\_unconstr\_fast.h"
#include "mpc\_follower/qp\_solver/qp\_solver\_qpoases.h"

/\*\* 
 \* @class MPC-based waypoints follower class
 \* @brief calculate control command to follow reference waypoints
 \*/
class MPCFollower
{
public:
  /\*\*
 \* @brief constructor
 \*/
  MPCFollower();

  /\*\*
 \* @brief destructor
 \*/
  ~MPCFollower();

private:
  ros::NodeHandle nh_;                    //!< @brief ros node handle
  ros::NodeHandle pnh_;                   //!< @brief private ros node handle
  ros::Publisher pub_steer_vel_ctrl_cmd_; //!< @brief topic publisher for control command
  ros::Publisher pub_twist_cmd_;          //!< @brief topic publisher for twist command
  ros::Subscriber sub_ref_path_;          //!< @brief topic subscriber for reference waypoints
  ros::Subscriber sub_pose_;              //!< @brief subscriber for current pose
  ros::Subscriber sub_vehicle_status_;    //!< @brief subscriber for currrent vehicle status
  ros::Timer timer_control_;              //!< @brief timer for control command computation

  MPCTrajectory ref_traj_;                                   //!< @brief reference trajectory to be followed
  Butterworth2dFilter lpf_steering_cmd_;                     //!< @brief lowpass filter for steering command
  Butterworth2dFilter lpf_lateral_error_;                    //!< @brief lowpass filter for lateral error to calculate derivatie
  Butterworth2dFilter lpf_yaw_error_;                        //!< @brief lowpass filter for heading error to calculate derivatie
  autoware_msgs::Lane current_waypoints_;                    //!< @brief current waypoints to be followed
  std::shared_ptr<VehicleModelInterface> vehicle_model_ptr_; //!< @brief vehicle model for MPC
  std::string vehicle_model_type_;                           //!< @brief vehicle model type for MPC
  std::shared_ptr<QPSolverInterface> qpsolver_ptr_;          //!< @brief qp solver for MPC
  std::string output_interface_;                             //!< @brief output command type
  std::deque<double> input_buffer_;                          //!< @brief control input (mpc\_output) buffer for delay time conpemsation

  /\* parameters for control\*/
  double ctrl_period_;              //!< @brief control frequency [s]
  double steering_lpf_cutoff_hz_;   //!< @brief cutoff frequency of lowpass filter for steering command [Hz]
  double admisible_position_error_; //!< @brief stop MPC calculation when lateral error is large than this value [m]
  double admisible_yaw_error_deg_;  //!< @brief stop MPC calculation when heading error is large than this value [deg]
  double steer_lim_deg_;            //!< @brief steering command limit [rad]
  double wheelbase_;                //!< @brief vehicle wheelbase length [m] to convert steering angle to angular velocity

  /\* parameters for path smoothing \*/
  bool enable_path_smoothing_;     //< @brief flag for path smoothing
  bool enable_yaw_recalculation_;  //< @brief flag for recalculation of yaw angle after resampling
  int path_filter_moving_ave_num_; //< @brief param of moving average filter for path smoothing
  int path_smoothing_times_;       //< @brief number of times of applying path smoothing filter
  int curvature_smoothing_num_;    //< @brief point-to-point index distance used in curvature calculation
  double traj_resample_dist_;      //< @brief path resampling interval [m]

  struct MPCParam
  {
    int prediction_horizon;                         //< @brief prediction horizon step
    double prediction_sampling_time;                //< @brief prediction horizon period
    double weight_lat_error;                        //< @brief lateral error weight in matrix Q
    double weight_heading_error;                    //< @brief heading error weight in matrix Q
    double weight_heading_error_squared_vel_coeff;  //< @brief heading error \* velocity weight in matrix Q
    double weight_steering_input;                   //< @brief steering error weight in matrix R
    double weight_steering_input_squared_vel_coeff; //< @brief steering error \* velocity weight in matrix R
    double weight_lat_jerk;                         //< @brief lateral jerk weight in matrix R
    double weight_terminal_lat_error;               //< @brief terminal lateral error weight in matrix Q
    double weight_terminal_heading_error;           //< @brief terminal heading error weight in matrix Q
    double zero_ff_steer_deg;                       //< @brief threshold that feed-forward angle becomes zero
    double delay_compensation_time;                //< @brief delay time for steering input to be compensated
  };
  MPCParam mpc_param_; // for mpc design parameter

  struct VehicleStatus
  {
    std_msgs::Header header;    //< @brief header
    geometry_msgs::Pose pose;   //< @brief vehicle pose
    geometry_msgs::Twist twist; //< @brief vehicle velocity
    double tire_angle_rad;      //< @brief vehicle tire angle
  };
  VehicleStatus vehicle_status_; //< @brief vehicle status

  double steer_cmd_prev_;     //< @brief steering command calculated in previous period
  double lateral_error_prev_; //< @brief previous lateral error for derivative
  double yaw_error_prev_;     //< @brief previous lateral error for derivative

  /\* flags \*/
  bool my_position_ok_; //< @brief flag for validity of current pose
  bool my_velocity_ok_; //< @brief flag for validity of current velocity
  bool my_steering_ok_; //< @brief flag for validity of steering angle

  /\*\*
 \* @brief compute and publish control command for path follow with a constant control period
 \*/
  void timerCallback(const ros::TimerEvent &);

  /\*\*
 \* @brief set current\_waypoints\_ with receved message
 \*/
  void callbackRefPath(const autoware_msgs::Lane::ConstPtr &);

  /\*\*
 \* @brief set vehicle\_status\_.pose with receved message 
 \*/
  void callbackPose(const geometry_msgs::PoseStamped::ConstPtr &);

  /\*\*
 \* @brief set vehicle\_status\_.twist and vehicle\_status\_.tire\_angle\_rad with receved message
 \*/
  void callbackVehicleStatus(const autoware_msgs::VehicleStatus &msg);

  /\*\*
 \* @brief publish control command calculated by MPC
 \* @param [in] vel\_cmd velocity command [m/s] for vehicle control
 \* @param [in] acc\_cmd acceleration command [m/s2] for vehicle control
 \* @param [in] steer\_cmd steering angle command [rad] for vehicle control
 \* @param [in] steer\_vel\_cmd steering angle speed [rad/s] for vehicle control
 \*/
  void publishControlCommands(const double &vel_cmd, const double &acc_cmd,
                              const double &steer_cmd, const double &steer_vel_cmd);

  /\*\*
 \* @brief publish control command as geometry\_msgs/TwistStamped type
 \* @param [in] vel\_cmd velocity command [m/s] for vehicle control
 \* @param [in] omega\_cmd angular velocity command [rad/s] for vehicle control
 \*/
  void publishTwist(const double &vel_cmd, const double &omega_cmd);

  /\*\*
 \* @brief publish control command as autoware\_msgs/ControlCommand type
 \* @param [in] vel\_cmd velocity command [m/s] for vehicle control
 \* @param [in] acc\_cmd acceleration command [m/s2] for vehicle control
 \* @param [in] steer\_cmd steering angle command [rad] for vehicle control
 \*/
  void publishCtrlCmd(const double &vel_cmd, const double &acc_cmd, const double &steer_cmd);

  /\*\*
 \* @brief calculate control command by MPC algorithm
 \* @param [out] vel\_cmd velocity command
 \* @param [out] acc\_cmd acceleration command
 \* @param [out] steer\_cmd steering command
 \* @param [out] steer\_vel\_cmd steering rotation speed command
 \*/
  bool calculateMPC(double &vel_cmd, double &acc_cmd, double &steer_cmd, double &steer_vel_cmd);

  /\* debug \*/
  bool show_debug_info_;      //!< @brief flag to display debug info

  ros::Publisher pub_debug_filtered_traj_;        //!< @brief publisher for debug info
  ros::Publisher pub_debug_predicted_traj_;       //!< @brief publisher for debug info
  ros::Publisher pub_debug_values_;               //!< @brief publisher for debug info
  ros::Publisher pub_debug_mpc_calc_time_;        //!< @brief publisher for debug info

  ros::Subscriber sub_estimate_twist_;         //!< @brief subscriber for /estimate\_twist for debug
  geometry_msgs::TwistStamped estimate_twist_; //!< @brief received /estimate\_twist for debug

  /\*\*
 \* @brief convert MPCTraj to visualizaton marker for visualization
 \*/
  void convertTrajToMarker(const MPCTrajectory &traj, visualization_msgs::Marker &markers,
                           std::string ns, double r, double g, double b, double z);

  /\*\*
 \* @brief callback for estimate twist for debug
 \*/
  void callbackEstimateTwist(const geometry_msgs::TwistStamped &msg) { estimate_twist_ = msg; }
};

```

以上。





