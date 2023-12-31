问题：既然C语言有缺陷和陷阱，那么为什么没有人去解决这些缺陷和陷阱呢？

答：
C语言存在缺陷和陷阱的一些原因涉及到它的历史、设计哲学和广泛的应用领域。

首先，C语言是在20世纪70年代初由Dennis Ritchie在贝尔实验室设计的。当时的计算环境和需求与今天有很大不同。在那个时候，主要的关注点是性能和底层控制，而不是高级抽象和安全性。C语言的设计目标是提供一种有效的编程工具，允许程序员对硬件进行更直接的控制。

随着时间的推移，C语言变得非常流行，许多现代编程语言也从它派生而来。尽管有更安全和高级的语言出现，但C语言在系统编程、嵌入式系统和性能关键应用等领域仍然被广泛使用。改变C语言的设计可能会破坏现有的代码和生态系统，因此人们更倾向于在新的编程语言中解决这些问题，而不是修改C语言本身。

此外，C语言的开放性和灵活性也是其受欢迎的原因之一。它允许程序员做出更多的决策，但这也意味着在不小心的情况下可能会引入错误。一些陷阱和缺陷可能被视为程序员的责任，需要仔细的代码审查和测试来确保质量。

总体而言，C语言的存在缺陷和陷阱是一种权衡，取决于在特定环境中的需求和权衡。

# 比较与赋值陷阱
```
#include <stdlib.h>
#include <stdio.h>
int main()
{
	int *p = (int *)malloc(sizeof(int));
	if(p = NULL)
	{
		printf("out of memory\n");
		exit(1);
	}

	*p = 10;
	printf("*p = %d\n",*p);
	return 0;
}
```

# 整形常量陷阱
```
// code1
#include <stdlib.h>
#include <stdio.h>

int main()
{
	int a = 012;  // 八进制的表示方式
	printf("a = %d\n",a);
	return 0;
}
```

在code2中，有的编译器会报警告，有的编译器中，会报错误。
在个人pc中，无论重启电脑多少次，局部变量a的地址值都是不变的。
在code2中，如果是stm32嵌入式系统，假设使用了FreeRTOS，虽然a是局部变量，但是打印他的地址是否变化，还要另说，需要做实验证明。
有可能a代表的是它的实际的物理地址。
这可能要考虑代码加载到RAM中时，符号解析和重定向的问题。
```
// code2
#include <stdlib.h>
#include <stdio.h>

int main()
{
	int a = 100;
	printf("address of a is:%d\n", &a);

	int *p = 6422296;  // 假设在个人计算机中，局部变量a的地址为6422296，具体地址是多少，需要在`printf("address of a is:%d\n", &a);`终端输出中查看
	*p = 90;
	
	printf("a = %d",a)
	return 0;
}
```


