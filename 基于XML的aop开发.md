## 基于XML的aop开发

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
package com.FallSoberly.aop;

public interface TargetInterface {
    public void save();
}
```

```
package com.FallSoberly.aop;

public class Target implements TargetInterface {
    public void save() {
        System.out.println("running......");
    }
}
```

3.创建切面类（增强方法）

```
package com.FallSoberly.aop;

public class MyAspect {
    public void before(){
        System.out.println("前置增强......");
    }
}
```

4.将目标类和切面类的对象创建权交给spring

```
<!--目标对象-->
<bean id="target" class="com.FallSoberly.aop.Target"></bean>
<!--切面对象-->
<bean id="myAspect" class="com.FallSoberly.aop.MyAspect"></bean>
```

5.在applicationContext.xml中配置织入关系

```
<!--配置织入，告诉Spring框架哪些方法（切点）需要进行哪些增强（前置、后置...）-->
<aop:config>
    <!--声明切面-->
    <aop:aspect ref="myAspect">
        <!--切点+通知-->
        <aop:before method="before" pointcut="execution(public void com.FallSoberly.aop.Target.save())"></aop:before>
    </aop:aspect>
</aop:config>
```

6.编写测试

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest {
    @Autowired
    private TargetInterface target;

    @Test
    public void test1(){
        target.save();
    }
}
```