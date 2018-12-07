---
layout: post
title: c++中char s[],char *s
date: 2018-08-11
tags: c/c++
---

## 前言
此博客总结了一些近期遇到的容易混淆的字符串表示方法，注重具体问题具体分析。

## 1.本质区别

char s[]是一个`字符串数组`，s指向首字符地址，字符元素为char类型。 char *s是一个`指针变量`，保存的是字符串首地址；

## 2.字符数组 char s[]
字符数组的定义方式：
```c++
 1. char s1[6];  //分配了6字节的空间
 2. char s2[] = "Hello";
 3. char s3[] = {'H','e','l','l','o','\0'};
 4. char s4[6]; s4[6] = "Hello";  //错误
 5. char s5[6]] = "Hel"; s5[0] = "H"; s5[1] = "E"; s5[2] = "L"; //正确
```
>* 使用char s[]定义一个字符数组时，[]里写上数字表示数组的大小(如1)；
>* 若不写，则需要为字符数组声明初始值(如2)，否则会出错；
>* s2和s3等价, 但是要注意 s2.size()==5, sizeof(s2)==6
>* s4指向的`内存区域的地址和容量`在生命期内不会改变
>* 4之所以错误，因为只能对字符数组元素赋值，不能对整个字符数组赋值。

## 3. 字符指针 char *s
以实例来说明
```c++
 1. char *s1 = "Hello";
 2. char s2[] = "HELLO";
 3. char *s3 = s2;
```
>* 使用char *s定义指针数组时，右侧的字符串在编译时就确定，因此指针s只能`访问字符串而不能改变。`
>* char类型的指针变量可以指向字符串。


## 4. char s[] 和 char *s区别

>* 执行`char s[] = "Hello";`，编译器为字符数组分配了内存空间，s指向Hello首地址，s不可改变，但是s指向的内容可以改变。
>* 执行`char *s = "Hello";`，编译器分配一块内存，保存字符元素`Hello`；然后在栈上分配内存保存s，s的内容为常量，为"Hello"的首地址，。
>* char ss

```c++
char s[] = "Hello";
s[0] = 'h';          //正确
char *s = "Hello";
s[0] = 'h';           //错误
```

>* vs2017编译结果如下：值得注意的是`&s[i]`是从下标i开始打印字符。

```c++
const char s[] = "World";
const char *ptr = "Hello";
cout << s << endl;              //World
cout << *s << endl;             //W
cout << &s << endl;             //地址
cout << s + 1 << endl;          //orld
cout << *(s + 1) << endl;       //o
cout << &s[1] << endl << endl;  //orld

cout << ptr << endl;            //Hello
cout << *ptr << endl;           //H
cout << &ptr << endl;           //地址
cout << ptr + 1 << endl;        //ello
cout << *(ptr + 1) << endl;     //e
cout << &ptr[1] << endl;        //ello
```
