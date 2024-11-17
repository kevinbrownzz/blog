# com.imooc.pan.web.exception

## WebExceptionHnadler.java

在 Java 编程中，异常处理是一个关键的概念，用于捕获和处理程序执行过程中的错误。Spring 框架提供了强大的异常处理机制，使开发者能够更方便地管理应用程序中的各种异常。代码使用了 Spring 的 `@RestControllerAdvice` 注解来定义一个全局异常处理类 `WebExceptionHandler`，这个类专门处理应用程序中发生的特定异常，并返回统一格式的响应对象 `R`。

### 代码详解

#### 1. `WebExceptionHandler` 类

这个类使用了 `@RestControllerAdvice` 注解，表明它是一个专门处理控制器（Controller）中异常的类。它包含多个异常处理方法，每个方法都使用 `@ExceptionHandler` 注解来指明它处理的特定异常类型。

##### 主要异常处理方法

- **`rPanBusinessHandler(RPanBusinessException e)`**: 处理自定义业务异常 `RPanBusinessException`，返回一个包含错误码和错误信息的响应对象。
- **`methodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e)`**: 处理方法参数校验失败的异常，返回第一个错误信息。
- **`constraintViolationException(ConstraintViolationException e)`**: 处理约束违规异常，返回第一个约束违规的错误信息。
- **`missingServletRequestParameterException(MissingServletRequestParameterException e)`**: 处理缺少请求参数的异常，返回统一的错误响应。
- **`IllegalStateException(IllegalStateException e)`**: 处理非法状态异常，返回统一的错误响应。
- **`bindExceptionHandler(BindException e)`**: 处理绑定异常，返回第一个字段错误的信息。
- **`runtimeExceptionHandler(RuntimeException e)`**: 处理运行时异常，返回错误信息。

```java
@ExceptionHandler(value = RPanBusinessException.class)
public R rPanBusinessHandler(RPanBusinessException e){
    return R.fail(e.getCode(), e.getMessage());
}

@ExceptionHandler(value = MethodArgumentNotValidException.class)
public R methodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e){
    ObjectError objectError = e.getBindingResult().getAllErrors().stream().findFirst().get();
    return R.fail(ResponseCode.ERROR_PARAM.getCode(),objectError.getDefaultMessage());
}
```