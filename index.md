## C语言 造轮子

### 计算两个日期的天数
```cpp
int day_diff(int year_start, int month_start, int day_start, int year_end, int month_end, int day_end)
{
	int y2, m2, d2;
	int y1, m1, d1;
	
	m1 = (month_start + 9) % 12;
	y1 = year_start - m1/10;
	d1 = 365*y1 + y1/4 - y1/100 + y1/400 + (m1*306 + 5)/10 + (day_start - 1);
 
	m2 = (month_end + 9) % 12;
	y2 = year_end - m2/10;
	d2 = 365*y2 + y2/4 - y2/100 + y2/400 + (m2*306 + 5)/10 + (day_end - 1);
	
	return (d2 - d1);
}

```

### 获取当前日期

```cpp
//    printf("%d\n",p->tm_sec); /*获取当前秒*/
//    printf("%d\n",p->tm_min); /*获取当前分*/
//    printf("%d\n",8+p->tm_hour);/*获取当前时,这里获取西方的时间,刚好相差八个小时*/
//    printf("%d\n",p->tm_mday);/*获取当前月份日数,范围是1-31*/
//    printf("%d\n",1+p->tm_mon);/*获取当前月份,范围是0-11,所以要加1*/
//    printf("%d\n",1900+p->tm_year);/*获取当前年份,从1900开始，所以要加1900*/
//    printf("%d\n",p->tm_yday); /*从今年1月1日算起至今的天数，范围为0-365*/

char * get_current_date()
{
    time_t timep;
    struct tm *p;
    time (&timep);
    p=gmtime(&timep);
    
    char *result=(char * )malloc(sizeof(char)*8);
    
    sprintf(result,"%d%02d%02d",p->tm_year+1900,p->tm_mon+1,p->tm_mday);
	
    return result;
}
```
### 计算一个值中值为1的位的个数
```cpp
int count_one_bits(unsigned int)
{
	int ones;
	for(ones=0;value!=0;value=value>>2)
	{
		if(value%2!=0)
		{
			ones += 1;
		}
	}
	return ones;
}
```
### 库函数使用fmod 和 modf

```c
#include <stdio.h>
#include <math.h> //fmod modf
#include <stdbool.h> //bool 类型
int main(void)
{
	printf("hello world\n");
	float s = fmod(3.2,2.0);//浮点数求余
	printf("%f\n",s);

	double integer;
	double fraction = modf(3.2,&integer);//取一个浮点数的整数部分和小数部分
	printf("%g %g\n",integer,fraction);

	bool isbool = false;
	printf("%d\n",isbool);
	return 0;
}
```

### 冒泡排序

```c
void bubbleSort(int arr[], int len) {
	int temp;
	int i, j;
	for (i = 0; i < len - 1; i++) /* 外循环为排序趟数，len个数进行len-1趟 */
		for (j = 0; j < len - 1 - i; j++) { /* 内循环为每趟比较的次数，第i趟比较len-i次 */
			if (arr[j] > arr[j + 1]) { /* 相邻元素比较，若逆序则交换（升序为左大于右，降序反之） */
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
}
```


### 库函数perror
```c
/* perror example */
void test_perror()
{
	FILE* pFile;
	pFile = fopen("unexist.ent", "rb");
	if (pFile == NULL)
		perror("The following error occurred");//The following error occurred: No such file or directory
	else
		fclose(pFile);
}
```

### 库函数 vsnprintf
```c
#include <stdarg.h>
/* vsnprintf example */
void PrintFError(const char* format, ...)
{
	char buffer[256];
	va_list args;
	va_start(args, format);
	vsnprintf(buffer, 256, format, args);
	perror(buffer);//perror 不会覆盖掉原有的buffer,而是在其后追加错误号对应的错误信息 Error opening 'myfile.txt': No such file or directory
	va_end(args);
}

void test_vsnprintf()
{
	FILE* pFile;
	char szFileName[] = "myfile.txt";

	pFile = fopen(szFileName, "r");
	if (pFile == NULL)
		PrintFError("Error opening '%s'", szFileName);
	else
	{
		// file successfully open
		fclose(pFile);
	}
}
```
### 字符串的读取
```c

int main()
{
	char greeting[50] = "hell,and how are you today?";//字符串常量,自动在末尾加上\0
	puts("here are some strings:");//自动在结尾加上换行符
	printf("%c\n", *"hello");//h :说明字符串常量 被视为该字符串存储位置的指针

//读取
	/* gets 和 puts 不使用
		gets_s(word);//读取整行输入,直到遇到换行符,然后丢弃换行符,并在末尾加上\0  gets被标准废除了 但是gets_s 有些编译器支持c11,也不一定支持gets_s
		puts(word);
	*/
	char name[50];
	scanf("%s", &name);//只能读取一个单词 
	printf("%s\n", name);
	
	/*
		问题点:当先输入:①hello回车 下面的fgets得到的是一个换行
					  ②hello world回车 下面的fgets直接得到空格world回车 然后程序结束

		用
			while (getchar() != '\n');//刷新 缓冲区
		解决
	*/

	while (getchar() != '\n');//刷新 缓冲区
	

	char word[100];
	fputs("enter a string:", stdout);//fputs 不会再末尾加上换行符
	fgets(word, 100,stdin);//读取一行 fgets会存储换行符 并在结尾加入\0
	fputs(word, stdout);



	return 0;
}
```

