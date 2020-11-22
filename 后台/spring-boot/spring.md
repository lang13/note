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
new AnnotationConfigApplicationContext("com.learn.bean")
```

