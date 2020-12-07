### spring框架

#### 控制反转(IOC)

> 将本身创建依赖对象过程交给外部容器创建和维护的行为叫做控制反转

IOC容器：一个Map，有key和value

#### 依赖注入

> 程序运行期间，动态的将依赖对象实例注入到该程序（对象）的过程

#### spring对自己定义的有不同属性的Java Bean进行统一管理

###### Bean的抽象定义

**BeanDefinition**：beanClassName、beanName、scope（作用域）

> 将Bean类再次封装

#### 初始化spring中的Bean

```java
ApplicationContext context = new AnnotationConfigApplicationContext("com.learn.bean")   //参数是要扫描的包路径
//获取Bean对象
User user = context.getBean("User", User.class);
```

spring容器中对象实例的别名

> 1. 默认为类的首字母小写
> 2. 定义注解时指定的别名称
> 3. 通过@Bean手动注册的对象的别名称为定义方法的名称