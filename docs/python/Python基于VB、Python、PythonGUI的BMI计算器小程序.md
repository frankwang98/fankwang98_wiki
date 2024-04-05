






昨天做了一个基于Labview的BMI计算器，想着既然Labview能做，其他编程语言行不行呢，说干就干！


首先，这两天我妹在学VB（学校的课程），因为我当时直接接触的C，并不了解这门语言，然后百度了一下，是这个样子的。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174030715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 总之，这是一门具有用户图形界面（GUI）和可以快速开发应用程序的编程语言，然后用它开发一个BMI计算的小程序效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174251766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 **BMI-VB代码如下（供参考）：**



```
Private Sub Command1_Click()  '计算BMI值

Dim sg As Double  '身高
Dim tz As Double  '体重
Dim jg As Double  '结果

sg = Val(Text1)
tz = Val(Text2)
jg = tz / ((sg / 100) ^ 2)

Select Case jg

Case ls < 18.5
Label3.Caption = "您的BMI值结果为：" & Format(jg, "00.00") & vbCrLf & "过轻，请合理添加膳食，补充营养！"
Case 18.5 To 23.9
Label3.Caption = "您的BMI值结果为：" & Format(jg, "00.00") & vbCrLf & "正常，请注意保持！"
Case 24 To 27
Label3.Caption = "您的BMI值结果为：" & Format(jg, "00.00") & vbCrLf & "重了，请注意！"
Case 28 To 32
Label3.Caption = "您的BMI值结果为：" & Format(jg, "00.00") & vbCrLf & "胖了，请多加运动，保持身体健康！"
Case ls > 32
Label3.Caption = "您的BMI值结果为：" & Format(jg, "00.00") & vbCrLf & "喝水都胖，我也很无奈！"

End Select
End Sub


Private Sub Command2_Click()
End
End Sub


```

用VB写完还不过瘾，那就再用当前最流行的Python来做一下吧，首先，用python的命令行来显示结果的效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425174940839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 **BMI-Python代码如下：**



```
# 声明变量 身高、体重
while True:
    height = input('请输入您的身高（cm）:')
    weight = input('清输入您的体重（kg）:')
    # 将输入的身高体重转换为小数float类型
    if height == '0' or weight == '0':
        print('您输入的数据有误，程序已结束')
        break
    height = float(height)
    weight = float(weight)
    # 体质指数（BMI）=体重（kg）/（身高/100）^2（cm）
    bmi = weight / ((height/100)\*\*2)
    print('您的BMI指数为：',bmi)
    '''
 过轻：低于18.5
 正常：18.5-23.9
 过重：24-27
 肥胖：28-32
 非常肥胖：高于32
 '''
    if bmi < 18.5:
        print('体重过轻，请合理添加膳食，补充营养！')
    elif 18.5 <= bmi <= 23.9:
        print('标准身材，注意保持！')
    elif 24 <= bmi <= 27:
        print('过重，请注意运动！')
    elif 28 <= bmi <= 32:
        print('过胖，请多加运动，保持身体健康！')
    else:
        print('喝水都胖，怕了怕了！可以学村上春树一样坚持跑步！')

```

另外一种，是需要调用python的第三方库PySimpleGUI，运行效果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210425175342680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 **BMI-PythonGUI代码如下：**



```
import PySimpleGUI as sg

def calc\_bmi(h, w):
    try:
        h, w = float(h), float(w)
        bmi = round(w / (h/100) \*\* 2, 1)
        if bmi < 18.5:
            standard = '体重过轻，请合理添加膳食，补充营养！'
        elif 18.5 <= bmi <= 23.9:
            standard = '标准身材，注意保持！'
        elif 24.0 <= bmi <= 27.9:
            standard = '过重，请注意运动！'
        elif 28.0 <= bmi <= 32.0:
            standard = '过胖，请多加运动，保持身体健康！'
        else:
            standard = '喝水都胖，怕了怕了！可以学村上春树一样坚持跑步！'
    except (ValueError, ZeroDivisionError):
        return None
    else:
        return f'BMI: {bmi}, {standard}'


layout = [[sg.Text('身高(单位：cm )'), sg.InputText(size=(15,))],
          [sg.Text('体重(单位：kg)'), sg.InputText(size=(15,))],
          [sg.Button('计算 BMI', key='submit')],
          [sg.Text('', key='bmi', size=(20, 2))],
          [sg.Quit('退出', key='q')]]

window = sg.Window('基于Python的BMI计算', layout, font='微软雅黑')

while True:
    event, value = window.Read()
    if event == 'submit':
        bmi = calc_bmi(value[0], value[1])
        if bmi:
            window.Element('bmi').Update(bmi, text_color='black')
        else:
            window.Element('bmi').Update('输入有误！', text_color='red')
    elif event == 'q':
        break

window.Close()



```

以上就是今天分享的全部内容了，喜欢的话记得点赞哦！





