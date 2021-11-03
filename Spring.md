# Spring 常用注解说明



## @value

在 Spring 组件中使用 @Value 注解的方式，很方便的读取 properties 文件的配置值。


使用场景

```java
// 声明的变量中使用
public static class FieldValueTestBean {
    @Value("#{ systemProperties['user.region'] }")
    private String defaultLocale;
}
```

```
# setter 方法中
```

