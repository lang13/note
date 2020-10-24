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
****
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
****
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
****
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
    private Date createTime;
//    @TableField(fill = FieldFill.INSERT_UPDATE) 插入或者更新数据时修改时间
    private Date updateTime;
}
```

新建一个mapper文件夹，准备一个**UserMapper接口**

```java
package com.learn.mybatisplus.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.learn.mybatisplus.entity.User;

@Repository
public interface UserMapper extends BaseMapper<User> {
	//BaseMapper里面封装了基础的CURD操作    
}
```

在入口函数加上**@MapperScan**注释

```java
@SpringBootApplication
//注释里面的包名必须是完整的
@MapperScan("com.learn.mybatisplus.mapper")
public class MybatisPlusApplication {
```
##### 2.条件查询

###### 1. map

map放的是**where name = "张三" and age = 20**

```java
 @Test
    void testSelectByMap() {
        Map<String, Object> map = new HashMap();
        //多条件查询
        map.put("name", "张三");
        map.put("age", 20);

        List<User> users = userMapper.selectByMap(map);
        System.out.println(users);
    }
```
###### 2.条件构造器（Wrapper）

###### 1.多条件查询

```java
void contextLoads(){
    //查询name和邮箱不为空的用户,且年龄>=20岁的
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    //名字不为空
    queryWrapper.isNotNull("name")
        //邮箱不为空
        .isNotNull("email")
        //年龄大于等于20岁
        .ge("age", 21);
    List<User> users = userMapper.selectList(queryWrapper);
    System.out.println(users);
}
```



##### 3.分页查询

###### 1.配置分页拦截器

```java
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    //分页插件
    interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
    return interceptor;
}
/**
  * 新的分页插件,一缓和二缓遵循mybatis的规则,需要设置 MybatisConfiguration#useDeprecatedExecutor = false 避免缓存出现   * 问题
  */
@Bean
public ConfigurationCustomizer configurationCustomizer() {
    return configuration -> configuration.setUseDeprecatedExecutor(false);
}
```

###### 2.分页查询代码

```java
@Test
void testPaginationInnerInterceptor(){
    //查询第一页,且需要5个数据
    Page<User> userPage = new Page<>(5,5);
    userMapper.selectPage(userPage, null);
    userPage.getRecords().forEach(System.out::println);
    System.out.println(userPage.getTotal());
}
```

##### 4.逻辑删除

###### 1.配置文件

```yml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: (逻辑删除的字段名)  # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

###### 2.在**entity**类中加上字段

```java
//    @TableLogic
//新版本在配置了logic-delete-field:后就不需要加@TableLogic标签
private int deleted;
```

###### 3.删除代码

```java
@Test
void testLogic(){
//   userMapper.deleteById(1L);
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```

###### 4.注意

**逻辑删除并不会修改`Version`字段的值**

****
#### 5.代码生成器

##### 1.依赖

```xml
<!--代码生成器-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.2</version>
</dependency>
```

##### 2.代码

```java
//代码生成器
        AutoGenerator ag = new AutoGenerator();

        //1.全局配置
        GlobalConfig gc = new GlobalConfig();
        //项目目录
        String projectPath = System.getProperty("user.dir");
        // 拼接出代码最终输出的目录
        gc.setOutputDir(projectPath + "/src/main/java");
        //设置作者
        gc.setAuthor("lang");
        //生成完毕是否打开文件夹
        gc.setOpen(false);
        //重新生成文件时是否覆盖
        gc.setFileOverride(false);
        // 配置主键生成策略，此处为 ASSIGN_ID（可选）
        gc.setIdType(IdType.ASSIGN_ID);
        // 配置日期类型，此处为 ONLY_DATE（可选）
        gc.setDateType(DateType.ONLY_DATE);
//         实体属性 Swagger2 注解，添加 Swagger 依赖，开启 Swagger2 模式（可选）
//        gc.setSwagger2(true);
        //将全局配置放入代码生成器
        ag.setGlobalConfig(gc);

        //2.数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/learn?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("13068298041tzc");
        //设置数据库类型
        dsc.setDbType(DbType.MYSQL);
        //将dsc放入代码生成器
        ag.setDataSource(dsc);

        //3.包配置
        PackageConfig pc = new PackageConfig();
        //设置模块名
        pc.setModuleName("generator");
        //配置父包名
        pc.setParent("com.learn.mybatisplus");
        //配置entity报名
        pc.setEntity("entity");
        //配置mapper包名
        pc.setMapper("mapper");
        //配置service包名
        pc.setService("service");
        //配置controller名
        pc.setController("controller");
        //将包配置文件放入自动代码生成器
        ag.setPackageInfo(pc);

        //4.策略配置
        StrategyConfig strategy = new StrategyConfig();
        // 指定表名（可以同时操作多个表，使用 , 隔开）（需要修改）
        strategy.setInclude("user");
        // 配置数据表与实体类名之间映射的策略
        strategy.setNaming(NamingStrategy.underline_to_camel);
        // 配置数据表的字段与实体类的属性名之间映射的策略
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 配置 lombok 模式
        strategy.setEntityLombokModel(true);
        // 配置 rest 风格的控制器（@RestController）
        strategy.setRestControllerStyle(true);
        // 配置驼峰转连字符
        strategy.setControllerMappingHyphenStyle(true);
        // 配置表前缀，生成实体时去除表前缀
        // 此处的表名为 test_mybatis_plus_user，模块名为 test_mybatis_plus，去除前缀后剩下为 user。
        strategy.setTablePrefix(pc.getModuleName() + "_");
        ag.setStrategy(strategy);

        ag.execute();
```
****
#### 6.配置日志

> 所有的SQL现在是不可见的，我们希望知道它是怎么执行的，所以需要及配置日志功能

```yml
# 配置日志
mybatis-plus:
  configuration:    #控制台输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```
***
#### 7.乐观锁

添加**config**配置

> 在MyBatis-Plus3.4.0版本，OptimisticLockerInterceptor已被弃置使用，但不添加会**报错**，看后续更新

```java
@Configuration
@EnableTransactionManagement
//@MapperScan("com.learn.mybatis_plus.generator.mapper")
public class MyBatisPlusConfig {
    //配置乐观锁
  	@Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        //乐观锁
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

新建一个**version**字段，并且加上**@Version**字段

```java
@Version //乐观锁注解
private Integer version;
```

****

