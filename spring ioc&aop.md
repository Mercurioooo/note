##### Spring IOC

1. ioc 解决的问题:随用随取,不去要关心对象的过程

2. ioc技术实现方式:构造方法注入和setter注入

3. ioc容器:需要实现的两个技术:对象的构建,对象的绑定
   实现的两种方式:BeanFactory . ApplicationContext

   - BeanFactory : BeanDefinition,BeanDefinationFactory,BeanFactory

     Bean的生成:

     - 容器启动:

       加载配置文件
       通过reader生成BeanDefinition 

       BeanDefinition注册到BeanDefinationRegistry

     - bean实例化:

       实例化对象(getBean的时候完成对bean的初始化,beanDefinition只完成对bean的定义)

       装配依赖

       生命周期回调

       对象其他处理:配置参数(propertyPlaceholderConfigurer)

       注册回调接口

   - ApplicationContext:建立在BeanFactory之上,拥有beanFactory的所有功能

     区别:

     1. bean的生成方式；

        读取配置文件的方式:

        ​	FileSystemXmlApplicationContext

        ​	ClassPathXmlApplicationContext

        ​	WebXmlApplicationContext

        ​	AnnotationConfigApplicationContext

        ​	ConfigurableWebApplicationContext

        ApplicationContext启动阶段完成所有的初始化，并不会等到getBean()才执行。所以，相对于BeanFactory来 说，ApplicationContext要求更多的系统资源，同时，因为在启动时就完成所有初始化，容 器启动时间较之BeanFactory也会长一些。在那些系统资源充足，并且要求更多功能的场景中， ApplicationContext类型的容器是比较合适的选择。

     2. 扩展了BeanFactory的功能，提供了更多企业级功能的支持。

        - ApplicationEventPublisher 介绍Spring事件的时候再讲
        - ResourceLoader 进行资源加载
        - MessageResource 提供国际化支持

##### Spring AOP

