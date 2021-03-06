# 队列

![](../.gitbook/assets/01-shu-ju-jie-gou-zhan-dui-lie-.bmp)

* 队列使用LinkedList实现
* `Queue<Integer> queue = new LinkedList<>()`;

### Queue队列

* add\(E e\) : 将元素e插入到队列末尾，如果插入成功，则返回true；如果插入失败（即队列已满），则会抛出异常；
* remove\(\) ：移除队首元素，若移除成功，则返回true；如果移除失败（队列为空），则会抛出异常；
* offer\(E e\) ：将元素e插入到队列末尾，如果插入成功，则返回true；如果插入失败（即队列已满），则返回false；
* poll\(\) ：移除并获取队首元素，若成功，则返回队首元素；否则返回null；
* peek\(\) ：获取队首元素，若成功，则返回队首元素；否则返回null

### Deque 双端队列

* `addFirst(E):void` 在队头添加元素。
* `addLast(E):void` 在队尾添加元素。
* `offerFirst(E):boolean` 在队头添加元素，并返回是否添加成功。
* `offerLast(E):boolean` 在队尾添加元素，并返回是否添加成功。
* `removeFirst():E` 删除队头元素，并返回删除的元素，如果队列为`null`，抛出异常。
* `removeLast():E` 删除队尾元素，并返回删除的元素，如果队列为`null`，抛出异常。
* `pollFirst():E` 删除队头元素，并返回删除的元素，如果队列为`null`，返回`null`。
* `pollLast():E` 删除队尾元素，并返回删除的元素，如果队列为`null`，返回`null`。
* `getFirst():E` 获取队头元素，如果队列为`null`将抛出异常。
* `getLast():E` 获取队尾元素，如果队列为`null`将抛出异常。
* `peekFirst():E` 获取队头元素，如果队列为`null`将返回`null`。
* `peekLast():E` 获取队尾元素，如果队列为`null`将返回`null`。
* `removeFirstOccurrence(Object):boolean` 删除第一次出现的指定元素，并返回是否删除成功。
* `removeFirstOccurrence(Object):boolean` 删除最后一次出现的指定元素，并返回是否删除成功。

