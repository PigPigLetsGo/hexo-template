---
title: MyBatisPlus配置类
categories:
   - [计算机学科,java,config配置类]
tags:
   - 计算机学科
   - java
   - config配置类
---

```java
@Configuration
@MapperScan("com.dkx.mapper")
public class MybatisPlusConfig {
	/**
	 * 新的分页插件
	 * 需要设置 MybatisConfiguration#useDeprecatedExecutor = false
	 * 避免缓存出现问题(该属性会在旧插件移除后一同移除)
	 */
	@Bean
	public MybatisPlusInterceptor mybatisPlusInterceptor() {
		MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
		interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
		return interceptor;
	}
}
```