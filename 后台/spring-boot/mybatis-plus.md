### mybatis-plus 

#### [官方文档](https://baomidou.com/guide/)、[具体案例](https://www.cnblogs.com/l-y-h/p/12859477.html#_label0_1)

#### 1.初始化（以spring-boot为基础）

Spring Boot Starter 父工程

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
    <relativePath/>
</parent>
```

#### 2.引入依赖

引入 `spring-boot-starter`、`spring-boot-starter-test`、`mybatis-plus-boot-starter`、`lombok`、`h2` 依赖：

**引入 `MyBatis-Plus` 之后请不要再次引入 `MyBatis` 以及 `MyBatis-Spring`，以避免因版本差异导致的问题。**

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <!-- mybatis-plus依赖-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.0</version>
    </dependency>
     <!-- mysql connector -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

#### 3.配置文件

```yml
# DataSource Config
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    #使用mysql-connector/6.以上的版本需要添加serverTimezone=UTC
    url: jdbc:mysql://127.0.0.1:3306/learn?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 13068298041tzc
```

#### 4.编码

##### 1.基础使用

新建一个entity文件夹，准备一个User类，使用到**@Data**

```java
package com.learn.mybatisplus.entity;

import lombok.Data;

@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

新建一个mapper文件夹，准备一个**UserMapper接口**

```java
package com.learn.mybatisplus.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.learn.mybatisplus.entity.User;

public interface UserMapper extends BaseMapper<User> {
	//BaseMapper里面封装了基础的CURD操作    
}
```

在入口函数加上**@MapperScan**注释

```java
@SpringBootApplication
//注释里面的包名必须是完整的
@MapperScan("com/learn/mybatisplus/mapper")
public class MybatisPlusApplication {
```

修