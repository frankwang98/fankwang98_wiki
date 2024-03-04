







> 
> 遇到一个问题，需要将excel表格的数据上下翻转，不是升序或者降序，不然就不需要程序来实现了。网上也看了有些插件有这个功能，但插件过于老旧，下载都有问题。
> 
> 
> 


记录一下程序实现的过程：


首先创建`example.xlsx`，如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/1486a6f0e7804a91a734ce0fcd8dcd1d.png)


然后创建`test.py`程序：


用`openpyxl`来读入xlsx表格数据，将数据存入一个二位列表，并对其翻转，然后将翻转后的数据重新写入文件。



```
from openpyxl import load_workbook

# 打开 Excel 文件并获取工作簿和工作表对象
workbook = load_workbook(filename='example.xlsx')
worksheet = workbook.active

# 将数据按列读取到一个二维列表中
data = []
for row in worksheet.iter_rows():
    row_data = []
    for cell in row:
        row_data.append(cell.value)
    data.append(row_data)

# 将列表上下翻转
data_reversed = data[::-1]

# 将翻转后的数据写回 Excel 文件中
for i, row in enumerate(data_reversed):
    for j, value in enumerate(row):
        worksheet.cell(row=i+1, column=j+1, value=value)

# 保存并关闭 Excel 文件
workbook.save(filename='example\_reversed.xlsx')

```

此外，需要安装包：`pip install openpyxl`


最后，结果如下：


![在这里插入图片描述](https://img-blog.csdnimg.cn/d2a31035b145479db1010e40c789ed19.png)


以上。





