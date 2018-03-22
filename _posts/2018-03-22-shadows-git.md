---
layout: post
title: 使用shadowsocks来代理git
---

# overview


{% highlight python %}

# 是否是1080取决于你自己的配置
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

{% endhighlight %}
