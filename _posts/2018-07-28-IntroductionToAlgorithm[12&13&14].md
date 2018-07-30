---
layout: post
title:  "算导注解Chapter[12&13&14]"
date:   2018-07-28
author: MinicitY
categories: Data_Structure
tags: 二叉搜索树 红黑树 AVL树
---

* content
{:toc}

## **Preview**

先修知识：一棵树不存在回路。其中除了根结点以外所有的结点都有父结点（或者说父结点为NIL），除了叶子以外所有的结点都有子结点（或者说子结点为NIL）。二叉树描述的是包含叶子和根在内的所有结点，都最多只有两个子结点（或者子树）的树结构。二叉树体系内最重要的一种叫做二叉搜索树（又叫二叉查找树），再往下分，二叉搜索树又包括平衡二叉查找树（AVL树）和红黑树等等。




## **二叉搜索树**

可以假定一棵二叉搜索树中的所有结点**必然拥有**下列属性：_left, right, p_ ——它们分别指向结点的左孩子(left child)、右孩子(right child)和父结点(parent)。对于根结点和叶子而言，父结点/子结点指向NIL。另外，二叉搜索树还有这样的性质：`node.left.key <= node.key <= node.right.key`

既然是一颗用来查找的树，那就必须要遍历了。二叉树中有三种常见的遍历：_中序遍历(inorder)、前序遍历(preorder)和后序遍历(postorder)_

遍历具体方式不再赘述，直接给个图：
![](https://raw.githubusercontent.com/MinicitY/MyImg/master/%E4%BA%8C%E5%8F%89%E6%A0%91%E9%81%8D%E5%8E%86.png)

考虑到二叉搜索树的性质，想把树中的关键字从小到大遍历出来，就肯定要用中序遍历了。

```java
INORDER-TREE-WALK(x)//中序遍历的伪代码
if (x != NIL){
 INORDER-TREE-WALK(x.left)
 print x.key
 INORDER-TREE-WALK(x.right)
}
```

我们假设处理一颗空的结点(只有一个结点NIL)需要一个**非常小的常数时间c**，而print一个结点的关键字需要常数时间d。显然地，一颗结点数为n的非空二叉树，应该有一个根节点和⌈n/2⌉个子结点，其中根节点的父结点指向NIL，子结点的两个子结点也指向NIL，所以这个非空二叉树起码要处理(n+1)个空结点。因此我们得到中序遍历的T(n)上界： `T(n) <= c*(n+1) + dn`，也就是说，调用中序遍历需要Θ(n)时间。

## **AVL树**

书上没讲AVL，这里按学校的课件来扯一下。

AVL是一种高度平衡的二叉搜索树。

## **红黑树**

#### 红黑树的扩张