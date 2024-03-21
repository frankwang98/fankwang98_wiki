







> 
> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍BMI身体质量指数计算工具的Java实现。  
>  **学其所用，用其所学。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞
> 
> 
> 




#### 文章目录


* + [:smirk:1. 知识介绍](#smirk1__7)
	+ [:blush:2. Java终端程序](#blush2_Java_24)
	+ [:satisfied:3. Java-Swing界面程序](#satisfied3_JavaSwing_86)
	+ [:satisfied:4. Java程序打包成jar](#satisfied4_Javajar_175)




### 😏1. 知识介绍


`BMI`（Body Mass Index，身体质量指数），也称为体重指数，是一种常用的衡量成人人体肥胖程度的指标。它通过身高和体重之间的数值关系来评估一个人的体重是否适中。


BMI的计算公式如下：



```
BMI = 体重（kg）/ (身高（m） * 身高（m）)

```

根据计算得到的BMI值，可以将人体的体重状况分为以下几个范围：



```
BMI < 18.5：体重过轻
18.5 <= BMI < 24：体重正常
24 <= BMI < 28：超重
BMI >= 28：肥胖

```

### 😊2. Java终端程序


![在这里插入图片描述](https://img-blog.csdnimg.cn/5967c1d275c84c3d998df7264767143e.png)



```
package org.example;

import java.util.Scanner;

public class Main {
    /\*
 \* main函数是程序的入口函数
 \*/
    public static void main(String[] args) {

        Scanner input = new Scanner(System.in);

        System.out.println("欢迎使用BMI计算器");
        System.out.print("请输入您的体重（kg）：");
        double weight = input.nextDouble();

        System.out.print("请输入您的身高（m）：");
        double height = input.nextDouble();

        double bmi = calculateBMI(weight, height);
        System.out.println("您的BMI指数为：" + bmi);
        System.out.println("BMI指数结果解读：");
        interpretBMI(bmi);

        input.close();

    }

    /\*\*
 \* BMI计算
 \* @param weight
 \* @param height
 \* @return
 \*/
    public static double calculateBMI(double weight, double height) {
        return weight / (height \* height);
    }

    /\*\*
 \* BMI解读
 \* @param bmi
 \*/
    public static void interpretBMI(double bmi) {
        if (bmi < 18.5) {
            System.out.println("您的体重过轻");
        } else if (bmi < 24) {
            System.out.println("您的体重正常");
        } else if (bmi < 28) {
            System.out.println("您的体重超重");
        } else {
            System.out.println("您的体重肥胖");
        }
    }

}

```

### 😆3. Java-Swing界面程序


可以使用 `JTextFields` 用于输入体重和身高，使用`JButton` 触发计算，并使用`JLabel`显示结果。


![在这里插入图片描述](https://img-blog.csdnimg.cn/9d03b47899ae40ef9f8ebc7308bfadce.png)



```
package org.example;

import javax.swing.\*;
import java.awt.\*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.Dimension;

public class Main extends JFrame {
    private JTextField weightField;
    private JTextField heightField;
    private JLabel resultLabel;

    public Main() {
        setTitle("BMI计算器");
        setSize(675, 120);
        setDefaultCloseOperation(JFrame.EXIT\_ON\_CLOSE);

        // 获取屏幕尺寸
        Dimension screenSize = Toolkit.getDefaultToolkit().getScreenSize();
        int screenWidth = (int) screenSize.getWidth();
        int screenHeight = (int) screenSize.getHeight();

        // 计算窗口显示位置
        int x = (screenWidth - getWidth()) / 2;
        int y = (screenHeight - getHeight()) / 2;
        setLocation(x, y);

        // 创建面板
        JPanel panel = new JPanel();
        JLabel weightLabel = new JLabel("体重（kg）：");
        weightField = new JTextField(10);
        JLabel heightLabel = new JLabel("身高（m）：");
        heightField = new JTextField(10);
        JButton calculateButton = new JButton("计算");
        resultLabel = new JLabel();

        calculateButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                double weight = Double.parseDouble(weightField.getText());
                double height = Double.parseDouble(heightField.getText());
                double bmi = calculateBMI(weight, height);
                String interpretation = interpretBMI(bmi);
                resultLabel.setText("您的BMI指数为：" + bmi + "，结果解读：" + interpretation);
            }
        });

        panel.add(weightLabel);
        panel.add(weightField);
        panel.add(heightLabel);
        panel.add(heightField);
        panel.add(calculateButton);
        panel.add(resultLabel);
        add(panel);
    }

    private double calculateBMI(double weight, double height) {
        return weight / (height \* height);
    }

    private String interpretBMI(double bmi) {
        if (bmi < 18.5) {
            return "您的体重过轻";
        } else if (bmi < 24) {
            return "您的体重正常";
        } else if (bmi < 28) {
            return "您的体重超重";
        } else {
            return "您的体重肥胖";
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new Main().setVisible(true);
            }
        });
    }
}

```

### 😆4. Java程序打包成jar


在`文件-项目结构-工件`中，添加工件：


![在这里插入图片描述](https://img-blog.csdnimg.cn/b5aff0dc83884b13995f35b977560461.png)


基于模板创建`jar`：


![在这里插入图片描述](https://img-blog.csdnimg.cn/00947c5d631f45f58b6bf41ba023fc97.png)


然后构建中选择`构建工件`，就会生成`jar`包到`out`目录了。然后在终端运行即可：



```
java -jar xxx.jar

```

![请添加图片描述](https://img-blog.csdnimg.cn/5ea93bb657184b9eb8515cc76047c16a.png)


以上。





