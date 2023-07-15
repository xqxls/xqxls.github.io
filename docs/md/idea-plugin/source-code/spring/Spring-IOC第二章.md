## 实现Bean的定义、注册和获取

#### 1.需求目标

- 把Bean的创建交给容器，而不是我们在调用时候传递一个实例化好的Bean对象。
- 考虑单例对象，在对象的二次获取时可以从内存中获取对象。

#### 2.设计

- Bean注册的时候只注册一个类信息，而不会直接把实例化信息注册到Spring容器中。那么就需要修改BeanDefinition中的属性Objec为Class。
- 在回去Bean对象时，需要处理Bean对象的实例化操作。
- 通过Map缓存单例信息。

#### 3.类图

![image-20230715235034748](Spring-IOC第二章.assets/image-20230715235034748.png)

#### 4.实现

新增BeanFactory接口，定义getBean接口方法，通过名称获取Bean。新增抽象类AbstractBeanFactory，编排Bean的获取流程：

1. 判断单例池是否存在对象，如果存在，直接返回。
2. 如果不存在，调用getBeanDefinition抽象方法。
3. 如果BeanDefinition池子里存在对象，直接返回。如果不存在，抛异常。
4. 根据获取到的BeanDefinition，调用createBean方法。此处暂时使用JDK动态代理创建Bean，后续可扩展出Cglib方法创建Bean。
5. 注册到单例池，并返回Bean对象。

