---
layout: post
title: 理解continuation
---

# overview

最近终于搞懂了延续相关的一些概念，简单记录一下

# 定义

延续就是执行完当前计算后，接下来要做的事情。比如(+ 3 (+ 1 2))中(+3 ...)是(+ 1 2)的延续

# call/cc
call/cc是call-with-current-continuation的简写,就是把当前的延续传做为参数传给call/cc指定的函数（也可能是一个延续对象）中,还是来举个例子吧

(+ 3 (call/cc (lambda (return)(return 4)))) => 7

1.把call/cc所在点的延续(+ 3 ...)传入函数(lambda (return)(return 4))

2.把4传入延续(+ 3 ...)中，(+ 3 4) => 7

# cps
cps=Continuation-passing style,就是在编码过程中把延续传入到被调用函数中

比如

(+ 3 (+ 1 2))是标准风格，对应的cps风格是

((lambda (x) (x (+ 1 2))) (lambda (y) (+ 3 y)))

# why cps


# 自动cps转换

这中cps的代码可读性很差，所以可以让程序员按照标准风格来写，由编译器自动把可以转换的代码转换成cps

# REF

https://en.wikipedia.org/wiki/Continuation

https://en.wikipedia.org/wiki/Call-with-current-continuation

https://en.wikipedia.org/wiki/Continuation-passing_style

https://www.cs.indiana.edu/~dfried/appcont.pdf 

http://www.sczyh30.com/posts/Functional-Programming/call-with-current-continuation/
