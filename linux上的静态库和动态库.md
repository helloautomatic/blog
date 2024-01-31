有三个源文件，每个源文件对应一个头文件，如下：

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

