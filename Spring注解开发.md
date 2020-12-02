## Spring注解开发

1.在applicationContext.xml文件添加组件扫描

```xml
<context:component-scan base-package="com.FallSoberly"/>
```

2.通过@Component将类注入Spring容器

```java
/*<bean id="userDao" class="com.FallSoberly.dao.Impl.UserDaoImpl"></bean>*/
@Component("userDao")
public class UserDaoImpl implements UserDao {
    public void say(){
        System.out.println("Im UserDaoImpl...");
    }
}
```

3.通过@Autowired和@Qualifier将子类注入父类

```java
@Autowired
@Qualifier("userDao")
private UserDao userDao;
public void setUserDao(UserDao userDao){
    this.userDao=userDao;
}
```

