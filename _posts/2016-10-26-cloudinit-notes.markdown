---
layout: post
title:  "cloud-init notes"
date:   2016-10-26 12:12:12 +0700
categories: notes
tags: cloud-init cloudinit
---
### Notes

#### Runtime data and state
{% highlight bash %}
/var/lib/cloud/
{% endhighlight %}

#### To run a single config handler
{% highlight bash %}
cloud-init --debug  single --name setup_ssh
{% endhighlight %}

####Logs are stored at
{% highlight bash %}
/var/log/cloud-init.log
{% endhighlight %}

### Links
1. [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/)
