---
title: EnableAutoConfiguration注解
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## @EnableAutoConfiguration注解

@EnableAutoConfiguratioon注解内部使用@Import(AutoConfigurationImportSelector.class)来加载配置类。

配置文件位置：META-INF/spring.factories，该配置文件中定义了大量的配置类，当SpringBoot应用启动时，会自动加载这些配置类，初始化Bean

并不是所有的Bean都会被初始化，在配置类中使用Condition来加载满足条件的Bean

点击进入@SpringBootApplication注解内部

![image-20230913110250750](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131102812.png)

里面有EnableAutoConfiguration点击进入查看

![image-20230913110333412](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131103399.png)

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
// 导入了自动配置的Selector
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {

	/**
	 * Environment property that can be used to override when auto-configuration is
	 * enabled.
	 */
	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	Class<?>[] exclude() default {};

	/**
	 * Exclude specific auto-configuration class names such that they will never be
	 * applied.
	 * @return the class names to exclude
	 * @since 1.3.0
	 */
	String[] excludeName() default {};

}
```

在AutoConfigurationImportSelector中重写了selectImports

```java
@Override
// String[]数组返回定义了很多需要被加载的类
public String[] selectImports(AnnotationMetadata annotationMetadata) {
   if (!isEnabled(annotationMetadata)) {
      return NO_IMPORTS;
   }                                              // 通过getAutoConfigurationEntry加载后返回AutoConfigurationEntry对象
   AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
   // 将autoConfigurationEntry转换为字符串数组
   return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
}
```

查看核心的源代码：

```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
   // 通过SpringFactoriesLoader加载并返回List集合，这个List集合中就是所有需要加载的类
   List<String> configurations = new ArrayList<>(
      SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader()));
   ImportCandidates.load(AutoConfiguration.class, getBeanClassLoader()).forEach(configurations::add);
   // 如果上面List集合是空的则下面断言抛出错误信息
   Assert.notEmpty(configurations,
                   "No auto configuration classes found in META-INF/spring.factories nor in META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports. If you "
                   + "are using a custom packaging, make sure that file is correct.");
   return configurations;
}
```

如果没有定义META-INF/spring.factories 文件或者这个文件有问题就会报configurations集合加载不到，加载不断就断言失败，失败报出异常，下面看下spring.factories文件中定义了哪些需要被自定加载的类

找到spring-boot-autoconfigure的依赖包

![image-20230913145220114](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131452748.png)

SpringBoot启动的时候并不是都会加载这些Bean，而是判断满足ConditionalOnClass注解的条件后才会加载到IOC容器中

![image-20230913145708405](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131457852.png)

一般SpringBoot官方提供的依赖功能名写在最后面，而第三方提供的功能名写在最前面

![image-20230913150659200](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131507335.png)