**涉及的业务难点
怎样实现用户名全局唯一的幂等性注册接口**

主要通过捕获数据库depulicatkey异常实现

**怎样实现全局统一登录拦截**

LoginIgnore注解不需要验证的类，在切点server下所有包的controller中的所有方法中执行增强逻辑，先校验是否有LoginIgnore，没有对accesstoken进行解析，与redis中存储的accesstoken进行比较，相同则校验通过不需要登录，任何一者为空或者不相同提升登录。

### **1. `LoginIgnore` 注解类**

#### **类定义：**

```
java复制代码@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
public @interface LoginIgnore {
}
```

#### **作用：**

`LoginIgnore` 是一个自定义注解，用于标记那些不需要进行登录验证的方法。被标记为 `@LoginIgnore` 的方法会跳过统一的登录拦截逻辑。

#### **解释：**

- `@Documented`：表示该注解会包含在 Javadoc 中，方便查看该注解的用途。
- `@Retention(RetentionPolicy.RUNTIME)`：注解会在运行时通过反射获取，因此可以在方法执行时检查该注解是否存在。
- `@Target({ElementType.METHOD})`：注解只作用于方法上。

通过使用 `@LoginIgnore` 注解，开发人员可以在控制器方法上标记那些不需要进行登录验证的接口。例如，登录接口或公开接口可能不需要登录校验。

#### **如何使用：**

```java
@RestController
public class UserController {

    // 登录接口，不需要进行登录校验
    @LoginIgnore
    @PostMapping("/login")
    public ResponseEntity<?> login() {
        // 登录逻辑
        return ResponseEntity.ok("登录成功");
    }
}
```

### **2. `CommonLoginAspect` 切面类**

#### **类定义：**

```java
@Component
@Aspect
@Slf4j
public class CommonLoginAspect {
    
    private static final String LOGIN_AUTH_REQUEST_HEADER_NAME = "Authorization";
    private static final String LOGIN_AUTH_PARAM_NAME = "authorization";
    private final static String POINT_CUT = "execution(* com.imooc.pan.server.modules.*.controller..*(..))";

    @Autowired
    private CacheManager cacheManager;

    @Pointcut(value = POINT_CUT)
    public void loginAuth() {}

    @Around("loginAuth()")
    public Object loginAuthAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        if (checkNeedCheckLoginInfo(proceedingJoinPoint)) {
            ServletRequestAttributes servletRequestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
            HttpServletRequest request = servletRequestAttributes.getRequest();
            String requestURI = request.getRequestURI();
            log.info("成功拦截到请求，URI为：{}", requestURI);
            if (!checkAndSaveUserId(request)) {
                log.warn("成功拦截到请求，URI为：{}. 检测到用户未登录，将跳转至登录页面", requestURI);
                return R.fail(ResponseCode.NEED_LOGIN);
            }
            log.info("成功拦截到请求，URI为：{}，请求通过", requestURI);
        }
        return proceedingJoinPoint.proceed();
    }

    private boolean checkAndSaveUserId(HttpServletRequest request) {
        String accessToken = request.getHeader(LOGIN_AUTH_REQUEST_HEADER_NAME);
        if (StringUtils.isBlank(accessToken)) {
            accessToken = request.getParameter(LOGIN_AUTH_PARAM_NAME);
        }
        if (StringUtils.isBlank(accessToken)) {
            return false;
        }
        Object userId = JwtUtil.analyzeToken(accessToken, UserConstants.LOGIN_USER_ID);
        if (Objects.isNull(userId)) {
            return false;
        }

        Cache cache = cacheManager.getCache(CacheConstants.R_PAN_CACHE_NAME);
        String redisAccessToken = cache.get(UserConstants.USER_LOGIN_PREFIX + userId, String.class);

        if (StringUtils.isBlank(redisAccessToken)) {
            return false;
        }

        if (Objects.equals(accessToken, redisAccessToken)) {
            saveUserId(userId);
            return true;
        }

        return false;
    }

    private void saveUserId(Object userId) {
        UserIdUtil.set(Long.valueOf(String.valueOf(userId)));
    }

    private boolean checkNeedCheckLoginInfo(ProceedingJoinPoint proceedingJoinPoint) {
        Signature signature = proceedingJoinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();
        return !method.isAnnotationPresent(LoginIgnore.class);
    }
}
```

#### **类功能：**

`CommonLoginAspect` 是一个 AOP（面向切面编程）切面类，负责对所有请求进行登录校验。它会拦截控制器方法，检查用户是否已登录。如果未登录，则返回一个提示用户登录的响应；如果已登录，则继续执行请求。

#### **详细解释：**

##### **1. `@Aspect` 注解：**

- `@Aspect` 表示这是一个切面类，用于在特定的地方（如方法执行前后）插入增强逻辑。

##### **2. `@Component` 注解：**

- `@Component` 使得这个类成为一个 Spring 管理的 Bean，可以注入到其他组件中。

##### **3. `@Pointcut` 注解：**

- `@Pointcut(value = POINT_CUT)` 定义了切点表达式。`POINT_CUT` 用于指定哪些方法需要被拦截。
- 切点的表达式 `execution(* com.imooc.pan.server.modules.*.controller..*(..))` 表示拦截 `com.imooc.pan.server.modules` 包下的所有控制器方法。

##### **4. `@Around` 注解：**

- `@Around("loginAuth()")` 环绕通知，在方法执行前后加入逻辑。
- 在执行方法前，`loginAuthAround` 方法会被触发，检查请求是否需要登录校验。如果需要校验，则检查 Token 是否有效。

##### **5. 登录验证逻辑：**

- `checkNeedCheckLoginInfo`：检查方法是否标记了 `@LoginIgnore` 注解。如果方法上有 `@LoginIgnore` 注解，表示此方法不需要进行登录校验，直接返回 `false`，跳过后续校验逻辑。

- ```
  checkAndSaveUserId
  ```

  ：从请求头或参数中获取 Token，并验证该 Token 是否有效。验证过程包括：

  - 从请求中获取 Token。
  - 使用 `JwtUtil` 解码 Token，并提取其中的 `userId`。
  - 从缓存中获取存储的 Token，与请求中的 Token 比较，确保 Token 一致。
  - 如果 Token 有效，则将 `userId` 存入线程上下文中，供下游使用。

##### **6. `saveUserId` 方法：**

- 如果 Token 验证通过，`userId` 会存储到线程上下文中（使用 `UserIdUtil.set`），以供后续的请求处理使用。

##### **7. 登录失败处理：**

- 如果 Token 无效或未提供 Token，`loginAuthAround` 方法会返回 `R.fail(ResponseCode.NEED_LOGIN)`，提示用户登录。

### **总结：**

- **`LoginIgnore` 注解**：用于标记那些不需要登录验证的方法，跳过登录校验逻辑。
- **`CommonLoginAspect` 切面类**：通过 AOP 机制，统一拦截控制器方法，检查请求是否包含有效的 Token，如果未登录则返回登录提示。如果方法上有 `@LoginIgnore` 注解，跳过该方法的登录验证。

这两个类结合使用，实现了灵活且可扩展的登录验证机制，可以在大部分请求中统一处理登录校验，而对于特定接口（如登录接口），则通过 `@LoginIgnore` 注解跳过校验。