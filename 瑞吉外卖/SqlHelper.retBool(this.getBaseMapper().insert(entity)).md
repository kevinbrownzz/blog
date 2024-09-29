

Controller层中代码

```java
@PostMapping
public R<String> save(HttpServletRequest request,@RequestBody Employee employee){
    log.info("新增员工,员工信息:{}",employee.toString());


    employee.setPassword(DigestUtils.md5DigestAsHex("123456".getBytes()));
    employee.setCreateTime(LocalDateTime.now());
    employee.setUpdateTime(LocalDateTime.now());

    Long empId = (Long) request.getSession().getAttribute("employee");

    employee.setCreateUser(empId);
    employee.setUpdateUser(empId);
    employeeService.save(employee);


    return R.success("新增员工成功");
}
```

employeeService.save(employee);深究这一行背后的源码逻辑

```java
public interface IService<T> {
    int DEFAULT_BATCH_SIZE = 1000;

    default boolean save(T entity) {
        return SqlHelper.retBool(this.getBaseMapper().insert(entity));
    }
```

**`SqlHelper.retBool` 方法**: 这个方法通常是用来将数据库操作的返回结果转换为布尔值的工具方法

this.getBaseMapper()获取service对应的mapper类。insert(entity)执行插入操作（对插入的表名和字段名存疑）

在 MyBatis 和 MyBatis-Plus 中，表名和字段名的确定有多种方式，不一定要求类名与表名、类中字段名与表中字段名完全一致。通常，通过配置文件、注解或其他机制来指定这些映射关系。以下是几种常见的方法：

### 1. 使用注解

你可以在实体类上使用注解来指定表名和字段名。

#### 指定表名

使用 `@TableName` 注解来指定表名：

```java
import com.baomidou.mybatisplus.annotation.TableName;

@TableName("employee")
public class Employee {
    // fields and methods
}
```

#### 指定字段名

使用 `@TableField` 注解来指定字段名：

```java
import com.baomidou.mybatisplus.annotation.TableField;

public class Employee {
    
    @TableField("name")
    private String name;

    @TableField("password")
    private String password;

    // other fields and methods
}
```

### 2. 使用 XML 映射文件

你可以在 XML 映射文件中显式地指定表名和字段名。

#### 表名映射

在 SQL 语句中直接使用表名：

```java
<mapper namespace="com.example.mapper.EmployeeMapper">

    <insert id="insert" parameterType="com.example.entity.Employee">
        INSERT INTO employee (name, password, create_time, update_time, create_user, update_user)
        VALUES (#{name}, #{password}, #{createTime}, #{updateTime}, #{createUser}, #{updateUser})
    </insert>

</mapper>
```



application.yml

```java
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: ASSIGN_ID
```

在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射

 map-underscore-to-camel-case: true