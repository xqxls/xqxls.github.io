## 通过注解注入属性信息

#### 1.需求目标

- 实现对属性和对象的注入操作。

#### 2.设计

- 定义 StringValueResolver接口，用于解析占位符中的属性，返回属性值。
- 修改 PropertyPlaceholderConfigurer类，向beanFactory嵌入字符串解析接口StringValueResolver。
- 自定义属性注入注解 Autowired、Qualifier、Value，用于注入对象、注入属性，而 Qualifier 一般与 Autowired 配合使用。
- 增加 AutowiredAnnotationBeanPostProcessor类， 是实现接口 InstantiationAwareBeanPostProcessor 的一个用于在 Bean 对象实例化完成后，设置属性操作前的处理属性信息的类和操作方法。只有实现了 BeanPostProcessor 接口才有机会在 Bean 的生命周期中处理初始化信息。核心方法 postProcessPropertyValues，主要用于处理类含有 @Value、@Autowired 注解的属性，进行属性信息的提取和设置。
- 修改 ClassPathBeanDefinitionScanner类，手动注册AutowiredAnnotationBeanPostProcessor到BeanDefinition注册表中。
- 修改 AbstractAutowireCapableBeanFactory类，在AbstractAutowireCapableBeanFactory#createBean 方法实例化Bean之后，填充属性之前新增applyBeanPostProcessorsBeforeApplyingPropertyValues方法，获取已经注入的 BeanPostProcessor 集合并从中筛选出继承接口 InstantiationAwareBeanPostProcessor 的实现类，调用相应的 postProcessPropertyValues 方法以及循环设置属性值信息。

#### 3.类图

![通过注解注入属性信息类图](Spring-AOP第十四章.assets/通过注解注入属性信息类图.png)

#### 4.原理图

![通过注解注入属性值原理图](Spring-AOP第十四章.assets/通过注解注入属性值原理图.png)
