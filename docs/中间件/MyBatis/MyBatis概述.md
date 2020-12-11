# MyBatis原理
**MyBatis完成2件事情**

1. 封装JDBC操作
2. 利用反射打通Java类与SQL语句之间的相互转换

**MyBatis的主要成员**

1. Configuration        MyBatis所有的配置信息都保存在Configuration对象之中，配置文件中的大部分配置都会存储到该类中
2. SqlSession           作为MyBatis工作的主要顶层API，表示和数据库交互时的会话，完成必要数据库增删改查功能
3. Executor             MyBatis执行器，是MyBatis 调度的核心，负责SQL语句的生成和查询缓存的维护
4. StatementHandler     封装了JDBC Statement操作，负责对JDBC statement 的操作，如设置参数等
5. ParameterHandler     负责对用户传递的参数转换成JDBC Statement 所对应的数据类型
6. ResultSetHandler     负责将JDBC返回的ResultSet结果集对象转换成List类型的集合
7. TypeHandler          负责java数据类型和jdbc数据类型(也可以说是数据表列类型)之间的映射和转换
8. MappedStatement      MappedStatement维护一条select|update|delete|insert节点的封装
9. SqlSource            负责根据用户传递的parameterObject，动态地生成SQL语句，将信息封装到BoundSql对象中，并返回
10. BoundSql            表示动态生成的SQL语句以及相应的参数信息


![](https://github.com/zaiyunduan123/Java-Interview/blob/master/image/frame-2.jpg)


# MyBatis缓存

MyBatis提供查询缓存，用于减轻数据库压力，提高性能。MyBatis提供了一级缓存和二级缓存。
![](https://github.com/zaiyunduan123/Java-Interview/blob/master/image/frame-3.jpg)

1. 一级缓存是SqlSession级别的缓存，每个SqlSession对象都有一个哈希表用于缓存数据，不同SqlSession对象之间缓存不共享。同一个SqlSession对象对象执行2遍相同的SQL查询，在第一次查询执行完毕后将结果缓存起来，这样第二遍查询就不用向数据库查询了，直接返回缓存结果即可。一级缓存是MyBatis内部实现的一个特性，用户不能配置，默认情况下自动支持的缓存，用户没有定制它的权利

2.  二级缓存是Application应用级别的缓存，它的是生命周期很长，跟Application的声明周期一样，也就是说它的作用范围是整个Application应用。MyBatis默认是不开启二级缓存的，可以在配置文件中使用如下配置来开启二级缓存
```xml
<settings>
    <setting name="cacheEnabled" value="true"/>
</settings>
```