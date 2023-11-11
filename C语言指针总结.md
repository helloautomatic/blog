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
```
#include <stdio.h>

// 假设有两个函数
void func1() {
    printf("这是函数1\n");
}

void func2() {
    printf("这是函数2\n");
}

int main() {
    // 定义函数指针数组，存放两个函数的地址
    void (*functionPointers[2])() = {func1, func2};

    // 调用数组中的第一个函数
    functionPointers[0]();

    // 调用数组中的第二个函数
    functionPointers[1]();

    return 0;
}
```

**11、返回值为指针的函数；**

这个需要特别注意。当一个函数返回指针的时候，首先要确定该指针指向的内容是有效的。
比如说，有如下代码
```
int * func(void)
{
	int a = 0;
	int *p = NULL;
	p = & a;
	return p;
}

int *p = func();
*p = 9;
```
在上述代码中，func中的变量a在函数调用结束后就无效了，但是却返回了他的地址。
然而，在函数外部使用*p=9对函数地址进行了内存改写操作，这样是错误的。会导致意想不到的结果，进而导致程序崩溃。究其原因就是，

没搞明白在C语言中，哪些变量是放在栈中，哪些变量是放在堆中，还有变量的生存周期是怎样的。

这种就叫做悬挂指针： 在 func 函数中，你返回了一个指向局部变量的指针。一旦函数执行完毕，a 的生命周期结束，指针 p 就变成了悬挂指针（dangling pointer），指向的内存区域可能被其他程序使用，导致未定义的行为。

这只是一个简单的例子，很多人觉得

12、悬挂指针和野指针有什么区别？

悬挂指针（Dangling Pointer）： 指的是指针仍然指向之前分配的内存地址，但这部分内存已经被释放或超出了其作用域。这样的指针可能导致未定义的行为，因为你访问的是无效的内存。悬挂指针可以是野指针的一种情况，但不限于野指针。

野指针（Wild Pointer）： 指的是指针变量的值是一个未知的地址，它没有被初始化，或者在释放后没有被置为 NULL。野指针可能指向任意位置的内存，包括一些未分配的内存区域。野指针是一种常见的编程错误，容易导致程序崩溃或产生不可预测的结果。

13、“void”型的指针；
```
#include <stdio.h>

// 一个使用void*的通用打印函数
void printValue(void *ptr, char type) {
    switch(type) {
        case 'i':
            printf("整数值：%d\n", *((int*)ptr));
            break;
        case 'f':
            printf("浮点数值：%f\n", *((float*)ptr));
            break;
        case 'c':
            printf("字符值：%c\n", *((char*)ptr));
            break;
        default:
            printf("未知类型\n");
    }
}

int main() {
    int intValue = 42;
    float floatValue = 3.14;
    char charValue = 'A';

    // 使用void*指针传递不同类型的数据
    printValue(&intValue, 'i');
    printValue(&floatValue, 'f');
    printValue(&charValue, 'c');

    return 0;
}

```

14、NULL，void*，数值0，三者有什么区别和联系？；

在很多系统中，NULL 被定义为数值 0 或者某种表示空指针的特殊值。因此，NULL 和数值 0 在指针上通常是等效的。
void* 通常用于接收或传递未知类型的指针，而 NULL 或者数值 0 通常用于表示空指针

16、数组作为函数的参数，指向一维数组的指针，那这个数组应是二维的；

18、maloc，calloc，free，realloc的使用；

malloc（Memory Allocation）：

函数签名：void* malloc(size_t size);
用于分配指定大小的内存块。
返回一个指向分配内存的指针。
例子：int *arr = (int*)malloc(5 * sizeof(int));
calloc（Contiguous Allocation）：

函数签名：void* calloc(size_t num, size_t size);
用于分配 num 个大小为 size 字节的连续内存块，并将每个字节初始化为0。
返回一个指向分配内存的指针。
例子：int *arr = (int*)calloc(5, sizeof(int));
free：

函数签名：void free(void *ptr);
用于释放之前由 malloc 或 calloc 分配的内存。
例子：free(arr);
realloc（Reallocate）：

函数签名：void* realloc(void *ptr, size_t size);
用于重新分配之前由 malloc 或 calloc 分配的内存块的大小。
返回一个指向新分配内存的指针。原始指针可能被释放或移动到新的位置。
例子：int *newArr = (int*)realloc(arr, 10 * sizeof(int));

20、指向结构体变量的指针（结构体指针）；

```
struct S1 {
    int a;
    int b;
    int c[];  // 柔性数组
};

struct S2 {
    int a;
    int b;
    int *c;
};
```

在上述代码中

sizeof(S1) = sizeof(int a) + sizeof(int b) = 4 + 4 = 8;

sizeof(S2) = sizeof(int a) + sizeof(int b) + sizeof(int*c) = 4 + 4 + 4 = 12;

### 结构体中的强制类型转换需要注意什么？

```
struct S1 {
    int a;
    int b;
};

struct S2 {
    int a;
    int b;
    int c;
};

struct S1 s1;
s1.a = 1;
s1.b = 2;

struct S2 s2;
s2.a = 3;
s2.b = 4;
s2.c = 5;

struct S1 *ps1 = &s1;
struct S2 *ps2 = &s2;

((S1*)ps2)->a;
((S2*)ps1)->c = 8;  // 可能会发生意向不到的错误 

```




