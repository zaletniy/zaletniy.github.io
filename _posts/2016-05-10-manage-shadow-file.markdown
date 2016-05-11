---
layout: post
title:  "Manage shadow file"
date:   2016-05-10 17:59:45 +0200
categories: notes
---
Let us assume you have to set password for user, and this password should be stored in f.e. git.
The way how password actually stored in Linux helps with this task a lot.
 
I mean [/etc/shadow][etc_shadow-password-hash-formats] file.
The only this you have to do is to [generate][shadow-hash-generation] such a string for you and update it with [sed][sed]

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

[etc_shadow-password-hash-formats]:http://www.aychedee.com/2012/03/14/etc_shadow-password-hash-formats/
[shadow-hash-generation]:http://unix.stackexchange.com/questions/81240/manually-generate-password-for-etc-shadow
[sed]:http://www.grymoire.com/Unix/Sed.html#uh-4

