---
layout: post
title:  "Explore the fleet of machines with Ansible"
date:   2016-02-19 14:41:45 +0200
categories: ansible
---
[Ansible][ansible] becomes more and more popular automation tool. It has no agent (technically it has sshd, python and bash but
such a software is present on any machine), zero initial deployment, very easy to start.

## Problem definition
Configure hardware watchdog on the fleet of machine.
We don't know what the hardware, what the operation systems and we have no proper discovery tool to get this information.

## Ansible key features

We are going to use following Ansible features

### Host grouping module [group_by][group_by_module]
Creates list of host groups for every unique key value.

{% highlight yaml lineos %}
{% raw %}
tasks:
   - name: Create a group of all hosts by hardware
     group_by: key={{ansible_product_name}}

{% endraw %}
{% endhighlight %}



### Debug module [debug][debug_module]
Writes some output to your ssh console

{% highlight yaml linenos %}
{% raw %}
- name: Unsupported configuration
     debug: msg="Configuration {{ansible_product_name}} {{ansible_distribution}}-{{ansible_distribution_version}} is not supported yet"
{% endraw %}
{% endhighlight %}

### Host list management
Ansible supports some sort of set operations on host list

1. All hosts means all following tasks will be performed to all available hosts in inventory
 
{% highlight yaml linenos %}
{% raw %}
- hosts: all
{% endraw %}
{% endhighlight %}

2. Reference to host group. Here we are creating new host groups based on previosly created one.

{% highlight yaml linenos %}
{% raw %}
- hosts: all
tasks:
- name: Create a group of all hosts by hardware
  group_by: key={{ansible_product_name}} 

- hosts: PowerEdge-R720xd
tasks:
   - name: Create a group of all hosts by operating system
     group_by: key={{ansible_distribution}}-{{ansible_distribution_version}}
{% endraw %}
{% endhighlight %}

3. Union
{% highlight yaml linenos %}
{% raw %}
- hosts: Ubuntu-14.04:Ubuntu-12.04
tasks:
...
{% endraw %}
{% endhighlight %}

4. Complement
 
{% highlight yaml linenos%}
{% raw %}
- hosts: all:!Ubuntu-14.04:!Ubuntu-12.04:!CentOS-7.1.1503
tasks:
...
{% endraw %}
{% endhighlight %}

## All together

{% highlight yaml linenos %}
{% raw %}
---
- hosts: '{{ target }}'
  tasks:
   - name: Create a group of all hosts by hardware
     group_by: key={{ansible_product_name}}

- hosts: PowerEdge-R720xd
  tasks:
   - name: Create a group of all hosts by operating system
     group_by: key={{ansible_distribution}}-{{ansible_distribution_version}}

- hosts: CentOS-7.1.1503
  tasks:
   - name: Configure watchdog
     [...]

- hosts: Ubuntu-14.04:Ubuntu-12.04
  tasks:
   - name: Configure watchdog
     [...]

- hosts: all:!Ubuntu-14.04:!Ubuntu-12.04:!CentOS-7.1.1503
  tasks:
   - name: Unsupported configuration
     debug: msg="Configuration {{ansible_product_name}} {{ansible_distribution}}-{{ansible_distribution_version}} is not supported yet"
{% endraw %}
{% endhighlight %}

So, starting with no host group by ansible_product_name we are exploring what hardware is present in our fleet, with
adding operation systems groups and substructing them from latest host group we have less and less unknown
configurations. As result this playbook could be improved incrementally with adding new types of configurations.

Sample output

{% highlight console %}
>$ ansible-playbook deploy-watchdog.yaml --ask-sudo

SUDO password:

PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [host1-prod]
ok: [host2-prod]
[...]

TASK [Create a group of all hosts by hardware] *********************************
ok: [host1-prod]
ok: [host2-prod]
[...]

PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [host1-prod]
ok: [host2-prod]
[...]

TASK [Create a group of all hosts by operating system] *************************
ok: [host1-prod]
ok: [host2-prod]
[...]

PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [host1-prod]
ok: [host2-prod]
[...]

TASK [Cconfigure watchdog] **********************************************************
ok: [host1-prod]
ok: [host2-prod]
[...]

TASK [Unsupported configuration] ***********************************************
ok: [host3] => {
    "msg": "Configuration PowerEdge R720xd CentOS-7.1.1503 is not supported yet"
}
ok: [host4] => {
    "msg": "Configuration PowerEdge R720xd CentOS-7.1.1503 is not supported yet"
}
[...]

PLAY RECAP *********************************************************************
host1       : ok=0    changed=0    unreachable=1    failed=0
host2       : ok=0    changed=0    unreachable=1    failed=0
[...]
{% endhighlight %}

[ansible]: https://www.ansible.com/
[group_by_module]: https://docs.ansible.com/ansible/group_by_module.html
[debug_module]: https://docs.ansible.com/ansible/debug_module.html