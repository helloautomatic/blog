# 映射的方法

###### 基地址

一、我们先来做个试验（在vs2019下做）

```c
// 代码1
#include <iostream>
using namespace std;
struct MyStruct
{
	int a;
	int b;
	int c;
};
#define BASE1 0x4000000
#define BASE2 ((MyStruct *) BASE1)
using namespace std;
int main()
{
	cout << BASE2 << endl;
	BASE2->a;
}
/*
代码解释：首先BASE1是一个常量，在“#define BASE2 ((MyStruct *) BASE1)”中，利用强制类型转换，把BASE1转换为了结构体类型。因此BASE2就是一个结构体常量，它指向一个结构体的地址。
在stm32中，该结构体的地址代表了寄存器中地址.
*/
```


二、再做一个实验如下：

```c
// 代码2
#include<stdio.h>
int main()
{
	int* p;
	p = 89342903;
	*p = 9;
	return 0;
}

```
在Windows vs2019中运行该代码会出错，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191123225220555.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjc2NjYzNw==,size_16,color_FFFFFF,t_70)

意思访问冲突了。在代码2中，给指针p随便赋了一个值，因此，我们不知道p指向了哪里（有可能p指向了Windows中的某个寄存器），所以会导致报错。当然了，这是在Windows下操作会这样。vs2019是一款比较智能的开发环境，本身自带保护机制。所以，不会允许用户写的程序中的指针乱指。
但在stm32中，就不一样了。stm32的地址都是固定的，如果指针乱指的话，很可能会不小心修改了某个寄存器的值，这样导致的bug很难找到。

###### 顶层头文件中的地址映射方法
以GPIOB为例子

```c
//代码3
typedef unsigned  int uint32_t;
#define PERIPH_BASE                    ((uint32_t)0x40000000)
#define APB2PERIPH_BASE                (PERIPH_BASE + 0x10000)
#define GPIOB_BASE                     (APB2PERIPH_BASE + 0x0C00)
#define GPIOB_ODR_Addr                 (GPIOB_BASE+12)
#define BITBAND(addr, bitnum)          ((addr & 0xF0000000)+0x2000000+((addr &0xFFFFF)<<5)+(bitnum<<2)) 
#define MEM_ADDR(addr)                 *((volatile unsigned long  *)(addr)) 
#define BIT_ADDR(addr, bitnum)         MEM_ADDR(BITBAND(addr, bitnum)) 
#define PBout(n)                       BIT_ADDR(GPIOB_ODR_Addr,n)
#define LED0                           PBout(5)
```

从以上代码可以看出：
以基本地址（PERIPH_BASE ）为最初的地址，其它寄存器的地址都是以基本地址开始的进行加加减减得到的。其它的任何寄存器都是同样的道理。

#### 总结

这是在Windows平台下做的实验，其实stm32道理也是一样的。这里就可以看出来，虽然，stm32和windows的硬件平台差了很多，但是某些方面是一样的。在个人电脑上裸机编程，也要写下
内存逻辑地址的确切数字。只不过，这些我们在个人计算机上都看不到。

其实上述就是寄存器映射（就是给地址取个名字，便于识别和记忆），在stm32中的memery map中，地址是统一编址的。这些地址就是一串数字，然后使用宏定义给这串数字命个名。就是寄存器映射。还需要注意的是，这里的寄存器指的是内部外设寄存器（peripherals），而非内核寄存器（CPU内核寄存器，比如systick）。这两者有很大的区别的。在地位上，外设寄存器和内核寄存器不一样。外设寄存器和SRAM是统一编址的，而内核寄存器并不参与统一编址。举个不恰当的例子，就是内核寄存器类似于CPU的演草纸，它可以被CPU进行最快速的操作。比如在FreeRTOS中，要用到定时器systick，来实现任务的时间片调度，这里就要用到systick，而非外设定时器Timer。
