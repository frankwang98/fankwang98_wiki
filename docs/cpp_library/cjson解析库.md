> 😏*★,°*:.☆(￣▽￣)/$:*.°★* 😏  
>  这篇文章主要介绍基于cjson库的json数据处理。  
>  **无专精则不能成，无涉猎则不能通。——梁启超**  
>  欢迎来到我的博客，一起学习，共同进步。  
>  喜欢的朋友可以关注一下，下次更新不迷路🥞

### 😏1. cjson介绍

项目Github地址：`https://github.com/DaveGamble/cJSON`

`cJSON`是一个轻量级的、用于C语言的JSON解析和生成库。它提供了一组简单易用的API，可以方便地将JSON数据转换为C语言中的数据结构，并能将C语言中的数据结构转换为JSON格式。

以下是cJSON库的一些特点和功能：

> 1. 轻量级：cJSON库的代码量较小，没有复杂的依赖关系，适合嵌入式系统或资源受限的环境使用。

> 2. 易于使用：cJSON库提供了简单易懂的API，可以方便地解析和生成JSON数据。

> 3. 解析功能：cJSON库可以将JSON字符串解析为C语言中的数据结构，包括对象、数组、字符串、数字等。您可以使用API函数来获取和修改JSON中的数据。

> 4. 生成功能：cJSON库可以根据C语言中的数据结构生成对应的JSON字符串。您可以使用API函数创建对象、数组，添加键值对，设置属性等。

> 5. 内存管理：cJSON库提供了内存管理功能，可以动态分配和释放内存，避免内存泄漏和溢出问题。

> 6. 跨平台支持：cJSON库在不同平台上都有很好的兼容性，可以在多种操作系统和编译器环境下使用。

### 😊2. 环境配置

在工程中，只需要包含两个文件即可：

### 😆3. 使用说明

#### 解析json数据

```cpp
#include <iostream>
extern "C" {
	#include <stdio.h>
	#include "cJSON.h"
}

// 定义一个Json数据
char *message =
"{                              \
    \"name\":\"XiaoMing\",   \
    \"age\": 22,                \
    \"weight\": 60,           \
    \"address\":                \
        {                       \
            \"country\": \"China\",\
            \"zip-code\": 111111\
        },                      \
    \"skill\": [\"c\", \"Java\", \"Python\"],\
    \"student\": false          \
}";

int main(void)
{
	// 实例化
	cJSON* cjson_test = NULL;
	cJSON* cjson_name = NULL;
	cJSON* cjson_age = NULL;
	cJSON* cjson_weight = NULL;
	cJSON* cjson_address = NULL;
	cJSON* cjson_address_country = NULL;
	cJSON* cjson_address_zipcode = NULL;
	cJSON* cjson_skill = NULL;
	cJSON* cjson_student = NULL;
	int    skill_array_size = 0, i = 0;
	cJSON* cjson_skill_item = NULL;

	// 解析整段Json数据
	cjson_test = cJSON_Parse(message);
	if (cjson_test == NULL)
	{
		printf("parse fail.\n");
		return -1;
	}

	// 依次根据名称提取JSON数据（键值对）
	cjson_name = cJSON_GetObjectItem(cjson_test, "name");
	cjson_age = cJSON_GetObjectItem(cjson_test, "age");
	cjson_weight = cJSON_GetObjectItem(cjson_test, "weight");

	printf("name: %s\n", cjson_name->valuestring);
	printf("age:%d\n", cjson_age->valueint);
	printf("weight:%.1f\n", cjson_weight->valuedouble);

	// 解析嵌套json数据 
	cjson_address = cJSON_GetObjectItem(cjson_test, "address");
	cjson_address_country = cJSON_GetObjectItem(cjson_address, "country");
	cjson_address_zipcode = cJSON_GetObjectItem(cjson_address, "zip-code");
	printf("address-country:%s\naddress-zipcode:%d\n", cjson_address_country->valuestring, cjson_address_zipcode->valueint);

	// 解析数组 
	cjson_skill = cJSON_GetObjectItem(cjson_test, "skill");
	skill_array_size = cJSON_GetArraySize(cjson_skill);
	printf("skill:[");
	for (i = 0; i < skill_array_size; i++)
	{
		cjson_skill_item = cJSON_GetArrayItem(cjson_skill, i);
		printf("%s,", cjson_skill_item->valuestring);
	}
	printf("\b]\n");

	// 解析布尔型数据
	cjson_student = cJSON_GetObjectItem(cjson_test, "student");
	if (cjson_student->valueint == 0)
	{
		printf("student: false\n");
	}
	else
	{
		printf("student:error\n");
	}

	return 0;
}

```

#### 封装json数据

```cpp
#include <iostream>
extern "C" {
#include <stdio.h>
#include "cJSON.h"
}

int main(void)
{
	cJSON* cjson_test = NULL;
	cJSON* cjson_address = NULL;
	cJSON* cjson_skill = NULL;
	char* str = NULL;

	/* 创建一个JSON数据对象(链表头结点) */
	cjson_test = cJSON_CreateObject();

	/* 添加一条字符串类型的JSON数据(添加一个链表节点) */
	cJSON_AddStringToObject(cjson_test, "name", "XiaoMing");

	/* 添加一条整数类型的JSON数据(添加一个链表节点) */
	cJSON_AddNumberToObject(cjson_test, "age", 22);

	/* 添加一条浮点类型的JSON数据(添加一个链表节点) */
	//cJSON_AddNumberToObject(cjson_test, "weight", 60); //提示找不到标识符

	/* 添加一个嵌套的JSON数据（添加一个链表节点） */
	cjson_address = cJSON_CreateObject();
	cJSON_AddStringToObject(cjson_address, "country", "China");
	//cJSON_AddNumberToObject(cjson_address, "zip-code", 111111);
	cJSON_AddItemToObject(cjson_test, "address", cjson_address);

	/* 添加一个数组类型的JSON数据(添加一个链表节点) */
	cjson_skill = cJSON_CreateArray();
	cJSON_AddItemToArray(cjson_skill, cJSON_CreateString("C"));
	cJSON_AddItemToArray(cjson_skill, cJSON_CreateString("Java"));
	cJSON_AddItemToArray(cjson_skill, cJSON_CreateString("Python"));
	cJSON_AddItemToObject(cjson_test, "skill", cjson_skill);

	/* 添加一个值为 False 的布尔类型的JSON数据(添加一个链表节点) */
	cJSON_AddFalseToObject(cjson_test, "student");

	/* 打印JSON对象(整条链表)的所有数据 */
	str = cJSON_Print(cjson_test);
	printf("%sn", str);

	return 0;
}

```

