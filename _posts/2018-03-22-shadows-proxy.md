---
layout: post
title: 使用shadowsocks代理
---

# overview


{% highlight python %}

# git使用代理
# 是否是1080取决于你自己的配置
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

{% endhighlight %}

{% highlight python %}

# vagrant use proxy
export http_proxy=http://0.0.0.0:1087;export https_proxy=http://0.0.0.0:1087;
vagrant plugin install vagrant-proxyconf
export VAGRANT_HTTP_PROXY=${http_proxy}
export VAGRANT_NO_PROXY="127.0.0.1"

{% endhighlight %}
