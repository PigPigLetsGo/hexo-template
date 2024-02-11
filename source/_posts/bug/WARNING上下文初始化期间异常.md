---
title: WARNING上下文初始化期间异常
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - SpringBoot
---

问题描述

- 可能引发异常的做法:
    - 将Spring创建工厂改为使用绝对路径来获取applicationContext.xml

```java
//        使用applicationContext.xml绝对路径来加载配置文件
        ApplicationContext c = new FileSystemXmlApplicationContext("E:\\apache-maven-3.6.3\\maven-work-space\\space-idea\\maven-spring-Demo06\\src\\main\\resources\\applicationContext.xml");
```

然后可能会引发这种异常的出现

```bash
Feb 24, 2023 3:42:49 PM org.springframework.context.support.AbstractApplicationContext refresh
WARNING: Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanDefinitionStoreException: Invalid bean definition with name 'dataSource' defined in file [E:\apache-maven-3.6.3\maven-work-space\space-idea\maven-spring-Demo06\src\main\resources\applicationContext.xml]: Could not resolve placeholder 'jdbc.driver' in value "${jdbc.driver}"; nested exception is java.lang.IllegalArgumentException: Could not resolve placeholder 'jdbc.driver' in value "${jdbc.driver}"

org.springframework.beans.factory.BeanDefinitionStoreException: Invalid bean definition with name 'dataSource' defined in file [E:\apache-maven-3.6.3\maven-work-space\space-idea\maven-spring-Demo06\src\main\resources\applicationContext.xml]: Could not resolve placeholder 'jdbc.driver' in value "${jdbc.driver}"; nested exception is java.lang.IllegalArgumentException: Could not resolve placeholder 'jdbc.driver' in value "${jdbc.driver}"

	at org.springframework.beans.factory.config.PlaceholderConfigurerSupport.doProcessProperties(PlaceholderConfigurerSupport.java:228)
	at org.springframework.beans.factory.config.PropertyPlaceholderConfigurer.processProperties(PropertyPlaceholderConfigurer.java:211)
	at org.springframework.beans.factory.config.PropertyResourceConfigurer.postProcessBeanFactory(PropertyResourceConfigurer.java:86)
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(PostProcessorRegistrationDelegate.java:291)
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(PostProcessorRegistrationDelegate.java:167)
	at org.springframework.context.support.AbstractApplicationContext.invokeBeanFactoryPostProcessors(AbstractApplicationContext.java:707)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:533)
	at org.springframework.context.support.FileSystemXmlApplicationContext.<init>(FileSystemXmlApplicationContext.java:142)
	at org.springframework.context.support.FileSystemXmlApplicationContext.<init>(FileSystemXmlApplicationContext.java:85)
	at com.dkx.spring.dao.UserTest.test5(UserTest.java:53)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:230)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:58)
Caused by: java.lang.IllegalArgumentException: Could not resolve placeholder 'jdbc.driver' in value "${jdbc.driver}"
	at org.springframework.util.PropertyPlaceholderHelper.parseStringValue(PropertyPlaceholderHelper.java:178)
	at org.springframework.util.PropertyPlaceholderHelper.replacePlaceholders(PropertyPlaceholderHelper.java:124)
	at org.springframework.beans.factory.config.PropertyPlaceholderConfigurer$PlaceholderResolvingStringValueResolver.resolveStringValue(PropertyPlaceholderConfigurer.java:230)
	at org.springframework.beans.factory.config.BeanDefinitionVisitor.resolveStringValue(BeanDefinitionVisitor.java:296)
	at org.springframework.beans.factory.config.BeanDefinitionVisitor.resolveValue(BeanDefinitionVisitor.java:217)
	at org.springframework.beans.factory.config.BeanDefinitionVisitor.visitPropertyValues(BeanDefinitionVisitor.java:147)
	at org.springframework.beans.factory.config.BeanDefinitionVisitor.visitBeanDefinition(BeanDefinitionVisitor.java:85)
	at org.springframework.beans.factory.config.PlaceholderConfigurerSupport.doProcessProperties(PlaceholderConfigurerSupport.java:225)
	... 31 more
```

解决方案:

在applicationContext.xml配置文件中的命名空间中的location里加上`classpath*:*` 来获取其它配置文件

```xml
<!--    2.使用context空间加载properties       classpath:*:通配符加载所有以properties为后缀的文件-->
    <context:property-placeholder location="classpath:*properties" system-properties-mode="NEVER"/>
```
