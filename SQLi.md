# Mybatis框架下的SQL注入漏洞
## 场景分析
### 1. 模糊查询like：
```
<select id="selectStudentByFuzzyQuery" resultMap="studentMap">
    SELECT *
    FROM student
    WHERE student.stu_name
            LIKE '%#{stuName}%'
</select>
```
MyBatis会把`%#{stuName}%`作为要查询的参数，数据库会执行
```
SELECT * FROM student WHERE student.stu_name LIKE '%#{stuName}%'
```
导致查询失败。
于是为了避免报错，研发人员将SQL查询语句改成`${}`，但是这样又存在SQL注入的风险。
### 2. 多值查询in之后的参数
```
Select * from news where id in (#{id})，
```
这样的SQL语句虽然能执行但得不到预期结果，于是研发人员将SQL查询语句修改如下：
```
Select * from news where id in (${id})，
```

### 3. order by之后
```
Select * from news where title ='京东' order by #{time} asc，
```
但由于发布时间time不是用户输入的参数，无法使用预编译。研发人员将SQL查询语句修改如下：
```
Select * from news where title =‘京东’ order by ${time} asc，
```
修改之后，程序通过预编译，但是产生了SQL语句拼接问题，极有可能引发SQL注入漏洞。

由于order by语句要求传入字段名或者字段位置：
```sql
select username from users ORDER BY id
select username from users ORDER BY 1
```
如果传入的是引号包裹的字符串，那么 ORDER BY 会失效，如：
```
SELECT * FROM user ORDER BY 'id'
```
所以，如果要动态传入 ORDER BY 参数，只能用字符串拼接的方式，如：

```
String sql = "SELECT * FROM user ORDER BY " + column;
```
那么这样依然可能会存在SQL注入的问题。

## 修复建议
### 1. 模糊查询like SQL注入修复建议
使用数据库自带的 CONCAT ，将 % 和我们用 #{} 传入参数连接起来，这样就既不存在注入的问题，也能满足需求啦。
```
<select id="selectStudentByFuzzyQuery" resultMap="studentMap">
    SELECT *
    FROM student
    WHERE student.stu_name
            LIKE CONCAT('%',#{stuName},'%')
</select>
```
采用预编译机制，避免了SQL语句拼接的问题，从根源上防止了SQL注入漏洞的产生。

### 2. in之后的参数SQL注入修复建议
可使用Mybatis自带循环指令解决SQL语句动态拼接的问题：
```
select * from news where id in

<foreach collection="ids" item="item" open="("separator="," close=")">#{item} </foreach>
```

### 3. order by(GROUP BY) SQL注入修复建议--在Java层面做映射
两种情况：
#### 1、column是字符型
手动过滤，列举出字段名，进行条件判断：
```java
String column = "id";
String sql ="";
switch(column){
    case "id":
        sql = "SELECT * FROM user ORDER BY id";
        break;
    case "username":
        sql = "SELECT * FROM user ORDER BY username";
        break;
    ......
}
```


#### column 是 int 型
因为 Java 是强类型语言，当用户传递的参数与后台定义的参数类型不匹配，程序会抛出异常，赋值失败。所以，不会存在注入的问题。

## 注
mybatis框架中`#{变量}`对应JDBC中的预编译机制(`java.sql.PreparedStatement`)，不存在SQL注入漏洞；
`${变量}`对应SQL语句拼接方式(`java.sql.Statement`)，存在SQL注入风险。



## 漏洞分类挖掘技巧
根据挖掘经验，白盒挖掘层面大致可以将SQLi的类型分为六类：

- 1、入参直接动态拼接(未使用预编译语句)；

- 2、预编译动态拼接（使用预编译语句）；

- 3、框架注入（Mybatis、Hibernate）；
Hibernate:
Mybatis: 挖掘技巧则是在注解中或者Mybatis相关的配置文件中搜索 `$`
- 4、order by、like、in等语句不能预编译导致的注入；

- 5、%和_绕过预编译；

- 6、SQLi检测绕过


## 参考
- [给自己一个更安全的 mysql](https://klionsec.github.io/2017/11/22/mysqlconfigsec/)
- https://jayl1n.github.io/2018/11/15/java-audit-step-by-step-3/
- [java框架之MybatisSQL注入漏洞](https://zhuanlan.zhihu.com/p/28168319)
- [Java代码审计汇总系列(一)——SQL注入](https://cloud.tencent.com/developer/article/1534109)
- [本以为用的MyBatis框架就万无一失了，没想到还是被黑客注入了，我真的无语了！](https://mp.weixin.qq.com/s/yr5kp91m6dWrFDZT28Vohw)
