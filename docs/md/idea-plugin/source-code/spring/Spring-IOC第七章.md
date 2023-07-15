## 初始化和销毁方法

#### 1.需求目标

- 将Bean的初始化和销毁操作交给Spring管理。

#### 2.设计

- 定义 InitializingBean接口，包含一个afterPropertiesSet方法，用于Bean处理了属性填充后调用。
- 定义 DisposableBean接口，包含一个destroy方法，用于Bean销毁时释放资源。
- BeanDefinition新增加了两个属性：initMethodName、destroyMethodName，这两个属性是为了在 spring.xml 配置的 Bean 对象中，可以配置 init-method="initDataMethod" destroy-method="destroyDataMethod" 操作，最终实现接口的效果是一样的。只不过一个是接口方法的直接调用，另外是一个在配置文件中读取到方法反射调用。同时需要在XmlBeanDefinitionReader解析类中增加对initMethodName、destroyMethodName的处理。
- 在 AbstractAutowireCapableBeanFactory中增加initializeBean方法，并在方法initializeBean内的 invokeInitMethods 方法中，增加对Bean初始化的操作。主要分为两块来执行：一块实现了 InitializingBean 接口的操作，处理 afterPropertiesSet 方法。另外一块是判断配置信息 init-method 是否存在，执行反射调用 initMethod.invoke(bean)。这两种方式都可以在 Bean 对象初始化过程中进行处理加载 Bean 对象中的初始化操作，让使用者可以额外新增加自己想要的动作。
- 定义销毁方法适配器，销毁方法的具体实现与初始化方法类似。目前有实现接口 DisposableBean、配置信息 destroy-method，两种方式。而这两种方式的销毁动作是由 AbstractApplicationContext 在注册虚拟机钩子后，看虚拟机关闭前执行的操作动作。

- 创建Bean时注册销毁方法对象。这个销毁方法的具体方法信息，会被注册到 DefaultSingletonBeanRegistry 中新增加的 Map<String, DisposableBean> disposableBeans 属性中去，因为这个接口的方法最终可以被类 AbstractApplicationContext 的 close 方法通过 getBeanFactory().destroySingletons() 调用。
- 虚拟机关闭钩子注册调用销毁方法。在 ConfigurableApplicationContext 接口中定义注册虚拟机钩子的方法 registerShutdownHook 和手动执行关闭的方法 close。

#### 3.类图

![初始化和销毁方法](/Users/huzhuo/Documents/笔记/Spring-IOC 第七章.assets/初始化和销毁方法.png)

#### 4.时序图

![ApiTest_test_xml](/Users/huzhuo/Documents/笔记/Spring-IOC 第七章.assets/ApiTest_test_xml.png)

