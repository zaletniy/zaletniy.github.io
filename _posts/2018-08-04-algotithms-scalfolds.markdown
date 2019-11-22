---
layout: post
title:  "Interview. Data structures"
date:   2018-08-04 12:44:45 +0200
categories: "cheatsheet"
tags: [algorithms, interview, golang, season_01]
---
#### Linked list

##### Model

{% highlight go %}
type Node struct {
    data int32
    next *Node
}
{% endhighlight %}

##### Iteration through

{% highlight go %}

// node *Node - the first element of llist
for {
      if node==nil{
          break
      }
      //OPERATIONS WITH CURRENT NODE
      node = node.next
}
{% endhighlight %}

##### Links
  + [hackerrank. Print in Reverse](https://www.hackerrank.com/challenges/print-the-elements-of-a-linked-list-in-reverse/submissions)
  + [hackerrank. Inserting a Node Into a Sorted Doubly Linked List](https://www.hackerrank.com/challenges/insert-a-node-into-a-sorted-doubly-linked-list/submissions)
  + [hackerrank. Delete duplicate-value nodes from a sorted linked list](https://www.hackerrank.com/challenges/delete-duplicate-value-nodes-from-a-sorted-linked-list/submissions)

#### Golang
##### Compact If

{% highlight go %}
if stacks = removeFromHighest(stacks); stacks == nil {
            return 0
}
{% endhighlight %}

##### Links
  + [hackerrank. Equal Stacks](https://www.hackerrank.com/challenges/equal-stacks/submissions/code/80440754)
  + [Effective Go](https://web.archive.org/web/20180326130602/https://golang.org/doc/effective_go.html#if)
