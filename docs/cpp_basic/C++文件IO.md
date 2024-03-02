下面是文件读写操作的简单示例应用。

### 1.将数据写入文件

将数据写入文件需要加入`fstream`这个头文件，然后定义`ofstream`对象，用起来的话就跟cout差不多，不同的就是需要有一个`open`和`close`的操作。

```cpp
// c++写数据到文件中
#include <iostream>
#include <vector>
#include <fstream>
using namespace std;

#define num 3

int main()
{
	ofstream write;
	write.open("test.txt",ios::app);

	vector<string> vec(num);
	vec[0] = "hello";
	vec[1] = "world";
	vec[2] = '!';

	for(int i = 0; i < num; i++)
	{
		write << vec[i];
		write << endl;
	}

	write.close();
	cout << "文件写入完成\n";
	return 0;
}

```

### 2.从文件中读取数据

读取数据也是用到`fstream`这个头文件，定义`ifstream`对象，这里只需要执行`open`操作即可，然后用`getline`去读取数据。

```cpp
// c++从文件中读取数据
#include <iostream>
#include <vector>
#include <fstream>
using namespace std;

int main()
{
	ifstream read;
	read.open("test.txt", ios::in);
	if (!read.is\_open())
	{
		cout << "读取文件失败" << endl;
		return 0;
	}

	string buf;
	while (getline(read,buf))
	{
		cout << buf << endl;
	}
	return 0;
}

```

以上。