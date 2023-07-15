## 实现应用上下文

#### 1.需求目标

- 对容器中Bean的实例化过程添加扩展机制。
- 开发 Spring 的应用上下文，把相应的 XML 加载 、注册、实例化以及新增的修改和扩展都融合进去，让 Spring 可以自动扫描到我们的新增服务，便于用户使用。

#### 2.设计

- 定义 BeanFactoryPostProcessor接口，这个接口是满足于在所有的 BeanDefinition 加载完成后，实例化 Bean 对象之前，提供修改 BeanDefinition 属性的机制。

- 定义 BeanPostProcessor接口，提供修改新实例化 Bean 对象的扩展点。此接口提供了两个方法：postProcessBeforeInitialization 用于在 Bean 对象执行初始化方法之前，执行此方法，postProcessAfterInitialization用于在 Bean 对象执行初始化方法之后，执行此方法。

- 定义 ApplicationContext应用上下文接口，继承于 ListableBeanFactory，也就继承了关于 BeanFactory 方法，比如一些 getBean 的方法。

- 定义 ConfigurableApplicationContext接口，继承自 ApplicationContext，并提供了 refresh 这个核心方法。

- 定义 AbstractApplicationContext抽象类，继承 DefaultResourceLoader 是为了处理 spring.xml 配置资源的加载。接下来是refresh()方法定义刷新容器的流程：

  1. 创建 BeanFactory，并加载 BeanDefinition
  2. 获取 BeanFactory
  3. 在 Bean 实例化之前，执行 BeanFactoryPostProcessor
  4. BeanPostProcessor 需要提前于其他 Bean 对象实例化之前执行注册操作
  5. 提前实例化单例Bean对象

  另外把定义出来的抽象方法，refreshBeanFactory()、getBeanFactory() 由后面的继承此抽象类的其他抽象类实现。

- 定义 AbstractRefreshableApplicationContext抽象类，在refreshBeanFactory() 中主要是获取了 DefaultListableBeanFactory 的实例化以及对资源配置的加载操作 loadBeanDefinitions(beanFactory)，在加载完成后即可完成对 spring.xml 配置文件中 Bean 对象的定义和注册，同时也包括实现了接口 BeanFactoryPostProcessor、BeanPostProcessor 的配置 Bean 信息。

- 定义 AbstractXmlApplicationContext抽象类，在 AbstractXmlApplicationContext 抽象类的 loadBeanDefinitions 方法实现中，使用 XmlBeanDefinitionReader 类，处理了关于 XML 文件配置信息的操作。同时这里又留下了一个抽象类方法，getConfigLocations()，此方法是为了从入口上下文类，拿到配置信息的地址描述。

- 定义 ClassPathXmlApplicationContext应用上下文实现类，在继承了 AbstractXmlApplicationContext 以及层层抽象类的功能分离实现后，在此类 ClassPathXmlApplicationContext 的实现中就简单多了，主要是对继承抽象类中方法的调用和提供了配置文件地址信息。

- 在Bean创建时完成前置和后置处理，也就是需要在创建 Bean 对象时，在 createBean 方法中添加 initializeBean(beanName, bean, beanDefinition) 操作。而这个操作主要是对于方法 applyBeanPostProcessorsBeforeInitialization、applyBeanPostProcessorsAfterInitialization 的使用。

#### 3.类图

![spring应用上下文](/Users/huzhuo/Documents/笔记/Spring-IOC 第六章.assets/spring应用上下文.png)

#### 4.时序图

![应用上下文时序图](/Users/huzhuo/Documents/笔记/Spring-IOC 第六章.assets/应用上下文时序图.png)