### 字符串相关函数
```c

void string_all()
{
	char greeting[50] = "hello,and how are you today?";//字符串常量,自动在末尾加上\0
	puts("here are some strings:");//自动在结尾加上换行符
	printf("%c\n", *"hello");//h :说明字符串常量 被视为该字符串存储位置的指针


	printf("%d\n", strlen(greeting));//长度

	char flowers[100] = "flowers ";
	strncat(flowers, greeting, 100 - strlen(flowers) - 1);//合并 第三个参数表示最大可以添加的字符数
	puts(flowers);

	char comp1[50] = "Hello";
	char comp2[50] = "hello";

	//比较
	//strcmp 一直比较到结尾
	if (strcmp(comp1, comp2) == 0)//相同
	{
		puts("same string");
	}
	else if (strcmp(comp1, comp2) > 0) //comp1 大于 comp2
	{
		puts("comp1 greater than comp2");
	}
	else if (strcmp(comp1, comp2) < 0)//comp1 小于 comp2
	{
		puts("comp1 less than comp2");
	}

	//比较指定的前n个字符
	char comp11[50] = "astrodsd";
	char comp22[50] = "astrosds";
	if (strncmp(comp11, comp22, 5) == 0)//相同
	{
		puts("same string");
	}
	else if (strncmp(comp11, comp22, 5) > 0) //comp11 大于 comp22
	{
		puts("comp11 greater than comp22");
	}
	else if (strncmp(comp11, comp22, 5) < 0)//comp11 小于 comp22
	{
		puts("comp11 less than comp22");
	}


	//拷贝
	char qwords[20] = "hello world";
	char aimword[50] = "somethingdssssssssss";

	int size = strlen(aimword) - 1;
	strncpy(aimword, qwords, size);//拷贝时将第三个参数设置为目标字符串长度-1 
	aimword[size] = '\0';//确保存储的是一个字符串

	puts(aimword);


}
```


### 计算星期几
```c
int CaculateWeekDay(int y, int m, int d)
{
	if (m == 1 || m == 2) {
		m += 12;
		y--;
	}
	int iWeek = (d + 2 * m + 3 * (m + 1) / 5 + y + y / 4 - y / 100 + y / 400) % 7;
	switch (iWeek)
	{
	case 0: return 0;//星期一
	case 1: return 1;//星期二
	case 2: return 2;//星期三
	case 3: return 3;//星期四
	case 4: return 4;//星期五
	case 5: return 5;//星期六
	case 6: return 6;//星期日
	}
}
```


### 密码输入
```c
printf("请输入密码:");
char c;
int i = 0;
while ((c = _getch()) != '\r')
{
	pwd[i] = c;
	i++;
	if (c != '\b')    //输入内容不是退格时就显示 “*”号
	{
		printf("*");
	}
	else     //输入内容是退格时 删除前一个 “*”号
	{
		printf("\b \b");
	}
}
printf("\n");
```
### 处理字符串中的字符
```c
#include <ctype.h>
void strch_test()
{
	char line[50] = "dsdsd";
	char* find = strchr(line, 's');
	printf("%c\n", *find);//s
	puts(find);//sdsd
}
//字符串中 是否存在字符
bool strch_test(char * line,char c)
{
	char* find = strchr(line, c);
	if (find!=NULL)
	{
		return true;
	}
	else
	{
		return false;
	}
}
```


### 字符串与数值之间的转换
