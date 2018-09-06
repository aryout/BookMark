#### CAS ABA

1. [ABA问题我们可以使用JDK的并发包中的AtomicStampedReference和 AtomicMarkableReference来解决](https://www.cnblogs.com/exceptioneye/p/5373498.html)<br>

```
// 用int做时间戳
AtomicStampedReference<QNode> tail = new AtomicStampedReference<CompositeLock.QNode>(null, 0);
int[] currentStamp = new int[1];

// currentStamp中返回了时间戳信息
QNode tailNode = tail.get(currentStamp);
tail.compareAndSet(tailNode, null, currentStamp[0], currentStamp[0] + 1)
```

1. AtomicStampedReference的源代码是如何实现的<br>实际的CAS操作比较的是当前的pair对象和新建的pair对象，pair对象封装了引用和时间戳信息。