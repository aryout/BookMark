#### InnoDB

[InnoDB主要特性、概念和架构](https://blog.csdn.net/qq_28674045/article/details/51721575)

[InnoDB的4个特性](http://www.mamicode.com/info-detail-2264842.html)

1. insert buffer<br>innodb使用insert buffer"欺骗"数据库:对于为非唯一索引，辅助索引的修改操作并非实时更新索引的叶子页,而是把若干对同一页面的更新缓存起来做合并为一次性更新操作,转化随机IO 为顺序IO,这样可以避免随机IO带来性能损耗，提高数据库的写性能。
2. [Double write](https://blog.csdn.net/jc_benben/article/details/78967380) <br>Double write 是InnoDB在 tablespace上的128个页（2个区）是2MB
   位于共享表空间上的double write buffer实际上也是一个文件。为了解决 partial page write (部分页失效)问题
3. 自适应哈希<br>MySQL的Heap存储引擎默认的索引类型为哈希,而InnoDB存储引擎提出了另一种实现方法，自适应哈希索引（adaptive hash index）
   自适应哈希索引通过缓冲池的B+树构造而来，因此建立的速度很快。
4. read-ahead预期<br>InnoDB 的预读功能是使用后台线程异步完成的。InnoDB启动了innodb_read_io_threads个后台线程
   InnoDB 提供了两种预读的方式，一种是 Linear read ahead,另外一种是Random read-ahead

[InnoDB与Myisam的几大区别](https://blog.csdn.net/lc0817/article/details/52757194)

[InnoDB与Myisam的区别](https://blog.csdn.net/xifeijian/article/details/20316775)