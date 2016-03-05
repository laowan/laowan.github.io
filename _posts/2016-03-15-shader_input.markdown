---
layout: post
title:  "如何传入数据给GLSL程序"
categories: laowan
---

###简介
任何一个程序都有输入和输出，GLSL程序（或称 shader 程序）也不例外。由于GLSL程序是运行在GPU上面，所以它只能使用显存里面的数据，这样的话怎么传入数据，就没有那么直观。

一般来说，有两种数据：
- 用户定义的 uniform 变量
- 用户定义的顶点属性变量

###uniform变量
用 uniform 修饰的变量可以理解为全局常量，变量的值在GLSL程序执行之前由用户指定，并在整个GLSL程序的执行过程中不会发生变化。OpenGL 提供了两个函数用来完成给 uniform 变量赋值的过程。

    glGetUniformLocation(program, name)

这个函数用来得到 **name** 对应的变量位置。GLSL程序会在内部维护一个包含了所有 uniform 变量的表格，这里的位置可以理解为表格中 **name** 变量的索引。有了变量的索引之后，就可以设置它的值，赋值过程由下面这个函数来实现。

```
    glUniform*(location, value)
```

###顶点属性变量
顶点属性变量一般使用关键字 **varying** 和 **in** 来修饰：

    varying vec3 position; // varying 在 GLSL 130（GL3.0）废弃
    in vec3 position;

跟 uniform 变量一样，为了赋值，首先我们需要知道变量在 shader 程序里的索引。OpenGL提供两个函数来获取索引：

    glGetAttribLocation(program, name)  
    glBindAttribLocation(program, index, name)

通过 glBindAttribLocation() 可以显示的指定一个属性变量和一个索引值的绑定，但是这个操作必须要在 shader 程序被链接之前执行。用户可以定义的顶点属性的最大数量是有限制的，可以用 GL_MAX_VERTEX_ATTRIBS 查询。得到索引之后，赋值函数如下：

```
    glVertexAttrib*(index, value)
```

另外，顶点属性可以保存在顶点数组中，为了指定用于更新变量值的数组，OpenGL 提供了另外一个函数：

```
    glVertexAttribPointer(index, size, type, normalized, stride, pointer)
```

最后不要忘记使用函数 glEnableVertexAttribArray(index) 来启用顶点属性数组。

