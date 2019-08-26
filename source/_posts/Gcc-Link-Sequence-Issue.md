---
title: Gcc Link Sequence Issue
date: 2017-08-21 10:50:11
categories: 
   - C
   - Compiler
tags: 
   - GCC
thumbnailImage: mushroom.jpg
thumbnailImagePosition: left
autoThumbnailImage: yes
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
comments: yes
---
When you use a GCC compiler or a makefile to build your project, do you know the details behind this compiling ? Let me tell you what i see and what i know.
<!--more-->
<!--toc-->
### GCC Link Sequence Issue

#### GCC Link Background

GCC linker will link some object files  to only one object file.  During this time, it must replace all symbols(parameters/functions) to memory address(parameters address/functions address), for referring all sections in your object.

The most standard functions are located in "libc.a"(a means achieve), or located in shared dynamic lib "libc.so"(so means share object). these libs always located in `/lib/` or `/usr/lib/` or other folders of gcc finding setting.



#### Simple Case

```c
//test.c
#include <stdio.h>
int main(void)
{
    printf("Hello World!\n");
    return 0;
}
```

Use `gcc test.c -o test` to make a quick compile, for making *.c to *.exe or *.bin.



#### four entire steps in Gcc Compile

```
gcc -E sqltest.c -o sqltest.i	//firstly, make *.c preprocessing to *.i
gcc -S sqltest.i -o sqltest.s	//secondly, make *.i compiling to *.s   
gcc -c sqltest.s -o sqltest.o	//thirdly, make *.s assemblying to *.o  
gcc sqltest.o 'mysql_configs --libs' -o sqltest 
//finally, make *.o and *.so/*.lib/*.dll linking to *.bin
//which 'mysql_configs --libs' equal to "-L/usr/lib/x86_64-linux-gnu -lmysqlclient -lpthread -lm -lrt -ldl"
```

#### Sequence issue in linking

if you use `gcc 'mysql_configs --libs'sqltest.o -o sqltest ` to make a link, you will get errors which said "undefined reference to xxx". but why ?

because the finding sequence about symbols and libs of linking of GCC, is from Left to Right, according to `-L` sequence. So the Mysql API in sqltest.o can not find its references if you put the mysql libs to the left place of the sqltest.o. 

Solutions:

Put the lowest layer libs to the end of the linking command, like `gcc obj($?) -l (top logic libs) -l (middle packeted libs) -l (system libs) -o $@`.

Or use repeat option to your linking command, make the "ld" keep finding refer libs in your command.



#### Other Usage on library connection

use `readelf -d sqltest` or `ldd sqltest` to see the reference connection of a *.bin/*.so/.lib.

![readelf](http://blog.kuberfly.me/2017/08/21/Gcc-Link-Sequence-Issue/readelf.png)



Hope you have a strong makefile and happy compiling on your code.

End.