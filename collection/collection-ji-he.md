# Collection集合

![](../.gitbook/assets/01-ji-he-kuang-jia-jie-shao-%20%282%29%20%282%29.bmp)

## 概述

* 学习顶层，使用底层

## List接口

1. 有序集合（存储和取出元素顺序相同）
2. 允许存储重复的元素
3. 有索引，可以使用普通for循环

## Set接口

1. 不允许存储重复元素
2. 没有索引（不能使用普通的for循环遍历）

## 常用方法

* `public boolean add(E e)`：  把给定的对象添加到当前集合中 。
* `public void clear()` :清空集合中所有的元素。
* `public boolean remove(E e)`: 把给定的对象在当前集合中删除。
* `public boolean contains(E e)`: 判断当前集合中是否包含给定的对象。
* `public boolean isEmpty()`: 判断当前集合是否为空。
* `public int size()`: 返回集合中元素的个数。
* `public Object[] toArray()`: 把集合中的元素，存储到数组中。



