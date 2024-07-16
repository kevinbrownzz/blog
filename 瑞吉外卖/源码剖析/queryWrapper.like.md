# queryWrapper.like



下面是对 `queryWrapper.like(!StringUtils.isEmpty(name), Employee::getName, name)` 这行代码背后源码的总结。

### 1. `like` 方法调用

首先，调用 `like` 方法：

```
java
复制代码
queryWrapper.like(!StringUtils.isEmpty(name), Employee::getName, name);
```

这个方法接收三个参数：

- `condition`：一个布尔值，表示是否添加该条件。
- `column`：表示要匹配的列。
- `val`：表示要匹配的值。

### 2. `like` 方法实现

```
java复制代码public Children like(boolean condition, R column, Object val) {
    return this.likeValue(condition, SqlKeyword.LIKE, column, val, SqlLike.DEFAULT);
}
```

该方法内部调用 `likeValue` 方法，将 `LIKE` 关键字和默认的 `SqlLike` 样式传递给它。

### 3. `likeValue` 方法实现

```
java复制代码protected Children likeValue(boolean condition, SqlKeyword keyword, R column, Object val, SqlLike sqlLike) {
    return this.doIt(condition, () -> {
        return this.columnToString(column);
    }, keyword, () -> {
        return this.formatSql("{0}", SqlUtils.concatLike(val, sqlLike));
    });
}
```

该方法根据 `condition` 参数，决定是否调用 `doIt` 方法。它传递了以下参数给 `doIt`：

- 列名转换为字符串。
- `LIKE` 关键字。
- 格式化后的 `LIKE` 值字符串。

### 4. `doIt` 方法实现

```
java复制代码protected Children doIt(boolean condition, ISqlSegment... sqlSegments) {
    if (condition) {
        this.expression.add(sqlSegments);
    }
    return this.typedThis;
}
```

`doIt` 方法将 `sqlSegments` 添加到 `expression` 列表中，前提是 `condition` 为 `true`。

### 5. `expression` 列表中的 SQL 片段

`expression` 列表存储所有的 SQL 片段。这些片段表示查询的不同部分，如列名、关键字和值。

### 6. 添加 SQL 片段到 `expression` 列表

通过 `doIt` 方法，传递的 SQL 片段被添加到 `expression` 列表中：

```
java复制代码public void add(ISqlSegment... iSqlSegments) {
    List<ISqlSegment> list = Arrays.asList(iSqlSegments);
    ISqlSegment firstSqlSegment = list.get(0);
    if (MatchSegment.ORDER_BY.match(firstSqlSegment)) {
        this.orderBy.addAll(list);
    } else if (MatchSegment.GROUP_BY.match(firstSqlSegment)) {
        this.groupBy.addAll(list);
    } else if (MatchSegment.HAVING.match(firstSqlSegment)) {
        this.having.addAll(list);
    } else {
        this.normal.addAll(list);
    }
    this.cacheSqlSegment = false;
}
```

### 7. 构建最终的 SQL 查询

在查询执行前，所有的 SQL 片段会被组合起来，形成最终的 SQL 查询字符串。