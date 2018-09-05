# BookMark
#### IOC
[IOC的实现原理—反射与工厂模式](https://blog.csdn.net/fuzhongmin05/article/details/61614873)
1. 不用反射机制时的工厂模式<br>
  每实现一个子类，就得在工厂类中添加新增子类的判断
2. 使用反射机制实现的工厂模式<br>
  可以通过反射取得接口的实例，只需要传入完整的包和类名<br>
  硬编码
3. 通过属性文件的形式配置所需要的子类[springboot的默认配置注入]<br>
  引用初始化时读取配置文件，将bean名和对应的全路径类对应。工厂用bean名为入参，返回实例<br>
  配置文件
4. 通过注解，将类声明为bean，取代手动添加配置文件或xml配置<br>
  元数据

[Spring IOC原理解读 源码](https://blog.csdn.net/he90227/article/details/51536348)
1. Spring IOC体系结构<br>
2. IoC容器的初始化<br>
3. IOC容器的依赖注入<br>
4. IoC容器的高级特性<br>

#### InnoDB
[InnoDB主要特性、概念和架构](https://blog.csdn.net/qq_28674045/article/details/51721575)

[InnoDB的4个特性](http://www.mamicode.com/info-detail-2264842.html)
1. insert buffer<br>
  innodb使用insert buffer"欺骗"数据库:对于为非唯一索引，辅助索引的修改操作并非实时更新索引的叶子页,而是把若干对同一页面的更新缓存起来做合并为一次性更新操作,转化随机IO 为顺序IO,这样可以避免随机IO带来性能损耗，提高数据库的写性能。<br>
2. [Double write](https://blog.csdn.net/jc_benben/article/details/78967380) <br>
  Double write 是InnoDB在 tablespace上的128个页（2个区）是2MB<br>
  位于共享表空间上的double write buffer实际上也是一个文件。为了解决 partial page write (部分页失效)问题
3. 自适应哈希<br>
  MySQL的Heap存储引擎默认的索引类型为哈希,而InnoDB存储引擎提出了另一种实现方法，自适应哈希索引（adaptive hash index）<br>
  自适应哈希索引通过缓冲池的B+树构造而来，因此建立的速度很快。<br>
4. read-ahead预期<br>
  InnoDB 的预读功能是使用后台线程异步完成的。InnoDB启动了innodb_read_io_threads个后台线程<br>
  InnoDB 提供了两种预读的方式，一种是 Linear read ahead,另外一种是Random read-ahead
 
[InnoDB与Myisam的几大区别](https://blog.csdn.net/lc0817/article/details/52757194)
[InnoDB与Myisam的区别](https://blog.csdn.net/xifeijian/article/details/20316775)

#### 动态代理
[java的动态代理机制详解](https://www.cnblogs.com/xiaoluo501395377/p/3383130.html)
1. InvocationHandler:<br>
  激活柄，需要实现这个接口，被代理的接口作为成员，并实现invoke方法体：method.invoke(subject, args)，subject是被代理的成员
2. Proxy：<br>
  Proxy.newProxyInstance()三个参数分别是ClassLoader，被代理对象的接口列表，包装了被代理对象的激活柄<br>
  通过反射机制动态的生成一个代理类，动态代理基于反射，就和依赖注入一样<br>
  动态代理在运行期通过接口动态生成代理类，所以，代理类必须实现一个接口
  
#### CAS ABA
1. [ABA问题我们可以使用JDK的并发包中的AtomicStampedReference和 AtomicMarkableReference来解决](https://www.cnblogs.com/exceptioneye/p/5373498.html)
```
// 用int做时间戳
AtomicStampedReference<QNode> tail = new AtomicStampedReference<CompositeLock.QNode>(null, 0);
int[] currentStamp = new int[1];

// currentStamp中返回了时间戳信息
QNode tailNode = tail.get(currentStamp);
tail.compareAndSet(tailNode, null, currentStamp[0], currentStamp[0] + 1)
```
2. AtomicStampedReference的源代码是如何实现的<br>
  实际的CAS操作比较的是当前的pair对象和新建的pair对象，pair对象封装了引用和时间戳信息。
  
#### Bean加载顺序
1. 获取配置文件资源<br>
  Resource resource = new ClassPathResource("spring/ioc/beans.xml");<br>
  通过Sring Core模块的工具从本地获得了xml资源，并生成Resource对象
2. XmlBeanFactory.java 创建BeanFactory对象<br>
  DefaultSingletonBeanRegistry 其中有静态对象需要实例化<br>
  DefaultListableBeanFactory创建里面的静态对象实例以及执行里面的静态模块<br>
  实例化了一个存放DefaultListableBeanFactory的map<br>
  执行XmlBeanFactory的构造方法<br>
  this.reader.loadBeanDefinitions(resource); <br>
  loadBeanDefinitions(EncodedResource encodedResource)<br>
  把resource加载到容器中去了。这里是整个资源加载进入的切入点。
3. XmlBeanDefinitionReader.java xml文件规则验证<br>
  doLoadBeanDefinitions<br>
  xml还是包装成了Document委托给DocumentLoader去处理执行<br>
  documentReader.registerBeanDefinitions<br>
  解析xml，doRegisterBeanDefinitions(Element root)
4. DefaultListableBeanFactory.java<br>
  注册bean，registerBeanDefinition，添加到beanDefinitionMap中
#### Bean获取过程
![bean](https://github.com/aryout/BookMark/blob/master/img/bean.png)
1. bean主要通过BeanFactory和ApplicationContext获取<br>
  这里ApplicationContext实际上是继承自BeanFactory的，两者的区别在于BeanFactory对bean的初始化主要是延迟初始化的方式，而ApplicationContext对bean的初始化是在容器启动时即将所有bean初始化完毕<br>
2. AbstractBeanFactory<br>
  final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);<br>
  return getMergedBeanDefinition(beanName, getBeanDefinition(beanName));
3. DefaultListableBeanFactory<br>
  BeanDefinition bd = this.beanDefinitionMap.get(beanName);
