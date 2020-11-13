### [官网](https://github.com/atlassian/commonmark-java)

#### 基本实用

##### 基本依赖

```xml
<dependency>
    <groupId>com.atlassian.commonmark</groupId>
    <artifactId>commonmark</artifactId>
    <version>0.15.2</version>
</dependency>
```

基本实用

```java
import org.commonmark.node.*;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.html.HtmlRenderer;

Parser parser = Parser.builder().build();
Node document = parser.parse("This is *Sparta*");
HtmlRenderer renderer = HtmlRenderer.builder().build();
renderer.render(document);  // "<p>This is <em>Sparta</em></p>\n"
```

#### 扩展

##### table标签的扩展

###### 依赖

```xml
<dependency>
    <groupId>com.atlassian.commonmark</groupId>
    <artifactId>commonmark-ext-gfm-tables</artifactId>
    <version>0.15.2</version>
</dependency>
```

##### h标签的扩展

###### 依赖

```xml
<dependency>
    <groupId>com.atlassian.commonmark</groupId>
    <artifactId>commonmark-ext-heading-anchor.</artifactId>
    <version>0.15.2</version>
</dependency>
```

