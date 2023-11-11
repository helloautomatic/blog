C语言中最重要的一种语法机制就是指针，接下笔者将通过对以下20问题的解答来说明C语言指针。

***注：接下来的代码调试都在vs2019中实现***

#### 1、指向一维数组的指针
code1
```c
// code1
#include<stdio.h>

int main()
{
	int array1[10];
	for (int i = 0; i < 10; i++)
	{
		array1[i] = i;
	}
	int* int_p;
	int_p = &array1[3];
	printf("%d\n", *int_p);
	int_p++;
	printf("%d\n", *int_p);
	return 0;
}
```

在code1中， 指针变量“int_p”，可以进行自加运算，也可以进行自减运算，只要不超过数组“array1”的地址空间范围。

代码的运行结果如下：

```c
3
4
```
#### 2、 指向高维数组不同维数的指针
code2
```c
// code2
#include<stdio.h>

int main()
{
	int array1[5][5][5][5];

	// 把array1中的625个单元从0到624赋值
	for (unsigned char i = 0; i < 5; i++)
	{
		for (unsigned char j = 0; j < 5; j++)
		{
			for (unsigned char k = 0; k < 5; k++)
			{
				for (unsigned char l = 0; l < 5; l++)
				{
					array1[i][j][k][l] = i * 5 * 5 * 5 + j * 5 * 5 + k * 5 + l;
					// printf("%d\n", array1[i][j][k][l]);
				}
				
			}
		}
	}

	int*   p1 = array1;
	printf("%d\n", *p1);
	return 0;
}
```

3、指向字符串的指针；

code1

```
char *str1 = "abcdef";  // 不合法
char str[] = "abcdef";  // 合法
const char str* = "abcdef";  // 合法
```

code2
```
char str2[] = "abc";
char *str1 = str2;  // 合法
// str1[5] = ??
str1[4] = 'd';  // 危险的行为
```

指针变量作为函数形参和数组作为函数形参

code3
```
void func(int arr[50]);  // 这里的50是无效的

```
上述代码其实等价于
```
void func(int arr[]);
```

在二维数组中，数组的第一维的数字是无效的，会被编译器忽略，比如
```
void func(int arr1[2][1]);
```
void func(int arr1[2][1]); 中的 2 并不会对编译产生影响。这个声明实际上等同于 void func(int arr1[][1]); 或者 void func(int (*arr1)[1]);。

code4
```
char str[] = "abc";
// sizeof(str) = ??
```
在code4这个例子中，char str[] = "abc"; 定义了一个字符数组 str，并初始化为字符串 "abc"。由于初始化时指定了字符串内容，编译器会自动根据字符串的长度为数组分配足够的空间。
因此，sizeof(str) 的值将是包括字符串末尾的空字符在内的数组的总大小。对于 "abc"，它的大小是 4（'a', 'b', 'c'，以及末尾的空字符 '\0'）。所以，sizeof(str) 将是 4。


4、指向int，double等的指针；

指向int和double的指针变量的大小是固定的，并不会因为所指向的内存大小不一样而改变。
通常，在32bit计算机中，指针变量的大小就是4个字节。64bit计算机中，就是8个字节。
```
int *a;
double *b;
```
a ++和b++，指针移动的字节数量不一样。a移动的数量是sizeof(int)，b移动的数量是sizeof(double)

5、指向指针的指针

指向指针的指针，其实就是声明一个指针，这个指针指向的空间中存放的是个数值，该数值仍然是指针。

6、指向函数的指针；

```
void (*functionPointer)(int);
```
(*functionPointer)这里的一对中括号不能省略


7、一个数组，里面存放的都是指针
```
int num1 = 10, num2 = 20, num3 = 30, num4 = 40;
int *integers[] = {&num1, &num2, &num3, &num4};
```

int *integers[]和int **integers的区别,int *integers[] 是一个指针数组，每个元素都是一个整数指针，而 int **integers 是一个指向整数指针的指针。

```
int num1 = 10, num2 = 20, num3 = 30, num4 = 40;
int *ptr1 = &num1, *ptr2 = &num2, *ptr3 = &num3, *ptr4 = &num4;

int *pointers[] = {ptr1, ptr2, ptr3, ptr4};
int **integers = pointers;
```

8、一个数组，里面存放的都是函数指针


10、有多个返回值的函数；

11、返回值为指针的函数；

12、指针型的数组；

13、“void”型的指针；

14、“NULL”和void指针；

15、单级间址，二级间址，多级间址；

16、数组作为函数的参数，指向一维数组的指针，那这个数组应是二维的；

17、对char**p”的理解；

18、maloc，calloc，free，realloc的使用；

19、强制类型转换分为显式转换和隐式转换；

20、指向结构体变量的指针（结构体指针）；
