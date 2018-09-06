#### Bean加载顺序

1. 获取配置文件资源<br>Resource resource = new ClassPathResource("spring/ioc/beans.xml");<br>通过Sring Core模块的工具从本地获得了xml资源，并生成Resource对象
2. XmlBeanFactory.java 创建BeanFactory对象<br>DefaultSingletonBeanRegistry 其中有静态对象需要实例化<br>DefaultListableBeanFactory创建里面的静态对象实例以及执行里面的静态模块<br>实例化了一个存放DefaultListableBeanFactory的map<br>执行XmlBeanFactory的构造方法<br>this.reader.loadBeanDefinitions(resource); <br>loadBeanDefinitions(EncodedResource encodedResource)<br>把resource加载到容器中去了。这里是整个资源加载进入的切入点。
3. XmlBeanDefinitionReader.java xml文件规则验证<br>doLoadBeanDefinitions<br>xml还是包装成了Document委托给DocumentLoader去处理执行<br>documentReader.registerBeanDefinitions<br>解析xml，doRegisterBeanDefinitions(Element root)
4. DefaultListableBeanFactory.java<br>注册bean，registerBeanDefinition，添加到beanDefinitionMap中

#### Bean获取过程

![bean](https://github.com/aryout/BookMark/blob/master/img/bean.png)

1. bean主要通过BeanFactory和ApplicationContext获取<br>这里ApplicationContext实际上是继承自BeanFactory的，两者的区别在于BeanFactory对bean的初始化主要是延迟初始化的方式，而ApplicationContext对bean的初始化是在容器启动时即将所有bean初始化完毕<br>
2. AbstractBeanFactory<br>final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);<br>return getMergedBeanDefinition(beanName, getBeanDefinition(beanName));
3. DefaultListableBeanFactory<br>BeanDefinition bd = this.beanDefinitionMap.get(beanName);