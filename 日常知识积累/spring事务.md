@Transactional 注解

失效场景：

1. 注解修饰的方法必须是public

2. 注解修饰的方法不能是final/static的

3. 在同一个类中的方法直接内部调用

4. 未被spring管理

5. 如果调用的事务方法是在另一个线程中执行的，会失效。

   spring事务是通过数据库连接实现的，当前线程中保存了个map，key是数据源，value是数据库连接

   ```java
   private static final ThreadLocal<Map<Object, Object>> resources =new NamedThreadLocal<>("Transactional resources");
   ```

   我们说的同一个事务，是指同一个数据库连接，只有拥有用一个数据库连接才能同时提交和回滚，如果在不同的线程，拿到的数据库连接不一样，是不同的事务。

6. 数据库引擎不支持事务，如myisam

7. 未开始事务，如spring未配置事务管理类

事务不回滚场景：

1. 传播特性错误

   在使用@Transactional注解时，可以指定propagation参数，作用是指定事务的传播特性，spring目前支持7种传播特性：
   
   REQUIRED：如果当前上下文中存在事务，则加入该事务，如果不存在事务，则创建事务，是默认值
   
   SUPPORTS：如果当前上下文中存在事务，则加入该事务，如果不存在，使用非事务方式执行
   
   MANDATORY：当前上下文中必须存在事务，否则抛出异常
   
   REQUIRES_NEW：每次都会新建一个事务，同时将上下文中的事务挂起，执行当前新建事务完成后，上下文事务恢复执行
   
   NOT_SUPPORTED：如果当前上下文中存在事务，则挂起当前事务，然后新方法在没有事务的环境中执行
   
   NEVER：如果当前上下文中存在事务，则抛出异常，否则在无事务环境上执行
   
   NESTED：如果当前上下文中存在事务，则嵌套事务执行，如果不存在，则新建事务
   
2. 自己吞了异常

   开发者自己捕获了异常，又没有手动抛出。如果想要spring事务能正常回滚，必须抛出他能处理的异常，如果没有抛出异常，spring认为程序正常

3. 手动抛出了别的异常

   spring事务，默认情况下只会回滚RuntimeException（运行时异常）和Error（错误），对于普通的Exception（非运行时异常）不会回滚

4. 自定义回滚异常

   rollbackFor设置自定义回滚异常

5. 嵌套事务回滚多了

其他

1. 基于@Transactional注解，声明式事务

2. 手动编写代码实现的事务，编程式事务

   代码实例

   ```java
   @Autowired
      private TransactionTemplate transactionTemplate;
      
      ...
      
      public void save(final User user) {
            queryData1();
            queryData2();
            transactionTemplate.execute((status) => {
               addData1();
               updateData2();
               return Boolean.TRUE;
            })
      }
   ```

   spring中支持编程式事务，提供了TransactionTemplate类，在他的execute中实现了事务功能。

