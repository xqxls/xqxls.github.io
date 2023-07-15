## 注入属性和依赖对象

#### 1.需求目标

- 增加注入属性的流程。
- 支持包含依赖对象的属性注入。

#### 2.设计

- 在AbstractAutowireCapableBeanFactory类中，增加applyPropertyValues方法，调用的时机为Bean实例化之后。
- 新增PropertyValues、PropertyValue、BeanReference类，BeanReference类是为了支持包含依赖对象的属性注入。

#### 3.类图

![image-20230710174429135](/Users/huzhuo/Documents/笔记/Spring-IOC 第四章.assets/image-20230710174429135.png)

#### 4.时序图

![image-20230710174621893](/Users/huzhuo/Documents/笔记/Spring-IOC 第四章.assets/image-20230710174621893.png)

