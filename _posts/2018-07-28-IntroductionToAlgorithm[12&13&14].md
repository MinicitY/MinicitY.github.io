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

可以假定一棵二叉搜索树中的所有结点**必然拥有**下列属性：_left, right, p_ ——它们分别指向结点的左孩子(left child)、右孩子(right child)和父结点(parent)。对于根结点和叶子而言，父结点/子结点指向NIL。另外，二叉搜索树还有这样的性质：`node.left.key <= node.key <= node.right.key` **这里的left和right指左右子树中所有的结点**。

#### 遍历

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

我们假设处理一颗空的结点(只有一个结点NIL)需要一个**非常小的常数时间c**，而print一个结点的关键字需要常数时间d。显然地，一颗结点数为n的非空二叉树，应该有一个根节点和⌈n/2⌉个子结点，其中根节点的父结点指向NIL，子结点的两个子结点也指向NIL，所以这个非空二叉树起码要处理(n+1)个空结点。因此我们得到中序遍历的T(n)上界： `T(n) <= c*(n+1) + dn`，也就是说，**调用中序遍历需要Θ(n)时间。**

#### 查询

如果我们想查询一个元素，就没有必要先遍历再查询了。利用被查询元素的关键字的大小比较，可以很快地在二叉搜索树中找到位置。

```java
TREE-SEARCH(x, k)//x是当前结点，k是被查询元素的关键字
 if(x == NIL || k == x.key)//如果查到了尽头或者查到了正确解
  return x
 if(k < x.key)
  return TREE-SEARCH(x.left, k)//姑且也算递归吧……
 else
  return TREE-SEARCH(x.right, k)
```

查询时间为O(h)，其中h是这棵树的高度。我们之前在堆排序中就提到过，如果是堆这样的结构的话，树高应该就是logn；然而树的形状可以很多变所以只能说查询时间为**Ω(logn), O(n)。**

当然，这个伪代码也可以写成迭代的形式——

```java
ITERATIVE-TREE-SEARCH(x, k)
 while(x != NIL && k != x.key){
  if (k < x.key)
   x = x.left
  else
   x = x.right
}
 return x
```

如果要查询最大或最小的结点，往右或者往左一直迭代就是了。如果要查询一个结点的前驱和后继，就需要分情况讨论（以查询后继为例子）：

1. 存在右子树，则迭代查询右子树的最小结点

2. 不存在右子树，且这个结点是父结点的左子结点，则返回父结点

3. 不存在右子树，且这个结点是父结点的右子节点，则需要用到循环：
```java
y = x.p
while(y != NIL && x == y.right)
 x = y
 y = y.p
return y
```

当然，如果树内存在相同的关键字，我们依然用上面的讨论来定义一个结点的前驱和后继。

#### 插入和删除

插入和删除的重点在于如何维持二叉搜索树的性质的成立。先来看插入。

```java
TREE-INSERT(T, z)//对树T插入一个结点z
 y = NIL
 x = T.root//x为结点z的暂时插入目标
 while(x != NIL){
  y = x//y一直跟随x
  if(z.key < x.key)
   x = x.left
  else
   x = x.right   
}
//此时x应该为某个叶子的一个空子结点
 z.p = y
 if(y == NIL)//也就意味着树T为空
  T.root = z
 else if(z.key < y.key)
  y.left = z
 else
  y.right = z
```

插入的关键在于使用双指针沿树下移，指针x记录了向下移动的简单路径，而指针y作为遍历指针始终保持指向x的父结点。插入的运行时间上界为O(h)

再来看删除。删除一个结点z分为以下三种情况：

1. 如果z没有子结点，那么就只用把它删除，并用NIL替代它父结点的子结点。
2. 如果z只有一个子结点，那么删除后将这个子结点提到原本z的位置上，并修改父结点的子结点指针的指向。
3. 如果z有两个子结点（这是最麻烦的一种情况），



## **AVL树**

书上没讲AVL，这里按学校的课件来扯一下。

AVL是一种高度平衡的二叉搜索树。

## **红黑树**

#### 红黑树的扩张