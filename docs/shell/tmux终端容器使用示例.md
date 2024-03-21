### 本地启动
```sh
# remote_run.sh
session="router"

window1="info_router"
window2="log_monitor"

if ! tmux list-sessions | grep -q $session; then
    tmux new-session -d -t router
fi

# info_router
if ! tmux list-windows -t $session | grep -q $window1; then
    # 如果窗口不存在，则创建新窗口
    echo "Create new window: $window1"
    tmux new-window -t $session -n $window1
    tmux send-keys -t router:$window1 "source devel/setup.bash" C-m
    tmux send-keys -t router:$window1 "rosrun n_info_router n_info_router _local_port:=8881 _local_id:=1001 _local_heart_port:=8891 truck_1" C-m
else
    echo "Window '$window1' already exists in session '$session'"
    tmux send-keys -t router:$window1 "source devel/setup.bash" C-m
    tmux send-keys -t router:$window1 "rosrun n_info_router n_info_router _local_port:=8881 _local_id:=1001 _local_heart_port:=8891 truck_1" C-m
fi

# log_monitor
if ! tmux list-windows -t $session | grep -q $window2; then
    # 如果窗口不存在，则创建新窗口
    echo "Create new window: $window2"
    tmux new-window -t $session -n $window2
    tmux send-keys -t router:$window2 "python3 log_monitor.py" C-m
else
    echo "Window '$window2' already exists in session '$session'"
    tmux send-keys -t router:$window2 "python3 log_monitor.py" C-m
fi
```

### 本地关闭
```sh
# remote_stop.sh
session="router"

window1="info_router"
window2="log_monitor"

if ! tmux list-sessions | grep -q $session; then
    tmux new-session -d -t router
fi

# info_router
if ! tmux list-windows -t $session | grep -q $window1; then
    # 如果窗口不存在，则创建新窗口
    tmux new-window -t $session -n $window1
    tmux send-keys -t router:$window1 C-c
else
    echo "Window '$window1' already exists in session '$session'"
    tmux send-keys -t router:$window1 C-c
fi

# log_monitor
if ! tmux list-windows -t $session | grep -q $window2; then
    # 如果窗口不存在，则创建新窗口
    tmux new-window -t $session -n $window2
    tmux send-keys -t router:$window2 C-c
else
    echo "Window '$window2' already exists in session '$session'"
    tmux send-keys -t router:$window2 C-c
fi
```

### 远程编译启动
```sh
# deploy_build_run.sh
#!/bin/bash

# 定义远程服务器的用户名和地址
declare -a servers=(
    # "nvidia@10.10.6.100 A0"
    # "nvidia@10.10.6.110 A1"
    "nvidia@10.10.6.120 A2"
    # "nvidia@10.10.6.130 A3"
    # "nvidia@10.10.6.140 A4"
    "nvidia@10.10.6.150 A5"
    # "nvidia@10.10.6.160 A6"
    "nvidia@10.10.6.170 A7"
    # "nvidia@10.10.6.180 A8"
    # "ubuntu@10.10.8.56 A9"
)

declare -a password="nvidia" # 对应服务器的密码
# declare -a password="wangzf" # test

# 本地 ZIP 文件路径
remote_file="sensor_roadside_info_router"
local_zip_file="sensor_roadside_info_router.zip"
local_file_path="${HOME}/sensor_roadside_info_router"
cd $local_file_path

# 远程目录路径
remote_dir="~/"
# remote_dir="~"

# 循环遍历所有服务器
for server in "${servers[@]}"
do
    end=${#server}
    start=`expr index "$server" A`
    start=`expr $start - 1`
    substring=${server:start:end}
    echo $substring
    sed -i "s/ A./ $substring/g" src/config/n_info_router.ini
    
    end=`expr $start - 1`
    zip -rq $local_zip_file *.py *.sh src/ -x "deploy*"
    # 将文件传输到远程服务器
    sshpass -p "${password}" scp "${local_zip_file}" "${server:0:end}:${remote_dir}"

    # 连接到服务器，使用 tmux 创建新会话进行操作
    sshpass -p "${password}" ssh -t "${server:0:end}" bash -c "'
        # 解压文件
        unzip -oq ${local_zip_file} -d ${remote_file}
        rm $local_zip_file

        cd ${remote_file}
        source /opt/ros/noetic/setup.sh
        bash build.sh
        bash remote_stop.sh
        bash remote_run.sh
        sleep 3
        rm -r src/nodes
    '"
    echo "Completed processing ${server:0:end}"
done

rm $local_zip_file
echo "All done."
```

### 远程启动
```sh
# deploy_run.sh
#!/bin/bash

# 定义远程服务器的用户名和地址
declare -a servers=(
    "nvidia@10.10.6.100"
    "nvidia@10.10.6.110"
    # "nvidia@10.10.6.120"
    "nvidia@10.10.6.130"
    "nvidia@10.10.6.140"
    # "nvidia@10.10.6.150"
    "nvidia@10.10.6.160"
    "nvidia@10.10.6.170"
    "nvidia@10.10.6.180"
)

# declare -a servers=(
#     "ubuntu@10.10.8.56"
# )

declare -a password="nvidia"
# declare -a password="wangzf" # 对应服务器的密码

# 本地 ZIP 文件路径
remote_file="sensor_roadside_info_router"

# 循环遍历所有服务器
for server in "${servers[@]}"
do
    echo "Processing ${server}..."

    # 连接到服务器，使用 tmux 创建新会话进行操作
    sshpass -p "${password}" ssh -t "${server}" bash -c "'
        cd ${remote_file}
        bash remote_run.sh
    '"
    echo "Completed processing ${server}"
done

echo "All done."
```

### 远程关闭
```sh
# deploy_stop.sh
#!/bin/bash

# 定义远程服务器的用户名和地址
declare -a servers=(
    "nvidia@10.10.6.100"
    "nvidia@10.10.6.110"
    "nvidia@10.10.6.120"
    "nvidia@10.10.6.130"
    "nvidia@10.10.6.140"
    "nvidia@10.10.6.150"
    "nvidia@10.10.6.160"
    "nvidia@10.10.6.170"
    "nvidia@10.10.6.180"
)

# declare -a servers=(
#     "ubuntu@10.10.8.56"
# )

declare -a password="nvidia"
# declare -a password="wangzf" # 对应服务器的密码

# 本地 ZIP 文件路径
remote_file="sensor_roadside_info_router"

# 循环遍历所有服务器
for server in "${servers[@]}"
do
    echo "Processing ${server}..."

    # 连接到服务器，使用 tmux 创建新会话进行操作
    sshpass -p "${password}" ssh -t "${server}" bash -c "'
        cd ${remote_file}
        bash remote_stop.sh
    '"
    echo "Completed processing ${server}"
done

echo "All done."
```

### 一键添加密钥
```sh
# add_hosts.sh
#!/bin/bash

# 定义主机 IP 地址数组
declare -a hosts=("10.10.6.100" "10.10.6.110" "10.10.6.120" "10.10.6.130" 
                  "10.10.6.140" "10.10.6.150" "10.10.6.160" "10.10.6.170" "10.10.6.180")

# 循环遍历数组中的每个主机 IP 地址
for host in "${hosts[@]}"
do
    echo "Adding SSH key for ${host} to known_hosts..."
    ssh-keyscan -H "${host}" >> ~/.ssh/known_hosts # 获取 SSH 公钥
done

echo "All keys have been added."
```