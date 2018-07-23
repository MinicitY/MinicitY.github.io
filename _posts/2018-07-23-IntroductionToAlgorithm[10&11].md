---
layout: post
title:  "算导注解Chapter[10&11]"
date:   2018-07-23
author: MinicitY
categories: Data Structure
tags: 栈 队列 链表 散列表
---

* content
{:toc}

## **Preview**

忙完实习后，终于有时间更新了……这一讲会讨论一些基本的数据结构和散列表。比起算法而言，数据结构应该更通用也更实用些，毕竟小项目可以只招一些API战士，而增删改查 (CRUD boy) 是绝对离不开数据结构的。书上的诸如「指针和对象的实现」就跳过了，我只用 java 和 python 。

keywords: `基本数据结构` `散列表`




## **栈和队列**

通俗地讲，栈实现了**后进先出**（last-in, first-out, i.e. LIFO），而队列实现了**先进先出**（first-in, first-out, i.e. FIFO）。利用静态数组来实现这两种结构的方式和利用动态数组的方式稍稍不一样。

![这里放张图](https://raw.githubusercontent.com/MinicitY/MyImg/master/stack.png)

![](https://raw.githubusercontent.com/MinicitY/MyImg/master/queue.png)

栈可以类比「羽毛球筒」，而队列可以类比「排队」，它们都可以有增删改查等操作。

#### 关于栈

我们可以声明一个数组 array[1...n] 来储存栈，这个栈占据着数组的 array[1] 到 array[S.top]部分（其中`S.top`指向最新插入的元素的下标）。**当我们在push的时候，我们实际在改写array[S.top+1]的值并把S.top+=1；当我们在pop的时候，我们实际在把S.top-=1并返回 array[S.top+1]。** 因此，栈和实现它的数组其实只是相关联的关系，并且我们通过改变S.top来指定数组中的部分元素为栈。

```java
STACK_EMPTY(array)
 if S.top == 1
 return true
 else return false

PUSH(array, x)
 S.top++
 array[S.top] = x
 
POP(array)
 if STACK_EMPTY(array)
 error "underflow"
 else S.top--
 return array[S.top+1]
```

对于空的栈，pop操作会导致栈下溢；对于S.top已经达到n的栈，push操作会导致栈上溢(i.e. stack overflow)。

#### 关于队列

我们依然可以声明一个数组array[1...n]来储存队列，这个队列不如栈那么耿直，它的`Q.head`甚至有可能在`Q.tail`的后面。总的来说更像一个环序，位置1在位置n后面紧紧跟随。当然，我们只需要关注队首和队尾（其中队首Q.head指向队列的最旧的元素，队尾Q.tail指向队列的最新的元素的下一个位置）。**当我们在enqueue的时候，我们实际在改写array[Q.tail]的值并把Q.tail+=1；当我们在dequeue的时候，我们实际在把Q.head+=1并返回 array[Q.head-1]。** 

```java
ENQUEUE(array, x)
 array[Q.tail] = x
 if Q.tail == array.length
  Q.tail = 1
 else
  Q.tail++
  
DEQUEUE(array)
 x = array[Q.head]
 if Q.head = Q.length
  Q.head = 1
 else Q.head++
 return x
```

对于`Q.head=Q.tail`的情况，队列为空，此时如果试图删除队列中的一个元素，则队列发生下溢(underflow)。对于`Q.head=Q.tail+1`的情况，队列为满（有一个空间不用），此时如果试图增加一个元素进队列，则队列发生上溢(overflow)。

上面那个伪代码没考虑上溢和下溢。

## **链表**

现在来谈谈链表。数组的线性顺序由下标决定，因此各类操作（CRUD）都很简单，但是链表的线性顺序由对象内部的指针决定，因此操作有点点变化。

链表可以有序，也可以无序；可以单链，也可以双链；甚至还有循环链表，不过我们统统都不管，现在只讨论无序双向链表。

双向链表中，设有元素x，那么`x.prev`指向前驱，而`x.next`指向后继，`x.prev=NIL`（大概是空指针的一种？）则元素x是链表L的head，`x.next=NIL`则元素x是链表L的tail，`L.head=NIL`则链表L为空。

#### 链表的搜索

```java
LIST-SEARCH(L,k)
 x = L.head
 while (x != NIL && x != k)
  x = x.next
 return x
```

链表的搜索只能采取线性方法。如果链表是有序的，那么可以二分链表来查找（这里不展开）。

#### 链表的插入

```java
LIST-INSERT(L, x)
 
```

#### 链表的删除

## **题目答案**



#### 第十章

10.1-7  
