-----

第七章

```shell
//7.2

/* Program1.c */
#include "Lib.h"
int main()
{
    foobar(1);
    return 0;
}

/* Program2.c */
#include "Lib.h"
int main()
{
    foobar(2);
    return 0;
}

/* Lib.c */
#include <stdio.h>
void foobar(int i)
{
    printf("Printing from Lib.so %d\n",i);
}

/* Lib.h */
#ifndef LIB_H
#define LIB_H

void foobar(int i);

#endif


// 命令
// 编译出共享对象文件
gcc -fPIC -shared -o Lib.so Lib.c
// 编译可执行文件
gcc -o Program1 Program1.c ./Lib.so
gcc -o Program2 Program2.c ./Lib.so
```

```shell
/* 7-1 SimpleDynamicalLinking */

/* Program1.c */
#include "Lib.h"
int main()
{
    foobar(1);
    return 0;
}

/* Program2.c */
#include "Lib.h"
int main()
{
    foobar(2);
    return 0;
}


/* Lib.c */
#include <stdio.h>
void foobar(int i)
{
    printf("Printing from Lib.so %d\n",i);
}

/* Lib.h */
#ifndef LIB_H
#define LIB_H

void foobar(int i);

#endif


/* 将Lib.c编译成一个共享对象文件 */
gcc -fPIC -shared -o Lib.so Lib.c

/* 分别编译连接Program1.c Program2.c */
gcc -o Program1 Program1.c ./Lib.so
gcc -o Program2 Program2.c ./Lib.so
```

```shell
ENTRY(nomain)

SECTIONS
{
	.=0X08048000 + SIZEOF_HEADERS;

	tinytext   : { *(.text) *(.data) *(.rodata) }

	/DISCARD/  : { *(.comment)}
}
```

```shell
/* SimpleSection.c */

#include <stdio.h>

int printf(const char* format,...);

int global_init_var=84;
int global_uninit_var;

void func1(int i)
{
	printf("%d\n",i);
}

int main(void)
{
	static int static_var=85;
	static int static_var2;
	int a=1;
	int b;

	func1(static_var+static_var2+a+b);

	return a;
}
```

```shell
/* b.c */
int shared=1;

void swap(int* a,int* b)
{
	*a ^= *b ^= *a ^= *b;
}
```

```shell
/* a.c */
extern int shared;

int main()
{
	int a = 100;
	swap(&a,&shared);
}
```

```shell
char* str = "Hello World!\n";

void print()
{
	asm( "movl $13,%%edx \n\t"
		"movl $0,%%ecx  \n\t"
		"movl $0,%%ebx  \n\t"
		"movl $4,%%eax  \n\t"
		"int $0x80      \n\t"
		::"r"(str):"edx","ecx","ebx");
}

void exit()
{
	asm( "movl $42,%ebx  \n\t"
		"movl $1,%eax  \n\t"
		"int $0x80    \n\t");
}

void nomain()
{
	print();
	exit();
}
```











