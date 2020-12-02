## Spring集成Junit步骤

1.导入junit包

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.1</version>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>3.8.2</version>
    <scope>test</scope>
</dependency>
```

2.使用@Runwith注解替换原来的运行期

```java
@RunWith(SpringJUnit4ClassRunner.class)
```

3.使用@ContextConfiguration指定配置文件或配置类

```java
/*@ContextConfiguration("classpath:applicationContext.xml")*/
@ContextConfiguration(classes = {SpringConfiguration.class})
```

4.使用@Autowired注入需要测试的类

```
@Autowired
private UserService userService;
@Autowired
private DataSource dataSource;
```

5.创建测试方法

```
@Test
public void test1(){
    System.out.println(userService);
}
```