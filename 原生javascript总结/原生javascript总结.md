# 原生javascript总结

## 数据类型
基本类型
- String、Undefined、Null、Boolean、Number

引用类型
- Object、Function、Array、Date、RegExp

判断数据类型的方法
- typeof 
- instanceof 
- Object.prototype.toString.call() (推荐用法)
- obj.constructor

存储方式
- 基本类型存储在栈中，栈存储能力小于堆相对速度快，基本类型大小比较固定
- 引用类型地址存储在栈中，对应的值存储在堆中，引用类型大小是动态的，堆内存是无序存储，可以根据引用直接获取

数据类型转换
- 字符串转换
    - Number -> String - String(1) || (1).toString()  ==> '1'
    - Boolean -> String  - String(true) || true.toString()   ==> 'true'
    - Date -> String  - String(Date())  || Date().toString() ==> 'Thu Jul 26 2018 15:09:44 GMT+0800 (中国标准时间)'
- 数字转换
    - String -> Number - Number('1')  || parseInt('1') ||  + '1'   ==> 1 (遇到无法转换会变成NaN)
    - Boolean -> Number - Number(false)   ==> 0
## 作用域
作用域是在代码执行中某些变量、函数或者对象是否而可以被访问
- 全局作用域 - 定义在函数之外的变量，任何其他作用域内都可以访问和修改到全局作用域的变量
- 局部作用域 - 定义在函数内部的变量
- 静态作用域 - 源码定义的时候就确定当前环境的范围，静态作用域一般通过闭包来实现，也就是说闭包就是一个静态作用域，通过闭包或者自由变量在词法环境拿到变量
- 动态作用域 - 调用当前函数的引用定义了当前运行函数的激活环境，执行的时候才确定环境的范围，查找最近的活动记录中的同名变量
## Event Loop
## 闭包

## 函数

## 事件

## 面向对象

## 继承
