# 简介

MyBatis-Plas是Java后端开发对数据库进行操作的工具，这里是[MyBatis-Plas的官方文档](https://baomidou.com/)，同时这里还有[哔哩哔哩的视频](https://www.bilibili.com/video/BV1Bq4y1f7YD/)可以学习。

主要内容可以根据这两个网址进行学习，以下是对MyBatis-Plus的补充内容。

# 事务管理

## 事务简介

事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你既需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务！

开启事务，在Springboot的启动类，或者某个@Configuration的类上加上@EnableTransactionManagement开启事务。因为这是数据库相关，所以我加在了mybatis-plus的配置类上

```java
/**
 * mybatisplus配置类
 */
//扫描mapper文件夹
@MapperScan("com.sec.mapper")
@EnableTransactionManagement//事务
@Configuration//配置类
public class MybatisPlusConf {


    //配置乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }

    //配置分页插件
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        paginationInterceptor.setOverflow(false);
        return paginationInterceptor;
    }
}
```

然后只要在需要使用事务的方法上加上@Transactional就可以开启事务了，还是很简单的

注意点

@Transactional默认回滚的是RuntimeException也就是说如果抛出的不是RuntimeException的异常，数据库是不会回滚的。但是所幸的是，在spring框架下，所有的异常都被org.springframework重写为RuntimeException，因此不需要太担心

还有如果在异常发生时，程序员自己手动捕获处理了，异常也不会回滚

```java
@Transactional
	public void buy() throws Exception {
        try{
        // 扣钱
        } catch (Exception e) {
        	// catch了自己处理，也就是异常被自己吞了，外层并不知道，此时也不会回滚
        }
        // 扣库存
    }
```

当我们需要在事务控制的service层类中使用try catch 去捕获异常后，就会使事务控制失效，因为该类的异常并没有抛出，就不是触发事务管理机制。怎样才能即使用try catch去捕获异常，而又让出现异常后spring回滚呢，这里就要用到TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();

```java
//假设这是一个service类的片段

try{
    //出现异常
} catch (Exception e) {
            e.printStackTrace();
           //设置手动回滚
            TransactionAspectSupport.currentTransactionStatus()
                    .setRollbackOnly();
        }
//此时return语句能够执行
return  xxx;
```