---
layout: post
title: systemtap 使用
---

# 多线程调试

{% highlight c %}
info thread // 查看当前线程的线程
thread <ID> // 切换线程
break file.c:10 thread all //所有线程执行到file.c的第10行中断
set scheduler-locking off|on|step // 执行step时候,on是只有当前进程能继续执行
{% endhighlight %}

# 条件断点

{% highlight c %}
break [where] if [condition]
{% endhighlight %}

# command

{% highlight c %}
// 当执行到断点1的时候执行以下三个命令
// 哈哈，这也许就是自动化调试吧
break func
command 1
print x
print y
print z
end
{% endhighlight %}



# REF

<http://www.unknownroad.com/rtfm/gdbtut/> 最重要的来源
<https://www.gnu.org/software/gdb/documentation/> 官方文档,很多有用的外部链接
<https://coolshell.cn/tag/gdb> 酷壳
