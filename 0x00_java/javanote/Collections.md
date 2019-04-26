## Collections
是集合类的一个工具类/帮助类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作。
```java
Collections.sort(list);
// 对指定列表按升序进行排序

Collections.Shuffling(list)
// 基于随机源的输入重排该 List(洗牌)

Collections.reverse(list)
// 对指定列表按降序进行排序

Collections.fill(li,"aaa");
// 替换所有的元素

Collections.copy(list,li)
// 用两个参数，一个目标 List 和一个源 List, 将源的元素拷贝到目标，并覆盖它的内容。
// 目标 List 至少与源一样长。如果它更长，则在目标 List 中的剩余元素不受影响。
// 前面一个参数是目标列表 ,后一个是源列表。

Collections.min(list)
Collections.max(list)

int count = Collections.lastIndexOfSubList(list,li);
// 返回指定源列表中最后一次出现指定目标列表的起始位置

int count = Collections.indexOfSubList(list,li);
// 返回指定源列表中第一次出现指定目标列表的起始位置

Collections.rotate(list,-1);
// 根据指定的距离循环移动指定列表中的元素
// 如果是负数，则正向移动，正数则方向移动
```

[参考链接](https://www.cnblogs.com/cathyqq/p/5279859.html)