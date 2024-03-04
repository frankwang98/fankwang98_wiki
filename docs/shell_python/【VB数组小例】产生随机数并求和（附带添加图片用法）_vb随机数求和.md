






刚学VB，最熟悉的控件就是TextBox了，下面将用9个TextBox和1个CommandButton做一个数组的小例子：产生随机数并求和。


效果如下：


![效果图](https://img-blog.csdnimg.cn/20210504140849371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMzQ0Nzkw,size_16,color_FFFFFF,t_70#pic_center)  
 前面板的控件选取就不用多说了，来说说这个添加图片的用法：首先，在窗体上添加image1控件，


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210504141244304.png#pic_center)  
 然后在它的属性Picture上设置，找到一张图片导入即可。  
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210504141449495.png#pic_center)  
 代码部分：



```
Dim i As Integer, sum As Integer
Private Sub Command1_Click()

sum = 0
For i = 0 To 8
Text1(i) = Int(Rnd * 100 + 200) '产生200-299之间的随机数
sum = sum + Text1(i)
Next
Print sum  ‘输出求和值

End Sub

```

大家赶紧去试试吧。


以上就是今天的内容，喜欢的话记得一键三连哦。





