### 有三个源文件，每个源文件对应一个头文件，如下：

lib1.cpp
```
#include<iostream>
#include "lib1.h"

void func1()
{
	std::cout<<"this is the latest version of lib1"<<std::endl;
}
```

lib1.h
```
#ifndef __LIB_1_H
#define __LIB_1_H

 
void func1();

#endif
```

lib2.cpp
```
#include<iostream>
#include "lib2.h"

void func2()
{
	std::cout<<"this is the latest version of lib2"<<std::endl;
}
```

lib2.h
```
#ifndef __LIB_2_H
#define __LIB_2_H

 
void func2();

#endif
```

lib3.cpp
```
#include<iostream>
#include "lib3.h"

void func3()
{
	std::cout<<"lib3"<<std::endl;
}
```

lib3.h
```
#ifndef __LIB_3_H
#define __LIB_3_H

 
void func3();

#endif
```

### 静态库的制作方法
```
g++ -c lib1.cpp lib2.cpp lib3.cpp
ar -cr liball.a lib1.o lib2.o lib3.o
ar -t liball.a
g++ main.cpp -o main.out -L./ -lall
./main.out
```

### 动态库的制作方法
```
g++ -fPIC -c lib1.cpp lib2.cpp lib3.cpp
g++ -shared -o liball.so lib1.o lib2.o lib3.o
g++ main.cpp -o main.out -L./ -lall
export LD_LIBRARY_PATH="./"
./main.out
```


## 编译指令总结

gcc/g++编译命令

	-E：预处理命令，生成 *.i 文件，虽然linux下没有文件后缀这一说法，但是通常会给个后缀名
	-S：编译命令，生成 *.s 文件
	-c：汇编命令，生成 *.0 文件
	-o output_filename：指定输出文件的名称。
	-I path：添加头文件的搜索路径。 -I. 或者 -I./
	-L path：添加库文件的搜索路径。   -L. 当前路径   gcc/g++只会在 /lib和 /usr/lib这两个目录下搜索库文件
	-l library：链接指定的库文件。
	-Wall：显示大部分编译告警。
	-g：生成调试信息。
	-O0、-O1、-O2、-O3：编译器的优化选项，-O0表示没有优化，-O1为缺省值，-O3优化级别最高。
	-std=c99 或 -std=c++11 等：指定C或C++语言标准版本。
	-fPIC：生成位置无关代码，用于创建共享库。
	-shared：生成共享库。
	-static：使用静态链接方式编译，生成的文件体积会很大。
	-fPIC：作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的。
	-ltest:库文件名称。 比如库文件名称是libtest.a，那么该命令就是 -ltest


	example
		库的命名规范lib*.a(静态库)  lib*.so(动态库)  *通常就是.o 文件的名字
		
		生成静态库
				生成汇编文件
					g++ -c -o lib1.o lib1.cpp 或者 g++ -c lib1.cpp  (c-create)
				执行ar命令   ar -cr libxxx.a *.o
					ar命令可以用来创建，修改库，也可以从库中提出单个模块，Ar是archiver的缩写，archive归档的意思
						-r 将文件插入库文件中，如果已经存在会直接替换
						-t 显示库文件中所包含的.o文件
						-c 表示创建

		生成动态库
			g++ -fPIC lib1.cpp -c (生成位置无关的.o文件)
			g++ -shared -o libtest.so test.o (生成动态库文件)

				动态库的使用(链接)：
				g++ main.c -L. -ltest
				但是生成的可执行文件并没有办法运行，主要是因为，需要动态的链接库，而连接器只知道  /lib和 /us/lib下的库文件，但是这样会污染原来的目录
				因此，有方法
				export LD_LIBRARY_PATH=""
				这个目录，会优先于  /lib和 /us/lib

		如何判断一个函数有没有动态链接库？
					file指令可以查看一个文件的类型
					ldd命令 
					
		-static选项
			如果目录下面，既有静态库又有动态库，而且两个库的名字相同，编译器会如何决定呢？
				这样的情况下，gcc会默认选择链接动态库，如果想要链接静态库，需要加上-static选项，如果动态库不存在，会链接动态库




		动态链接和静态链接相互混合

