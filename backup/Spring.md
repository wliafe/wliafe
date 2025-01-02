# 基本概念 

## IoC(Inversion of Control)控制反转

定义：使用对象时，由主动new产生对象转换为由外部提供对象，此过程中对象创建控制权由程序转移到外部，此思想称为控制反转

## IoC容器
 
定义：spring中使用IoC容器实现IoC

IoC容器负责对象的创建、初始化等一系列工作。

## Bean

被创建或被管理的对象在IoC容器中统称为Bean

## DI(Dependency Injection)依赖注入

在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

# 代码样例

## 业务层代码

BookService

```java
public interface BookService {
	public void save();
}
```

BookServiceImpl

```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();

    public void save() {
        System.out.println("book service save...");
        bookDao.save();
    }
}
```

## 数据层实现

BookDao

```java
public interface BookDao {
    void save();
}
```

BookDaoImpl

```java
public class BookDaoImpl implements BookDao {
	public void save(){
		System.out.println("book dao save ...")
	}
}
```

## Application

```java
public class Application {
    public static void main(String[] args) {
        BookService bookService = new BookServiceImpl();
        bookService.save();
    }
}
```

## pom.xml

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>5.2.10.RELEASE</version>
</dependency>
```

## applicationContext.xml

```xml
<?xml version="1.0"encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
						https://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

# IoC使用案例

定义类和接口

BookService

```java
public interface BookService {
	public void save();
}
```

BookServiceImpl

```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();

    public void save() {
        System.out.println("book service save...");
        bookDao.save();
    }
}
```

创建配置文件，配置bean

```xml
<bean id="bookService"class="com.itheima.service.impl.BookServiceImpl"/>
```

初始化容器

```java
public class Application {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```

## DI使用案例

删除new

提供setter方法

BookServiceImpl

```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save...");
        bookDao.save();
    }
}
```

配置service与dao绑定关系

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
    <property name="bookDao" ref="bookDao"/>
</bean>
```

# Bean属性

id：标签

name：别名

class：bean类型

scope：是否是单例

init-method：bean初始化对应的操作

destory-method：bean销毁前对应的操作

autowire：自动装配

# Bean实例化

spring创造bean调用无参构造方法。

## 使用静态工厂实例化bean

添加工厂

```java
public class BookDaoFactory {
    public static BookDao getBookDao() {
        return new BookDaoImpl();
    }
}
```

设置bean

```xml
<bean id="bookDao" class="com.itheima.factory.BookDaoFactory" factory-method="getBookDao"/>
```

## 实例工厂实例化bean

添加工厂

```java
public class BookDaoFactory {
    public  BookDao getBookDao() {
        return new BookDaoImpl();
    }
}
```

设置bean

```xml
<bean id="bookDaoFactory" class="com.itheima.factory.BookDaoFactory"/>
<bean id="bookDao" factory-method="getBookDao" factory-bean="bookDaoFactory"/>
```

## 使用FactoryBean实例化bean

添加工厂

```java
public class BookDaoFactoryBean implements FactoryBean<BookDao> {
    @Override
    public BookDao getObject() throws Exception {
        return new BookDaoImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return BookDao.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

设置bean

```xml
<bean id="bookDao" class="com.itheima.factory.BookDaoFactoryBean"/>
```

# 通过接口控制bean生命周期

添加接口，代替init-method,destory-method

```java
public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save...");
        bookDao.save();
    }

    @Override
    public void destroy() throws Exception {
        
    }

    @Override
    public void afterPropertiesSet() throws Exception {

    }
}
```

# 依赖注入

## setter注入

上文提到过引用类型注入

简单类型注入的bean

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
	<property name="databaseName" value="mysql"/>
</bean>
```

## 构造器注入

提供构造方法

注入bean

```xml
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
	<constructor-arg name="bookDao" ref="bookDao"/>
</bean>
```

还有按类注入，按位置注入

## 集合注入

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
	<property name="array">
		<array>
			<value>100</value>
			<value>200</value>
			<value>300</value>
		</array>
	</property>
	<property name="list">
		<list>
			<value>itcast</value>
			<value>itheima</value>
			<value>boxuegu</value>
		</list>
	</property>
	<property name="set">
		<set>
			<value>itcast</value>
			<value>itheima</value>
			<value>boxuegu</value>
			<value>boxuegu</value>
		</set>
	</property>
	<property name="map">
		<map>
			<entry key=""value=""/>
			<entry key=""value=""/>
			<entry key=""value=""/>
		</map>
	</property>
</bean>
```

# Bean纯注解开发

定义配置类

```java
@Configuration
@ComponentScan("com.itheima.dao")
public class SpringConfig {
}
```

定义类(注解，四个任选其一)

```java
@Component("bookDao")
//@Controller:用于表现层bean定义
//@Service:用于业务层bean定义
//@Repository:用于数据层bean定义
@Scope("prototype")//单例或非单例
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
```

BookServiceImpl

```java
@Service("bookService")
public class BookServiceImpl implements BookService {
    @Autowired
    @Qualifier("bookDao")
    private BookDao bookDao;
    
    @Value("itheima")
    private String name;

    public void save() {
        System.out.println("book service save...");
        bookDao.save();
    }

    @PreDestroy
    public void destroy() {
        System.out.println("BookService destroy");
    }

    @PostConstruct
    public void afterPropertiesSet() {
        System.out.println("BookService init");
    }
}
```

使用类

```java
public class Application {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean("bookDao", BookDao.class);
        bookDao.save();
    }
}
```

## 加载外部properties文件

定义配置类

```java
@Configuration
@ComponentScan("com.itheima")
@PropertySource("jdbc.properties")
public class SpringConfig {
}
```

定义类

```java
@Service("bookService")
public class BookServiceImpl implements BookService {
    @Autowired
    @Qualifier("bookDao")
    private BookDao bookDao;

    @Value("${name}")
    private String name;

    public void save() {
        System.out.println("book service save..." + name);
        bookDao.save();
    }
}
```