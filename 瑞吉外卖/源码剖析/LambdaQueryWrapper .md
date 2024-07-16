## 一.`LambdaQueryWrapper` 

```java
LambdaQueryWrapper<Employee> queryWrapper =  new LambdaQueryWrapper<>();
queryWrapper.eq(Employee::getUsername,employee.getUsername());
Employee emp = employeeService.getOne(queryWrapper);
```

### 1. 创建 `LambdaQueryWrapper` 对象

```java
LambdaQueryWrapper<Employee> queryWrapper =  new LambdaQueryWrapper<>();
```

**`LambdaQueryWrapper` 是 MyBatis-Plus 提供的一个用于构建查询条件的辅助类。它主要用于 lambda 表达式，以保证字段名在重构时的安全性。**

#### 源码分析

`LambdaQueryWrapper` 继承自 `AbstractLambdaWrapper`，其构造函数如下：

```java
public class LambdaQueryWrapper<T> extends AbstractLambdaWrapper<T, LambdaQueryWrapper<T>> {
    public LambdaQueryWrapper() {
        this(null);
    }

    public LambdaQueryWrapper(T entity) {
        super.setEntity(entity);
        super.initNeed();
    }
}
```

这里我们调用的是无参构造函数，内部调用了父类的 `setEntity(null)` 和 `initNeed()` 方法。

### 2. 添加查询条件

```java
queryWrapper.eq(Employee::getUsername, employee.getUsername());
```

这行代码向 `queryWrapper` 中添加了一个等值查询条件，即 `username = ?`。

#### 源码分析

`eq` 方法定义在 `AbstractLambdaWrapper` 类中：

```java
public Children eq(SFunction<T, ?> column, Object val) {
    return addCondition(val, column, SqlKeyword.EQ, val);
}
```

`SFunction` 是一个函数式接口，用于接受 lambda 表达式。`eq` 方法内部调用 `addCondition` 方法来添加查询条件。

`addCondition` 方法如下：

```java
protected Children addCondition(boolean condition, SFunction<T, ?> column, SqlKeyword sqlKeyword, Object val) {
    if (condition) {
        String columnName = getColumn(column);
        this.addCondition(new SqlSegment(columnName, sqlKeyword, val));
    }
    return typedThis;
}
```

- `getColumn(column)` 方法将 lambda 表达式 `Employee::getUsername` 转换为实际的数据库列名。
- `addCondition(new SqlSegment(columnName, sqlKeyword, val))` 方法将条件添加到内部的查询条件列表中。

### 3. 执行查询

```java
Employee emp = employeeService.getOne(queryWrapper);
```

这行代码使用 `employeeService` 执行查询，并返回符合条件的单个 `Employee` 对象。

#### 源码分析

`getOne` 方法通常定义在 MyBatis-Plus 的 `IService` 接口中，其实现如下：

```java
public T getOne(Wrapper<T> queryWrapper) {
    return this.baseMapper.selectOne(queryWrapper);
}
```

这里的 `baseMapper` 是 `BaseMapper` 的实例，`selectOne` 方法负责执行查询并返回结果。

### `selectOne` 方法实现

`selectOne` 方法在 `BaseMapper` 接口中定义，其实现如下：

```java
@Override
public T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
    List<T> list = this.selectList(queryWrapper);
    if (CollectionUtils.isEmpty(list)) {
        return null;
    } else if (list.size() > 1) {
        throw new MyBatisPlusException("Expected one result (or null) to be returned by selectOne(), but found: " + list.size());
    } else {
        return list.get(0);
    }
}
```

- `selectList` 方法执行查询并返回一个列表。
- 如果列表为空，返回 `null`。
- 如果列表中有多个元素，抛出异常。
- 如果列表中只有一个元素，返回该元素。

### 综上所述

```java
LambdaQueryWrapper<Employee> queryWrapper =  new LambdaQueryWrapper<>();
queryWrapper.eq(Employee::getUsername, employee.getUsername());
Employee emp = employeeService.getOne(queryWrapper);
```

这段代码的执行流程如下：

1. 创建一个 `LambdaQueryWrapper` 对象，用于构建查询条件。
2. 使用 lambda 表达式添加一个等值查询条件 `username = ?`。
3. 调用 `employeeService.getOne(queryWrapper)` 执行查询，返回符合条件的单个 `Employee` 对象。

通过这种方式，MyBatis-Plus 提供了一个简洁且类型安全的查询条件构建方式，有效减少了手动拼接 SQL 语句的繁琐和可能的错误。