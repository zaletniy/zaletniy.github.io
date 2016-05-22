---
layout: post
title:  "Behind proxy"
date:   2016-05-19 10:00:00 -0700
categories: notes cheatsheet
---
When you are working on production you usually doesn't have direct outgoing connection enabled or it could be really
limited.

Here you can find a collection of configuration for different software for working via proxy.
Let as assume that in all next examples we are talking about http proxy working on host
{% highlight bash %}PROXY_HOST{% endhighlight %} and port {% highlight bash %}PROXY_PORT{% endhighlight %}

githib.com behind proxy
=======================
Github could be accessed via various [protocols][git_protocols]

You can talk to github.com at 443 port as well see [ssh over https port][ssh_over_https_port]

{% highlight bash %}
~$ cat ~/.ssh/config

Host github.com
  Hostname ssh.github.com
  Port 443
  
{% endhighlight %}

General proxy solution could be build (and you can use it of other cases) with [corkscrew][corkscrew] utility.


{% highlight bash %}
~$ sudo apt-get install corkscrew

~$ cat git-proxy.sh
#!/bin/sh

exec corkscrew PROXY_HOST PROXY_PORT $*


~$ env | grep PROXY
GIT_PROXY_COMMAND=/home/illia_svyrydov/git-proxy.sh
{% endhighlight %}

corkscrew could be used for tunneling ssh connections in pretty handy way

{% highlight bash %}
~$ cat ~/.ssh/config
Host my_host
  Port 22
  ProxyCommand corkscrew PROXY_HOST PROXY_PORT %h %p
{% endhighlight %}

curl & wget
============================
Just export system variables

{% highlight bash %}
$ env | grep proxy
http_proxy=http://PROXY_HOST:PROXY_PORT
https_proxy=http://PROXY_HOST:PROXY_PORT
{% endhighlight %}

gradle & gradlew
================
It follows standard java application proxy configuration, just to enable it isolated, it is better to use gradle 
specific env variable

{% highlight bash %}
$ env | grep proxy
GRADLE_OPTS=-Dhttps.proxyHost=PROXY_HOST -Dhttps.proxyPort=PROXY_PORT
{% endhighlight %}

npm
=====
Source [here][node_behind_proxy]
{% highlight bash %}
npm config set proxy http://PROXY_HOST:PROXY_PORT
npm config set https-proxy http://PROXY_HOST:PROXY_PORT
{% endhighlight %}

[git_protocols]:https://git-scm.com/book/ch4-1.html
[ssh_over_https_port]:https://help.github.com/articles/using-ssh-over-the-https-port/
[corkscrew]:http://www.unix.com/man-page/debian/1/corkscrew/
[node_behind_proxy]:https://jjasonclark.com/how-to-setup-node-behind-web-proxy

