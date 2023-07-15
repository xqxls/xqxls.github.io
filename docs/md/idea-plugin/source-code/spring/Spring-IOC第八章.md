## Aware感知容器对象

#### 1.需求目标

- 提供感知容器对象的能力，使得用户端可以获得Spring框架提供的BeanFactory、ApplicationContext、BeanClassLoader等组件的能力。

#### 2.设计

- 定义 Aware接口，标记类接口，实现该接口可以被Spring容器感知。
- 定义 BeanFactoryAware接口，实现此接口，既能感知到所属的 BeanFactory。
- 定义 BeanNameAware接口，实现此接口，既能感知到所属的 BeanName。
- 定义 BeanClassLoaderAware接口，实现此接口，既能感知到所属的 BeanClassLoader。
- 定义 ApplicationContextAware接口，实现此接口，既能感知到所属的 ApplicationContext。
- 新增 ApplicationContextAwareProcessor类，由于 ApplicationContext 的获取并不能直接在创建 Bean 时候就可以拿到，所以需要在 refresh 操作时，把 ApplicationContext 写入到一个包装的 BeanPostProcessor 中去，再由 AbstractAutowireCapableBeanFactory.applyBeanPostProcessorsBeforeInitialization 方法调用。
- 注册 BeanPostProcessor，refresh方法新增关于 addBeanPostProcessor 的操作。
- 在 initializeBean 中，通过判断 bean instanceof Aware，获取到属于BeanFactoryAware、BeanNameAware、BeanClassLoaderAware中哪一种bean，然后设置对应的BeanFactory、BeanName、BeanClassLoader。

#### 3.Bean生命周期

![image-20230713153916544](/Users/huzhuo/Documents/笔记/Spring-IOC 第八章.assets/image-20230713153916544.png)

#### 4.类图

![感知容器](/Users/huzhuo/Documents/笔记/Spring-IOC 第八章.assets/感知容器.png)

