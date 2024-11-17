# com.imooc.pan.web.validator

## WebValidatorConfig.java

### 关于 `javax.validation` 和 Hibernate Validator

#### **什么是 Bean Validation？**

`javax.validation` 是 Java 中的 Bean Validation API，它为 Java 对象提供了统一的校验机制。通过使用校验注解，你可以确保 Java 对象的属性或方法参数符合某些特定的条件。

#### **常用注解**

- **`@NotNull`**：确保某个值不为 `null`。
- **`@Size(min=, max=)`**：确保集合或字符串的大小在指定的范围内。
- **`@Min(value)`** 和 **`@Max(value)`**：确保数值在指定的最小值和最大值之间。
- **`@Email`**：确保某个字符串是合法的电子邮件地址。
- **`@Pattern(regexp)`**：确保某个字符串符合指定的正则表达式。

#### **如何使用 Bean Validation？**

你可以在对象的字段或方法参数上添加这些注解。当你使用这些注解时，Spring 会在方法执行前进行校验，如果校验不通过，它会抛出异常并阻止方法的执行。

##### 例如：

```java
public void createUser(@NotNull @Size(min = 2, max = 20) String username) {
    // 这个方法会验证 username 不为空，并且长度在 2 到 20 之间
}
```

#### **Hibernate Validator**

Hibernate Validator 是 Java Bean Validation 的一个实现。它除了支持标准的 `javax.validation` 注解外，还提供了一些额外的特性，比如上面提到的 `FAIL_FAST` 模式。

### `FAIL_FAST` 模式

`FAIL_FAST` 是指在校验过程中，如果第一个约束失败，校验就立即停止，而不是继续校验剩余的字段或规则。这可以提高校验效率，尤其是当校验规则比较多时。

在这个类中，`FAIL_FAST` 是通过 `addProperty(FAIL_FAST_KEY, RPanConstants.TRUE_STR)` 来开启的，`FAIL_FAST_KEY` 的值为 `"hibernate.validator.fail_fast"`，而 `RPanConstants.TRUE_STR` 则是 `"true"`。

### 1. `WebValidatorConfig` 类

`WebValidatorConfig` 是一个 Spring Boot 配置类，使用了 `@SpringBootConfiguration` 注解，表示这是一个 Spring 配置类。这个类的主要功能是配置 Hibernate Validator，并启用方法级别的校验功能。

`@Log4j2` 注解用于启用日志记录功能，使用 `Log4j2` 作为日志记录框架。

### 2. `methodValidationPostProcessor()` 方法

```java
@Bean
public MethodValidationPostProcessor methodValidationPostProcessor() {
    MethodValidationPostProcessor postProcessor = new MethodValidationPostProcessor();
    postProcessor.setValidator(rpanValidator());
    log.info("The hibernet validator is loaded successfully");
    return postProcessor;
}
```

#### 解释：

- **`@Bean`** 注解：这个方法定义了一个 Bean，该 Bean 将被 Spring 管理并注册到应用的上下文中。这个 Bean 的作用是配置方法级别的参数校验器。
- **`MethodValidationPostProcessor`**：
  - Spring 提供的类，用于启用方法级别的参数校验。通过它，可以对控制器或服务层方法的参数进行校验。
  - 这个类将会拦截带有校验注解的方法参数，并使用配置的 `Validator` 进行参数的验证。
- **`setValidator(rpanValidator())`**：通过调用 `setValidator()`，我们把自定义的 `rpanValidator()` 设置为校验器，替代 Spring 默认的校验器。
- **日志记录**：`log.info("The hibernet validator is loaded successfully");` 在日志中输出 Hibernate Validator 加载成功的消息。

#### 用途：

这个方法将方法级别的校验功能集成到 Spring 中，这样我们可以在方法参数上使用校验注解（例如 `@NotNull`、`@Size` 等），来自动验证传递的参数是否合法。

### 3. `rpanValidator()` 方法

```java
private Validator rpanValidator() {
    ValidatorFactory validatorFactory = Validation.byProvider(HibernateValidator.class)
            .configure()
            .addProperty(FAIL_FAST_KEY, RPanConstants.TRUE_STR)
            .buildValidatorFactory();
    Validator validator = validatorFactory.getValidator();
    return validator;
}
```

#### 解释：

- **`ValidatorFactory`**：这是一个工厂类，用于创建 `Validator` 实例。它是 `javax.validation` 中的核心类，负责提供校验器。
- **`Validation.byProvider(HibernateValidator.class)`**：这里指定使用 Hibernate Validator 作为实现的校验器。
- **`configure()`**：配置校验器，允许通过各种设置定制校验器的行为。
- **`addProperty(FAIL_FAST_KEY, RPanConstants.TRUE_STR)`**：设置 `FAIL_FAST` 模式。
  - `FAIL_FAST` 是 Hibernate Validator 提供的一种模式，表示在校验时，如果遇到第一个校验失败的规则，立刻停止校验，返回错误。
  - `RPanConstants.TRUE_STR` 是一个常量，值为 `"true"`，表示开启 `FAIL_FAST` 模式。
- **`buildValidatorFactory()`**：根据配置生成一个 `ValidatorFactory`，用于创建 `Validator` 实例。
- **`getValidator()`**：从工厂中获取具体的 `Validator` 实例。

#### 用途：

这个方法创建了一个 `Validator` 实例，它将被用作校验方法参数的核心校验器。通过配置 `FAIL_FAST` 模式，校验器将尽快返回第一个错误，避免多次无效的校验操作。