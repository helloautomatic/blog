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

4、指向int，double等的指针；

5、指向指针的指针

6、指向函数的指针；

7、一维数组做函数的参数；

8、二维数组做函数的参数；

9、三维数组做函数的参数；

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
