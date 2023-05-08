## 简介
这是一个微服务权限系统(github地址：https://github.com/xqxls/mall-admin-cloud)

## 项目内容

#### 1.项目模块构建

- cloud-gateway（网关）
- cloud-auth（认证服务）
- cloud-mall-admin（业务）
- cloud-mall-admin-rpc（业务服务rpc）

#### 2.登录认证

- jwt
- oauth2
- spring security

#### 3.功能开发

- 登录认证
- 动态路由鉴权
- 用户管理
- 角色管理
- 菜单管理
- 资源管理
- 资源分类管理

#### 4.基础组件

- redis缓存服务封装
- 基于雪花算法封装Id生成器
- 自定义BaseMapper实现基本增删改查
- AOP组件，自定义注解用于方法监控、缓存异常处理等
- BaseMapper扩展insertBatch方法支持批量插入、批量删除

## 软件架构

|         技术         |     版本      |      说明      |
| :------------------: | :-----------: | :------------: |
|     Spring Boot      |     2.6.3     |      框架      |
|     Spring-Cloud     |   2021.0.3    |   微服务框架   |
| Spring-Cloud-Alibaba |  2021.0.1.0   | 微服务扩展框架 |
|        Oauth2        | 2.2.5.RELEASE |    认证框架    |
|         JWT          |     9.23      |    登录支持    |
|        MySQL         |       8       |     数据库     |
|      tkMyBatis       |     2.0.2     |     持久层     |
|       Swagger2       |     3.0.0     |      文档      |
|        Redis         |      5.0      |      缓存      |
|   Spring-Security    |     2.6.3     |    安全框架    |
|      OpenFeign       |     3.1.3     |    rpc支持     |



## 使用说明

- sql：建表语句、初始化数据


