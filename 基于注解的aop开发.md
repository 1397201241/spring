## 基于注解的aop开发

1.导入aop相关的包

```
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.4</version>
</dependency>
```

2.创建目标接口和目标类（有切点）

```
package com.FallSoberly.anno;

public interface TargetInterface {
    public void save();
}
```

```
package com.FallSoberly.anno;

import org.springframework.stereotype.Component;

@Component("target")
public class Target implements TargetInterface {
    public void save() {
        System.out.println("running......");
    }
}
```

3.创建切面类（增强方法）

```
package com.FallSoberly.anno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component("myAspect")
@Aspect  //标志切面
public class MyAspect {
    @Before("execution(public void com.FallSoberly.anno.Target.save())")
    public void before(){
        System.out.println("前置增强......");
    }
    @AfterReturning("execution(public void com.FallSoberly.anno.Target.save())")
    public void afterReturning(){
        //do something
        System.out.println("后置增强.......");
    }
    @Around("execution(public void com.FallSoberly.anno.Target.save())")
    public void around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕前增强.......");
        Object proceed= pjp.proceed();
        System.out.println("环绕后增强......");
    }
    @AfterThrowing("execution(public void com.FallSoberly.anno.Target.save())")
    public void afterThrowing(){
        System.out.println("抛出增强......");
    }
    @After("execution(public void com.FallSoberly.anno.Target.save())")
    public void After(){
        System.out.println("最终增强......");
    }
}

```

4.将目标类和切面类的对象创建权交给spring

```
<!--目标对象-->
@Component("target")
public class Target implements TargetInterface {}
<!--切面对象-->
@Component("myAspect")
@Aspect  //标志切面
public class MyAspect {}
```

5.在applicationContext-anno.xml中配置织入关系

```
<context:component-scan base-package="com.FallSoberly.anno"/>
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

6.编写测试

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext-anno.xml")
public class AopTest {
    @Autowired
    private TargetInterface target;

    @Test
    public void test1(){
        target.save();
    }
}
```