#### 从文件中解析json

```cpp
#include <stdio.h> 
#include <string.h> 
#include <sys/types.h> 
#include <stdlib.h> 
#include <unistd.h> 
#include<sys/stat.h> //stat
#include "cJSON.h" 
#define FILENAME "./map.json"

typedef struct 
{  
    int x;
    int y;
}arr;  
 

//解析一个结构数组 
int cJSON_to_struct_array(char *text, arr worker[])  
{  
    cJSON *json,*arrayItem,*item,*object;  
    int i = 0;  
 
    json=cJSON_Parse(text);     //parse
    if (!json)  
    {  
        printf("Error before: [%s]n",cJSON_GetErrorPtr());  
    }  
    else 
    {  
        arrayItem=cJSON_GetObjectItem(json,"arr");  // 获取json对象中的arr节点
        if(arrayItem!=NULL)  
        {  
            int size=cJSON_GetArraySize(arrayItem);  //获取数组大小
            printf("cJSON_GetArraySize: size=%dn",size);  
 
            for(i=0;i<size;i++)  
            {  
                printf("i=%dn",i);  
                object=cJSON_GetArrayItem(arrayItem,i);  
 
                item=cJSON_GetObjectItem(object,"x"); // 获取json对象中的firstName节点
                if(item!=NULL)  
                {  
                    printf("cJSON_GetObjectItem: type=%d, valueint is %dn",item->type,item->valueint);  
                    memcpy(worker[i].x,item->valueint,strlen(item->valueint));  
                }  
                else 
                {  
                    printf("cJSON_GetObjectItem: get age failedn");  
                }  
 
                item=cJSON_GetObjectItem(object,"y");  
                if(item!=NULL)  
                {  
                    printf("cJSON_GetObjectItem: type=%d, valueint=%dn",item->type,item->valueint);  
                    worker[i].y=item->valueint;  
                }  
                else 
                {  
                    printf("cJSON_GetObjectItem: get age failedn");  
                }  
            }  
        }  
  
    printf("nn");
        for(i=0;  i<sizeof(worker) / sizeof(worker[0]) ;i++)    //获取数组长度
        {  
            printf("i=%d",i);
            printf("x=%d,y=%dn",  
                    worker[i].x,  
                    worker[i].y);
        }  
 
        cJSON_Delete(json);  
    }  
    return 0;  
}  
 
 // 文件大小
size_t get_file_size(const char *filepath)
{
   
    if(NULL == filepath)
        return 0;
    struct stat filestat;
    memset(&filestat,0,sizeof(struct stat));
    /*获取文件信息*/
    if(0 == stat(filepath,&filestat))
        return filestat.st_size;
    else
        return 0;
}

// 读取文件
void read_file(char *filename)  
{  
     FILE *fp;  

     arr worker[4]={{0}};  

  /*get file size*/
    size_t size = get_file_size(filename);
    if(0 == size)
    {
        printf("get_file_size failed!!!n");
    }
        
    /*malloc memory*/
    char *buf = malloc(size+1);
    if(NULL == buf)
    {
        printf("malloc failed!!!n");
        
    }
    memset(buf,0,size+1);

    fp=fopen(filename,"rb");  

    fread(buf,1,size,fp);  
    fclose(fp);  
 
    printf("read file %s complete, size=%d.n",filename,size);  

    cJSON_to_struct_array(buf, worker);  
 
    free(buf);  
}  

// main
int main(int argc, char **argv)  
{  
 
    read_file(FILENAME);  

    // // 实例化
	// cJSON* cjson_x = NULL;
	// cJSON* cjson_y = NULL;

    // // 依次根据名称提取JSON数据（键值对）
	// cjson_x = cJSON_GetObjectItem(cjson_x, "x");
	// cjson_y = cJSON_GetObjectItem(cjson_y, "y");

    // printf("x:%dn", cjson_x->valueint);
    // printf("y:%dn", cjson_y->valueint);
 
    return 0;  
}  

```

#### json数据写入文件

```cpp
#include <iostream>
#include <fstream>
#include "cJSON.h"

int main()
{
    // 创建JSON对象并设置数据
    cJSON* root = cJSON_CreateObject();
    cJSON_AddStringToObject(root, "name", "John");
    cJSON_AddNumberToObject(root, "age", 30);
    cJSON_AddStringToObject(root, "city", "New York");

    // 生成JSON字符串
    char* json_str = cJSON_Print(root);

    // 打开文件
    std::ofstream file("data.json");

    if (file.is_open()) {
        // 将JSON字符串写入文件
        file << json_str;

        // 关闭文件
        file.close();

        std::cout << "JSON data has been written to file." << std::endl;
    }
    else {
        std::cerr << "Unable to open file for writing." << std::endl;
    }

    // 释放内存
    cJSON_Delete(root);
    free(json_str);

    return 0;
}

```

以上。