---
layout: post
title:  "Netflix Spinnaker"
date:   2016-05-19 10:00:00 -0700
categories: notes overview
---
[Spinnaker][spinnaker] is a continuous delivery platform developed by Netflix. It is an descendant of [Asgard][asgard] 
project also developed by Netflix, but targeting AWS only. The is a version of implementation of Asgards ideas for
OpenStack called [aurora][aurora] implemented internally in PayPal and opensourced eventually.

In the next evolution stage Spinnaker targeting several cloud providers and even platforms (AWS, Google Cloud Platform,
Kubernetes, CloudFoundary) and this week the OpenStack support development was started by Target and Veritas 
([#spin-openstack][spin_openstack_spinnaker_slack_channel] slack channel)

Community around spinnaker is very active and taking into account players, it is probably going to be flagship of 
opensource image based deployment automation software for clouds.
 
Below you can find current architecture overview of Spinnaker. As most of opensource projects in active development
stage the technical aspects mostly not documented, however user's documentation and various deployment 
possibilities are enough to start.
 
Architecture
==============
Spinnaker has a microservice architecture and consists of set of services providing different aspects of 


Components
----------

Development stack
=================

Let us assume you have to set password for user, and this password should be stored in f.e. git.
The way how password actually stored in Linux helps with this task a lot.
 
I mean [/etc/shadow][etc_shadow-password-hash-formats] file.
The only this you have to do is to generate such a string for you and update it with [sed][sed]

{% highlight bash %}
>python -c 'import crypt; print crypt.crypt("test", "$6$random_salt")'
$6$random_salt$BnOQxEG8Gk2rzFYwoWXjr59zLVYzwshvca5oV0PtU8fAfT4a571evgca.E0hLnYNCdfq//zw9YyQN33QtztI10
{% endhighlight %}

F.e. with a script like this


{% highlight bash %}
bash-4.2$ export record="root:/8Ca1ce4.tORCPhjaFi/QE/VQ046Sd7DXAb...20g/:16855:0:99999:7:::"
bash-4.2$ echo ${record} | sed -e 's@^\(root\):\([0-9/A-Za-z\.$]*\):\(.*\):.*@\1:new_shadow_hash:\3@'
root:new_shadow_hash:16855:0:99999:7::
{% endhighlight %}

Where:
{% highlight bash %}
s # means replace in form of s/regexp/replacement/
@ # delimiter of our choice to not conflict with any of symbols in expression
^\(root\):\([0-9/A-Za-z\.$]*\):\(.*\):.* # escaped expression see original one next line
^(root):([0-9/A-Za-z\.$]*):(.*):.* # original expression: queries for 3 groups so on
\1:new_shadow_hash:\3 # replacement actually. But contains group 1 and group 2 data
{% endhighlight %}


[spinnaker]:http://www.spinnaker.io/
[asgard]:https://github.com/Netflix/asgard
[aurora]:https://github.com/paypal/aurora
[spin_openstack_spinnaker_slack_channel]:https://goo.gl/eiUAvx
