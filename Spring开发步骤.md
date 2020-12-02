### Spring开发步骤

1.导入Spring开发的基本包

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.1</version>
    </dependency>
</dependencies>
```

2.编写Dao包和接口

```java
public interface UserDao {
    public void say();
}
```

```java
public class UserDaoImpl implements UserDao{
    public void say(){
        System.out.println("userDaoImpl");
    }
}
```

3.创建Spring核心配置文件（即一个Spring容器）

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--bean实例化-->
    <bean id="UserDao" class="com.FallSoberly.dao.Impl.UserDaoImpl">
        <property name="username" value="fall_soberly"></property>
    </bean>
    <bean id="UserService" class="com.FallSoberly.service.Impl.UserServiceImpl">

        <!--set方法注入，将UserDao注入UserService-->
        <!--<property name="UserDao" ref="UserDao"></property>-->

        <!--构造器注入-->
        <constructor-arg name="userDao" ref="UserDao"></constructor-arg>
    </bean>
</beans>
```

4.在Spring配置文件中配置UserDaoImpl

```xml
<bean id="UserDao" class="com.fallSoberly.UserDaoImpl"></bean>
```

5.使用Spring的API获得bean实例

```java
public class demo {
    public static void main(String[] args){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao=(UserDao) applicationContext.getBean("UserDao");
        userDao.say();
    }
}
```