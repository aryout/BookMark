#### IOC
[IOC的实现原理—反射与工厂模式](https://blog.csdn.net/fuzhongmin05/article/details/61614873)
1. 不用反射机制时的工厂模式<br>每实现一个子类，就得在工厂类中添加新增子类的判断
2. 使用反射机制实现的工厂模式<br>可以通过反射取得接口的实例，只需要传入完整的包和类名<br>硬编码
3. 通过属性文件的形式配置所需要的子类[springboot的默认配置注入]<br>引用初始化时读取配置文件，将bean名和对应的全路径类对应。工厂用bean名为入参，返回实例<br>配置文件
4. 通过注解，将类声明为bean，取代手动添加配置文件或xml配置<br>元数据

[Spring IOC原理解读 源码](https://blog.csdn.net/he90227/article/details/51536348)

1. Spring IOC体系结构<br>
2. IoC容器的初始化<br>
3. IOC容器的依赖注入<br>
4. IoC容器的高级特性<br>

#### 动态代理

[java的动态代理机制详解](https://www.cnblogs.com/xiaoluo501395377/p/3383130.html)

1. InvocationHandler:<br>激活柄，需要实现这个接口，被代理的接口作为成员，并实现invoke方法体：method.invoke(subject, args)，subject是被代理的成员
2. Proxy：<br>Proxy.newProxyInstance()三个参数分别是ClassLoader，被代理对象的接口列表，包装了被代理对象的激活柄<br>通过反射机制动态的生成一个代理类，动态代理基于反射，就和依赖注入一样<br>动态代理在运行期通过接口动态生成代理类，所以，代理类必须实现一个接口