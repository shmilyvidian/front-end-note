# js高阶与新特性
## 柯里化
函数柯里化是函数式编程的重要概念，也是高阶函数中一个重要的应用，其含义是给函数分步传递参数，每次传递部分参数，并返回一个更具体的函数接收剩下的参数。
- 最基本的柯里化函数
    ```javascript
        function addCurrying(a) {
            return function (b) {
                return function (c) {
                    return a + b + c;
                }
            }
        }
    ```
- es5
    ```javascript
    function currying(func, ...args) {
        if(args.length > func.length){
            func(args)
        }
        return function(...args2){
            return currying(func,...args,...args2)
        }
    }
    ```
## 尾调用
- 递归实现斐波拉契
    ```javascript
        function fibonacci(n){
            if(n < 2){
                return 1
            }
            return fibonacci(n-1)+fibonacci(n-2)
        }
    ```
函数执行是居于栈的，每次调用都会新起一个栈，超过浏览器内存就会堆栈溢出
- 利用尾调用改进
尾调用就是函数的最后一步是执行一个函数没有别的操作，且最后执行的函数的返回直接被当前函数返回，
它不会在调用栈上增加新的堆栈帧，而是直接更新调用栈，调用栈所占空间始终是常量，节省了内存，避免了爆栈的可能性
```javascript
     function fibonacci (n, prev = 1, next = 1) {
        if (n < 2) {
            return next
        }
        return fibonacci(n - 1, next, prev + next)
    }

    function fibonacciLoop(n, a = 0, b = 1) { 
        while (n--) {
            [a, b] = [b, a + b]
        }
        return a
    }
```
## compose
实际需求中需要我们组合函数来实现
```javascript
    var compose = function(...args){
        return function fn(res){
            var list = args.length
            while(list.length > 0){
                result = list.pop()(result)
            }
            return list
        }
    }   

    var compose = (...args)=>(res)=>args.reverse().reduce((res)=>fn(res),res)
```