## 创建简单的Bean容器

#### 1.需求目标

实现一个Bean容器，支持Bean的注册和获取。

#### 2.设计

构造一个BeanFactory工厂，通过Map存储Bean信息，Map的键是Bean的名称，值是一个包含Bean信息的定义类。

#### 3.实现

包含三个步骤：

1. 初始化 BeanFactory
2. 注入Bean
3. 获取Bean

