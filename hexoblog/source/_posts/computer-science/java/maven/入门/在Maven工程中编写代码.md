---
title: 在maven工程中编写代码
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

## 主体程序

![image-20240208131659277](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208131659277.png)

主体程序指的是被测试的程序,同时也是将来在项目中真正要使用的程序

```java
package com.dkx.maven
public class Calculator{
    public int sum(int i,int j){
        return i + j;
    }
}
```

## 测试程序

![image-20240208131713758](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208131713758.png)

```java
package com.dkx.maven;
import org.junit.Test;
import static org.junit.Assert.*;
public class CalculatorTest{
	@Test
	public void testSum() {
		Calculator c = new Calculator();
		int s = c.sum(5,3);
		System.out.println(s);
		int a = 8;
		assertEquals(a,s);
	}
}
```

