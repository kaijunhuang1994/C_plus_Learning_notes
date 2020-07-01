# C++之makefile写法

## 什么是makefile

Makefile 文件描述了整个工程的编译、连接等规则。

编译整个工程你所要做的唯一的一件事就是在shell 提示符下输入make命令。

## 编译与链接

编译（compile）是指把源文件编译成中间代码文件，在Windows下也就是.obj文件，UNIX下是.o文件，即Object File。

链接（link）是指大量的Object File合成执行文件，主要是链接函数和全局变量。在大多数时候，由于源文件太多，编译生成的中间目标文件太多，我们要给中间目标文件打个包在Windows下这种包叫“库文件”（LibraryFile)，也就是 .lib文件，在UNIX下，是Archive File，也就是.a文件。

## makefile基本格式

makefile基本格式如下：

```shell
target ... : prerequisites ...
	command
	...
	...
```

- target - 目标文件, 可以是 Object File, 也可以是可执行文件

- prerequisites - 生成 target 所需要的文件或者目标

- command - make需要执行的命令 (任意的shell命令)，如果其不与“target:prerequisites”在一行，那么，必须以**[Tab]**开头，如果和prerequisites在一行，那么可以用分号做为分隔

## 举例说明

### 初始情况

我们有一个主程序代码(main.c)、三份函数代码(getop.c、stack.c、getch.c)以及一个头文件(calc.h)。

通常情况下，我们需要这样编译：

```shell
gcc -o calc main.c getch.c getop.c stack.c
```

使用makefile，我们可以这样做：

```shell
calc: main.c getch.c getop.c stack.c
	gcc -o calc main.c getch.c getop.c stack.c 
```

- 目标（target）：冒号前的calc

- 依赖关系表：冒号后的部分（main.c getch.c getop.c stack.c）

- 命令部分：`gcc -o`，当依赖关系表中文件发生变化即可触发命令部分（此时尚未考虑 .h 文件的修改）

> `gcc -c`：只编译源文件但不链接，即将 .c / .cpp 文件编译为 .o 文件
>
> `gcc -o`：用于指定输出（out）文件名，不用`-o`的话，一般会在当前文件夹下生成默认的a.out文件作为可执行程序。
>
> `gcc`：后接 .o 文件生成可执行文件

### 引入宏定义

为了提升效率，做以下修改：

```shell
cc = gcc
prom = calc
source = main.c getch.c getop.c stack.c
 
$(prom): $(source)
	$(cc) -o $(prom) $(source)
```

定义宏：cc、prom以及source，用于字符串替换

### 考虑头文件的影响

但是如果我们修改的是calc.h文件，make就无法察觉到变化了，故应该将 .h 文件也添加到依赖表中：

```shell
cc = gcc
prom = calc
deps = calc.h
obj = main.o getch.o getop.o stack.o
 
$(prom): $(obj)
	$(cc) -o $(prom) $(obj)
 
main.o: main.c $(deps)
	$(cc) -c main.c
 
getch.o: getch.c $(deps)
	$(cc) -c getch.c
 
getop.o: getop.c $(deps)
	$(cc) -c getop.c
 
stack.o: stack.c $(deps)
	$(cc) -c stack.c 
```

### 优化头文件依赖的重复操作

这样做可以在更改了 .h 文件后，使得make也能够察觉。但是代码显得非常啰嗦。根据所有 .c 都被编译成了 .o 文件的特点，可以做进一步的简化：

```shell
cc = gcc
prom = calc
deps = calc.h
obj = main.o getch.o getop.o stack.o
 
$(prom): $(obj)
	$(cc) -o $(prom) $(obj)
 
%.o: %.c $(deps)
	$(cc) -c $< -o $@
```

这里又用到了几个特殊的宏。

- %.o:%.c，这是一个模式规则，表示所有的 .o 目标都依赖于与它同名的 .c 文件（**%替代同名**）
- `$<`，代表的是**依赖关系表**中的第一项（如果我们想引用的是整个关系表，那么就应该使用`$^`），具体到我们这里就是%.c。

- `$@`，代表的是当前语句的**目标**，即%.o。

这样一来，make命令就会自动将所有的 .c 源文件编译成同名的 .o 文件。

| 自动变量 | 含义                                                 |
| -------- | ---------------------------------------------------- |
| $@       | 目标集合                                             |
| $%       | 当目标是函数库文件时, 表示其中的目标文件名           |
| $<       | 第一个依赖目标. 如果依赖目标是多个, 逐个表示依赖目标 |
| $?       | 比目标新的依赖目标的集合                             |
| $^       | 所有依赖目标的集合, 会去除重复的依赖目标             |
| $+       | 所有依赖目标的集合, 不会去除重复的依赖目标           |
| $*       | 这个是GNU make特有的, 其它的make不一定支持           |

### 利用shell命令完成自动化搜索源文件

前面提到利用宏：cc、prom以及source，进行字符串替换，但是=后的文件需要手动输入比较麻烦，故可以进一步优化：

```shell
cc = gcc
prom = calc
deps = $(shell find ./ -name "*.h")
src = $(shell find ./ -name "*.c")
obj = $(src:%.c=%.o) 
 
$(prom): $(obj)
	$(cc) -o $(prom) $(obj)
 
%.o: %.c $(deps)
	$(cc) -c $< -o $@
 
clean:
	rm -rf $(obj) $(prom)
```

- shell函数主要用于执行shell命令
- `$(src:%.c=%.o)`则是一个字符替换函数，它会将src所有的.c字串替换成.o
- make clean命令时，它就会去删除该工程生成的所有编译文件

这样我们就完成了自动化编译的makefile文件！！

---

版权声明：本文为CSDN博主「ZONG_XP」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zong596568821xp/java/article/details/81134406