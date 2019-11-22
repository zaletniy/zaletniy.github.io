---
layout: post
title:  "Interview. Binary trees"
date:   2018-08-04 12:44:45 +0200
categories: "cheatsheet"
tags: [algorithms, interview, golang, season_01]
---
#### Binary trees

##### Model

{% highlight go %}
type TreeNode struct {
      Val int
      Left *TreeNode
      Right *TreeNode
}
{% endhighlight %}

##### DFS Recurcive skeleton
{% highlight go %}

// example of max node
// assume that all nodes' values are > 0
func dfs(root *TreeNode)int{
    if root == nil{
      // neutral for result output
      return 0
    }

    // doing for both subtrees
    leftMax:=dfs(root.Left)
    rightMax:=dfs(root.Max)

    // CORE LOGIC GOES HERE
    result:=Max(root.Val, leftMax, rightMax)
    // CORE LOGIC ENDS HERE

    return result
}
{% endhighlight %}

##### DFS Iterative skeleton
{% highlight go %}

// example of max node
// assume that all nodes' values are > 0
func dfs(root *TreeNode)int{
    max := 0
    current  := root
    stack := []*TreeNode{}

    for current !=nil || len(stack)!=0{
      // navigating to last Left leaf node for current parent
      // and pushing them to stack to process later
      for current !=nil{
        stack = append(stack, current)
        current = current.Left
      }

      //poping element from stack
      current = stack[len(stack)-1]
      stack = stack[:len(stack)-1]

      // CORE LOGIC GOES HERE
      // we have an element to work with
      max = Max(max, current.Val)
      // CORE LOGIC ENDS HERE

      // pointing to right node now
      // if it doesn't exist we will pick next from stack, or exit the loop
      current = current.Right
    }
    return max
}
{% endhighlight %}

##### BFS Iterative skeleton
{% highlight go %}

// example of max node
// assume that all nodes' values are > 0
func dfs(root *TreeNode)int{
    max := 0
    current  := root
    stack := []*TreeNode{}

    for current !=nil || len(stack)!=0{
      // navigating to last Left leaf node for current parent
      // and pushing them to stack to process later
      for current !=nil{
        stack = append(stack, current)
        current = current.Left
      }

      //poping element from stack
      current = stack[len(stack)-1]
      stack = stack[:len(stack)-1]

      // CORE LOGIC GOES HERE
      // we have an element to work with
      max = Max(max, current.Val)
      // CORE LOGIC ENDS HERE

      // pointing to right node now
      // if it doesn't exist we will pick next from stack, or exit the loop
      current = current.Right
    }
    return max
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
