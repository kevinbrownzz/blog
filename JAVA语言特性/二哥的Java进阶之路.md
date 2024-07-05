# 二哥的Java进阶之路

1.

变量可以分为局部变量、成员变量、静态变量。

当变量是局部变量的时候，必须得先初始化，否则编译器不允许你使用它。

int 作为成员变量时或者静态变量时的默认值是 0。那不同的基本数据类型，是有不同的默认值和大

小的，来个表格感受下

| **数据类型** | **默认值** | **大小** |
| ------------ | ---------- | -------- |
| boolean      | false      | 1 比特   |
| char         | '\u0000'   | 2 字节   |
| byte         | 0          | 1 字节   |
| short        | 0          | 2 字节   |
| int          | 0          | 4        |
| long         | 0L         | 8        |
| float        | 0.0f       | 4        |
| double       | 0.0        | 8        |
|              |            |          |



基本数据类型： 1、变量名指向具体的数值。 

2、基本数据类型存储在栈上。 

引用数据类型： 1、变量名指向的是存储对象的内存地址，在栈上

2、内存地址指向的对象存储在堆上



2.

new Integer(18) 每次都会新建一个对象;

 Integer.valueOf(18) 会使⽤用缓存池中的对象，多次调用只会取同⼀一个对象的引用

```java
Integer x = new Integer(18);

 Integer y = new Integer(18);

 System.out.println(x == y); 

Integer z = Integer.valueOf(18);

 Integer k = Integer.valueOf(18);

 System.out.println(z == k); 

Integer m = Integer.valueOf(300); 

Integer p = Integer.valueOf(300);

 System.out.println(m == p);
```

```
false 

true 

false
```

基本数据类型的包装类除了 Float 和 Double 之外，其他六个包装器类（Byte、Short、Integer、Long、 Character、Boolean）都有常量缓存池。 Byte：-128~127，也就是所有的 byte 值 Short：-128~127 Long：-128~127 Character：\u0000 - \u007F Boolean：true 和 false 拿 Integer 来举例子，Integer 类内部中内置了 256 个 Integer 类型的缓存数据，当使用的数据范围在 -128~127 之间时，会直接返回常量池中数据的引用，而不是创建对象，超过这个范围时会创建新的对象。 18 在 -128~127 之间，300 不在



3.

编译器之所以提示错误，是因为 = 右侧的算术表达式默认为 int 类型，左侧是 short 类型的时候需要进行强 转。

4