## com.imooc.pan.web

跨域类

# cors

## CosFilter.java

**跨域（Cross-Origin）** 通常指的是前端应用和后端服务部署在不同的域名、端口或协议下。这种情况在现代Web开发中非常常见，尤其是当前后端分离的架构广泛应用时。下面，我将详细解释跨域的概念、为什么会发生跨域问题，以及如何通过您提供的 `CorsFilter` 类来解决这些问题。

### **1. 什么是跨域？**

**同源策略（Same-Origin Policy）** 是浏览器的一种安全机制，用于限制从一个源（域名、协议和端口）加载的文档或脚本如何与来自不同源的资源进行交互。具体来说，如果前端应用和后端服务不在同一个“源”下，就会触发跨域问题。

- 同源的定义：
  - **协议相同:** 比如都是 `https`。
  - **域名相同:** 比如都是 `example.com`。
  - **端口相同:** 比如都是使用端口 `443`。

**跨域的情况包括但不限于：**

- 前端在 `https://www.example.com`，后端在 `https://api.example.com`（不同子域名）。
- 前端在 `http://localhost:3000`，后端在 `http://localhost:8080`（不同端口）。
- 前端在 `https://www.example.com`，后端在 `http://www.example.com`（不同协议）。

### **2. 为什么会有跨域问题？**

浏览器的同源策略旨在保护用户的安全，防止恶意网站读取或修改其他网站的数据。例如，如果一个恶意网站能够自由访问用户在银行网站上的数据，这将带来严重的安全隐患。因此，浏览器默认禁止跨域请求，除非目标服务器明确允许。

### **3. 什么是CORS（跨域资源共享）？**

**CORS（Cross-Origin Resource Sharing）** 是一种机制，允许服务器通过设置特定的HTTP头，来指示浏览器允许哪些跨域请求。通过CORS，服务器可以控制哪些来源（域名）可以访问其资源，从而在保证安全的前提下实现必要的跨域通信。

### **4. 如何通过 `CorsFilter` 类实现CORS？**

您提供的 `CorsFilter` 类是一个自定义的CORS过滤器，用于在每个HTTP响应中添加必要的CORS头信息，从而允许跨域请求。以下是其主要功能和工作原理：

#### **4.1 添加CORS响应头**

在 `doFilter` 方法中，过滤器会调用 `addCorsResponseHeader` 方法，设置以下CORS相关的HTTP头：

- **`Access-Control-Allow-Origin: \*`**
  - 允许来自任何来源的请求。
  - **注意:** 当 `Access-Control-Allow-Credentials` 设置为 `true` 时，`Access-Control-Allow-Origin` 不能使用通配符 `*`，需要指定具体的来源。
- **`Access-Control-Allow-Credentials: true`**
  - 表示服务器允许浏览器发送凭证（如Cookies、HTTP认证信息等）。
- **`Access-Control-Allow-Methods: POST, GET, PATCH, DELETE, PUT`**
  - 指定允许的HTTP方法。
- **`Access-Control-Max-Age: 3600`**
  - 预检请求的结果可以缓存的时间（秒）。
- **`Access-Control-Allow-Headers: \*`**
  - 允许任何请求头。

```java
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    HttpServletResponse response = (HttpServletResponse)servletResponse;
    addCorsResponseHeader(response);
    filterChain.doFilter(servletRequest,servletResponse);

}
private void addCorsResponseHeader(HttpServletResponse response) {
    response.setHeader(CorsConfigEnum.CORS_ORIGIN.getKey(), CorsConfigEnum.CORS_ORIGIN.getValue());
    response.setHeader(CorsConfigEnum.CORS_CREDENTIALS.getKey(), CorsConfigEnum.CORS_CREDENTIALS.getValue());
    response.setHeader(CorsConfigEnum.CORS_METHODS.getKey(), CorsConfigEnum.CORS_METHODS.getValue());
    response.setHeader(CorsConfigEnum.CORS_MAX_AGE.getKey(), CorsConfigEnum.CORS_MAX_AGE.getValue());
    response.setHeader(CorsConfigEnum.CORS_HEADERS.getKey(), CorsConfigEnum.CORS_HEADERS.getValue());
}

/**
 * 跨域设置枚举类
 */
@AllArgsConstructor
@Getter
public enum CorsConfigEnum {
    /**
     * 允许所有远程访问
     */
    CORS_ORIGIN("Access-Control-Allow-Origin", "*"),
    /**
     * 允许认证
     */
    CORS_CREDENTIALS("Access-Control-Allow-Credentials", "true"),
    /**
     * 允许远程调用的请求类型
     */
    CORS_METHODS("Access-Control-Allow-Methods", "POST, GET, PATCH, DELETE, PUT"),
    /**
     * 指定本次预检请求的有效期，单位是秒
     */
    CORS_MAX_AGE("Access-Control-Max-Age", "3600"),
    /**
     * 允许所有请求头
     */
    CORS_HEADERS("Access-Control-Allow-Headers", "*");

    private String key;
    private String value;

}
```

# log包

## HttpLogEntity.java

 是一个用于存储和打印 HTTP 调用日志信息的实体类。它包含了请求和响应的各类详细信息，并提供了打印日志的方法。

#### **功能解析**

- **`print()` 方法**：使用日志记录器 `log` 按照预定格式打印 HTTP 调用的详细信息，包括请求时间、URI、方法、远程地址、IP、请求头、请求参数、响应状态、响应头、响应数据和处理耗时。
- **`toString()` 方法**：重写 `toString` 方法，提供同样的日志信息，但以字符串形式返回，方便在其他场景下使用。

```java
@NoArgsConstructor
@Getter
@Setter
@Accessors(chain = true)
@Slf4j
public class HttpLogEntity {

    /**
     * 请求资源
     */
    private String requestUri;

    /**
     * 被调方法
     */
    private String method;

    /**
     * 调用者地址
     */
    private String remoteAddr;

    /**
     * 调用的IP地址
     */
    private String ip;

    /**
     * 请求头
     */
    private Map<String, String> requestHeaders;

    /**
     * 请求参数
     */
    private String requestParams;

    /**
     * 响应状态
     */
    private Integer status;

    /**
     * 响应头
     */
    private Map<String, String> responseHeaders;

    /**
     * 响应数据
     */
    private String responseData;

    /**
     * 接口耗时
     */
    private String resolveTime;

    /**
     * 打印日志
     */
    public void print() {
        log.info("====================HTTP CALL START====================");
        log.info("callTime: {}", DateFormatUtils.ISO_8601_EXTENDED_DATETIME_FORMAT.format(new Date()));
        log.info("requestUri: {}", getRequestUri());
        log.info("method: {}", getMethod());
        log.info("remoteAddr: {}", getRemoteAddr());
        log.info("ip: {}", getIp());
        log.info("requestHeaders: {}", getRequestHeaders());
        log.info("requestParams: {}", getRequestParams());
        log.info("status: {}", getStatus());
        log.info("responseHeaders: {}", getResponseHeaders());
        log.info("responseData: {}", getResponseData());
        log.info("resolveTime: {}", getResolveTime());
        log.info("====================HTTP CALL FINISH====================");
    }

    @Override
    public String toString() {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("====================HTTP CALL START====================");
        stringBuilder.append("callTime: ");
        stringBuilder.append(DateFormatUtils.ISO_8601_EXTENDED_DATETIME_FORMAT.format(new Date()));
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("requestUri: ");
        stringBuilder.append(getRequestUri());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("method: ");
        stringBuilder.append(getMethod());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("remoteAddr: ");
        stringBuilder.append(getRemoteAddr());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("ip: ");
        stringBuilder.append(getIp());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("requestHeaders: ");
        stringBuilder.append(getRequestHeaders());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("requestParams: ");
        stringBuilder.append(getRequestParams());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("status: ");
        stringBuilder.append(getStatus());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("responseHeaders: ");
        stringBuilder.append(getResponseHeaders());
        stringBuilder.append(System.lineSeparator());


        stringBuilder.append("responseData: ");
        stringBuilder.append(getResponseData());
        stringBuilder.append(System.lineSeparator());


        stringBuilder.append("resolveTime: ");
        stringBuilder.append(getResolveTime());
        stringBuilder.append(System.lineSeparator());

        stringBuilder.append("====================HTTP CALL FINISH====================");
        return stringBuilder.toString();
    }

}
```









### **示例 HTTP 请求**

```
POST /api/users HTTP/1.1
Host: www.example.com
Connection: keep-alive
Content-Length: 85
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/plain, */*
Origin: https://www.clientapp.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
 Chrome/113.0.0.0 Safari/537.36
Content-Type: application/json;charset=UTF-8
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.clientapp.com/register
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7

{
  "username": "john_doe",
  "password": "SecureP@ssw0rd!",
  "email": "john.doe@example.com"
}
```

------

### **详细解析**

#### **1. 请求行（Request Line）**

```
POST /api/users HTTP/1.1
```

- **方法（Method）**: `POST`
  表示客户端希望向服务器提交数据，通常用于创建新的资源。
- **请求目标（Request Target）**: `/api/users`
  指定请求的资源路径，这里是服务器上的 `/api/users` 端点。
- **HTTP 版本（HTTP Version）**: `HTTP/1.1`
  表示使用的 HTTP 协议版本。

#### **2. 请求头（Headers）**

```yaml
Host: www.example.com
Connection: keep-alive
Content-Length: 85
Pragma: no-cache
Cache-Control: no-cache
Accept: application/json, text/plain, */*
Origin: https://www.clientapp.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
 Chrome/113.0.0.0 Safari/537.36
Content-Type: application/json;charset=UTF-8
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.clientapp.com/register
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
```

- **Host**: `www.example.com`
  指定目标服务器的域名和端口。
- **Connection**: `keep-alive`
  表示客户端希望保持连接以便进行后续的请求，减少握手次数。
- **Content-Length**: `85`
  请求体的长度（以字节为单位）。
- **Pragma** 和 **Cache-Control**: `no-cache`
  指示浏览器不要缓存请求和响应内容。
- **Accept**: `application/json, text/plain, */*`
  指定客户端能够接受的内容类型。
- **Origin**: `https://www.clientapp.com`
  指示请求的来源，常用于跨域请求。
- **User-Agent**:
  描述发起请求的客户端软件信息，这里是一个典型的浏览器标识。
- **Content-Type**: `application/json;charset=UTF-8`
  指定请求体的媒体类型，这里是 JSON 格式，使用 UTF-8 编码。
- **Sec-Fetch-Site**, **Sec-Fetch-Mode**, **Sec-Fetch-Dest**:
  这些是安全相关的请求头，用于指示请求的上下文和目的，帮助服务器进行更精确的安全控制。
- **Referer**: `https://www.clientapp.com/register`
  指示发起请求的页面地址。
- **Accept-Encoding**: `gzip, deflate, br`
  指定客户端能够处理的内容编码方式，用于压缩响应体。
- **Accept-Language**: `en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7`
  指定客户端首选的语言，服务器可根据此信息返回相应语言的内容。

#### **3. 请求体（Request Body）**

```
{
  "username": "john_doe",
  "password": "SecureP@ssw0rd!",
  "email": "john.doe@example.com"
}
```

- 内容

  :

  一个 JSON 对象，包含用户注册所需的信息：

  - **username**: 用户名
  - **password**: 密码
  - **email**: 电子邮件地址

------

### **如何通过您的 `HttpLogFilter` 记录这个请求**

根据您提供的 `HttpLogFilter` 实现，以下是该请求在日志中可能被记录的内容：

1. **请求 URI**: `/api/users`

2. **HTTP 方法**: `POST`

3. **远程地址（Remote Address）**: 例如，`192.168.1.10`

4. **IP 地址（IP）**: 例如，`203.0.113.45`

5. 请求头（Request Headers）

   :

   ```
   {
     "Host": "www.example.com",
     "Connection": "keep-alive",
     "Content-Length": "85",
     "Pragma": "no-cache",
     "Cache-Control": "no-cache",
     "Accept": "application/json, text/plain, */*",
     "Origin": "https://www.clientapp.com",
     "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36",
     "Content-Type": "application/json;charset=UTF-8",
     "Sec-Fetch-Site": "cross-site",
     "Sec-Fetch-Mode": "cors",
     "Sec-Fetch-Dest": "empty",
     "Referer": "https://www.clientapp.com/register",
     "Accept-Encoding": "gzip, deflate, br",
     "Accept-Language": "en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7"
   }
   ```

6. 请求参数（Request Params）

   :

   ```
   {
     "username": "john_doe",
     "password": "SecureP@ssw0rd!",
     "email": "john.doe@example.com"
   }
   ```

7. **响应状态（Status）**: 例如，`201`（Created）

8. 响应头（Response Headers）

   :

   ```
   {
     "Content-Type": "application/json;charset=UTF-8"
   }
   ```

9. 响应数据（Response Data）

   :

   ```
   {
     "id": 12345,
     "username": "john_doe",
     "email": "john.doe@example.com",
     "createdAt": "2024-04-27T12:34:56Z"
   }
   ```

10. **接口耗时（Resolve Time）**: 例如，`StopWatch[id=1, time=250ms]`

------

### **对应的日志输出示例**

根据您的 `HttpLogEntity.print()` 方法，日志可能如下所示：

```
====================HTTP CALL START====================
callTime: 2024-04-27T12:34:56Z
requestUri: /api/users
method: POST
remoteAddr: 192.168.1.10
ip: 203.0.113.45
requestHeaders: {Host=www.example.com, Connection=keep-alive, Content-Length=85, Pragma=no-cache, Cache-Control=no-cache, Accept=application/json, text/plain, */*, Origin=https://www.clientapp.com, User-Agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36, Content-Type=application/json;charset=UTF-8, Sec-Fetch-Site=cross-site, Sec-Fetch-Mode=cors, Sec-Fetch-Dest=empty, Referer=https://www.clientapp.com/register, Accept-Encoding=gzip, deflate, br, Accept-Language=en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7}
requestParams: {"username":"john_doe","password":"SecureP@ssw0rd!","email":"john.doe@example.com"}
status: 201
responseHeaders: {contentType=application/json;charset=UTF-8}
responseData: {"id":12345,"username":"john_doe","email":"john.doe@example.com","createdAt":"2024-04-27T12:34:56Z"}
resolveTime: StopWatch[id=1, time=250ms]
====================HTTP CALL FINISH====================
```

------

### **解释**

1. **请求部分**:
   - **请求 URI**、**方法**、**远程地址**和**IP**：基本的请求信息，帮助了解请求来源和目标。
   - **请求头**：详细记录了客户端发送的所有 HTTP 头信息，包括内容类型、接受的格式、来源等。
   - **请求参数**：对于 POST 请求，记录了请求体的 JSON 数据；对于 GET 请求，则记录了查询参数。
2. **响应部分**:
   - **状态码**：`201` 表示资源已成功创建。
   - **响应头**：记录了服务器返回的 HTTP 头信息。
   - **响应数据**：服务器返回的 JSON 数据，包含新创建用户的详细信息。
   - **接口耗时**：记录了处理该请求所花费的时间，帮助进行性能分析。





## HttpLogEntityBuilder.java

**`getIpAddress` 方法**

```java
public static String getIpAddress(HttpServletRequest request) {
    if (request == null) {
        return "unknown";
    }
    String ip = request.getHeader("x-forwarded-for");
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("X-Forwarded-For");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("WL-Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("X-Real-IP");
    }

    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getRemoteAddr();
    }

    return "0:0:0:0:0:0:0:1".equals(ip) ? "127.0.0.1" : ip;
}
```

##### **功能解析**

- **目的**：获取客户端的真实IP地址，尤其在存在反向代理或负载均衡器的情况下。

- **逻辑**：

  1. 检查 `X-Forwarded-For`

     ：

     - 该头通常用于传递原始客户端IP。

  2. 检查其他代理相关的头

     ：

     - 如 `Proxy-Client-IP`、`X-Forwarded-For`（重复）、`WL-Proxy-Client-IP`、`X-Real-IP` 等。

  3. 最后，使用 `getRemoteAddr`

     ：

     - 如果上述头信息都不可用，则使用 `request.getRemoteAddr()` 获取直接的远程地址。

  4. 处理IPv6本地回环地址

     ：

     - 如果IP为 `0:0:0:0:0:0:0:1`（IPv6的本地回环地址），则转换为IPv4的 `127.0.0.1`。

#### **`getRequestHeaderMap` 方法**

```java
public static Map<String, String> getRequestHeaderMap(HttpServletRequest request) {
    Map<String, String> result = Maps.newHashMap();
    if (Objects.nonNull(request)) {
        Enumeration<String> headerNames = request.getHeaderNames();
        if (Objects.nonNull(headerNames)) {
            while (headerNames.hasMoreElements()) {
                String headerName = headerNames.nextElement();
                String headerValue = request.getHeader(headerName);
                result.put(headerName, headerValue);
            }
        }
    }
    return result;
}
```

##### **功能解析**

- **目的**：提取所有请求头信息，并存储在一个 `Map` 中，键为头名称，值为头的值。

- **逻辑**：

  1. **检查请求对象是否非空**。

  2. 获取所有头名称

     ：

     - 使用 `request.getHeaderNames()` 获取所有头的枚举。

  3. 遍历头名称

     ：

     - 逐一获取每个头的值，并存入 `result` Map 中。

  4. **返回包含所有请求头的Map**。

#### **`getResponseHeaderMap` 方法**

```
java复制代码public static Map<String, String> getResponseHeaderMap(HttpServletResponse response) {
    Map<String, String> result = Maps.newHashMap();
    if (Objects.nonNull(response)) {
        String contentType = response.getContentType();
        result.put("contentType", contentType);
        // 如果需要，可以在此处添加更多响应头的获取逻辑
    }
    return result;
}
```

##### **功能解析**

- **目的**：提取响应的头信息，并存储在一个 `Map` 中。

- **逻辑**：

  1. **检查响应对象是否非空**。

  2. 获取 `Content-Type`

     ：

     - 使用 `response.getContentType()` 获取响应的内容类型，并存入 `result` Map 中，键为 `"contentType"`。

  3. **返回包含响应头的Map**。

### **核心方法：`build`**

```java
public static HttpLogEntity build(ContentCachingRequestWrapper requestWrapper, ContentCachingResponseWrapper responseWrapper, StopWatch stopWatch) {
    // 创建一个新的HttpLogEntity实例
    HttpLogEntity httpLogEntity = new HttpLogEntity();
    
    // 设置请求URI
    httpLogEntity.setRequestUri(requestWrapper.getRequestURI())
            // 设置HTTP方法（如GET、POST等）
            .setMethod(requestWrapper.getMethod())
            // 设置远程地址（发起请求的客户端地址）
            .setRemoteAddr(requestWrapper.getRemoteAddr())
            // 设置请求者的真实IP地址
            .setIp(getIpAddress(requestWrapper))
            // 设置请求头信息
            .setRequestHeaders(getRequestHeaderMap(requestWrapper));
    
    // 根据HTTP方法设置请求参数
    if (requestWrapper.getMethod().equals(RequestMethod.GET.name())) {
        // 对于GET请求，将参数映射转换为JSON字符串
        httpLogEntity.setRequestParams(JSON.toJSONString(requestWrapper.getParameterMap()));
    } else {
        // 对于其他请求（如POST），将请求体内容转换为字符串
        httpLogEntity.setRequestParams(new String(requestWrapper.getContentAsByteArray()));
    }
    
    // 获取响应的Content-Type
    String responseContentType = responseWrapper.getContentType();
    if (StringUtils.equals("application/json;charset=UTF-8", responseContentType)) {
        // 如果响应类型为JSON，记录响应体内容
        httpLogEntity.setResponseData(new String(responseWrapper.getContentAsByteArray()));
    } else {
        // 否则，记录为"Stream Body..."，避免记录大量或不易阅读的内容
        httpLogEntity.setResponseData("Stream Body...");
    }
    
    // 设置响应状态码
    httpLogEntity.setStatus(responseWrapper.getStatusCode())
            // 设置响应头信息
            .setResponseHeaders(getResponseHeaderMap(responseWrapper))
            // 设置请求处理耗时
            .setResolveTime(stopWatch.toString());
    
    return httpLogEntity;
}
```

#### **方法参数**

- `ContentCachingRequestWrapper requestWrapper`

  ：

  - 包装后的HTTP请求对象，缓存了请求体内容，便于多次读取。

- `ContentCachingResponseWrapper responseWrapper`

  ：

  - 包装后的HTTP响应对象，缓存了响应体内容，便于多次读取。

- `StopWatch stopWatch`

  ：

  - 用于记录请求处理的耗时。

#### **方法功能**

1. **创建 `HttpLogEntity` 实例**：
   - 实例化一个新的 `HttpLogEntity` 对象，用于存储HTTP调用的详细信息。
2. **设置基本请求信息**：
   - **`requestUri`**：获取请求的URI，例如 `/api/users`。
   - **`method`**：获取HTTP方法，例如 `GET`、`POST`。
   - **`remoteAddr`**：获取远程地址，即客户端的地址。
   - **`ip`**：通过 `getIpAddress` 方法获取请求者的真实IP地址。
   - **`requestHeaders`**：通过 `getRequestHeaderMap` 方法获取所有请求头信息，并存储在Map中。



## HttpLogFilter.java

```java
@WebFilter(filterName = "httpLogFilter")
@Slf4j
@Order(Integer.MAX_VALUE)
public class HttpLogFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        StopWatch stopWatch = StopWatch.createStarted();
        ContentCachingRequestWrapper requestWrapper = new ContentCachingRequestWrapper(request);
        ContentCachingResponseWrapper responseWrapper = new ContentCachingResponseWrapper(response);
        filterChain.doFilter(requestWrapper, responseWrapper);
        HttpLogEntity httpLogEntity = HttpLogEntityBuilder.build(requestWrapper, responseWrapper, stopWatch);
        httpLogEntity.print();
        responseWrapper.copyBodyToResponse();
    }

}
```

### **2.1 方法概述**

`doFilterInternal` 是 `OncePerRequestFilter` 抽象类中需要实现的核心方法。它定义了过滤器在处理请求和响应时的具体逻辑。

### **2.2 步骤解析**

1. **启动计时器**

   ```java
   StopWatch stopWatch = StopWatch.createStarted();
   ```

   - **功能**：使用 `StopWatch` 记录整个请求处理过程的耗时。
   - **用途**：后续用于分析请求的响应时间，帮助进行性能调优。

2. **包装请求和响应**

   ```java
   ContentCachingRequestWrapper requestWrapper = new ContentCachingRequestWrapper(request);
   ContentCachingResponseWrapper responseWrapper = new ContentCachingResponseWrapper(response);
   ```

   - `ContentCachingRequestWrapper`
     - **功能**：包装原始的 `HttpServletRequest`，缓存请求体的内容。这样可以在后续处理过程中多次读取请求体，而不会导致输入流被消耗。
   - `ContentCachingResponseWrapper`
     - **功能**：包装原始的 `HttpServletResponse`，缓存响应体的内容。允许在记录日志后，将响应内容写回到实际的响应中，确保客户端能够收到正确的响应数据。

3. **执行过滤器链**

   ```java
   filterChain.doFilter(requestWrapper, responseWrapper);
   ```

   - **功能**：将包装后的请求和响应对象传递给过滤器链中的下一个过滤器或目标资源（如控制器）。
   - **作用**：实际处理HTTP请求并生成响应。

4. **构建日志实体**

   ```java
   HttpLogEntity httpLogEntity = HttpLogEntityBuilder.build(requestWrapper, responseWrapper, stopWatch);
   ```

   - **功能**：使用 `HttpLogEntityBuilder` 类，根据包装后的请求和响应对象以及计时器，构建一个 `HttpLogEntity` 实例，包含所有需要记录的日志信息。

5. **打印日志**

   ```java
   httpLogEntity.print();
   ```

   - **功能**：调用 `HttpLogEntity` 的 `print` 方法，将收集到的HTTP调用信息记录到日志系统中。
   - **输出内容**：包括请求URI、HTTP方法、远程地址、IP地址、请求头、请求参数、响应状态、响应头、响应数据以及接口耗时等详细信息。

6. **复制响应体**

   ```java
   responseWrapper.copyBodyToResponse();
   ```

   - **功能**：将缓存的响应体内容复制回实际的 `HttpServletResponse` 对象，确保客户端能够接收到正确的响应数据。
   - **必要性**：由于使用了 `ContentCachingResponseWrapper` 缓存响应体，需要手动将其内容写回，否则客户端将无法收到响应。

### **2.3 详细解释**

- **计时器 `StopWatch` 的使用**

  - **目的**：衡量从请求开始到响应结束的总耗时，有助于性能监控和瓶颈分析。
  - **启动位置**：在请求进入过滤器时立即启动，确保准确记录整个处理过程的时间。

- **请求和响应的包装**

  - **原因**：默认情况下，`HttpServletRequest` 和 `HttpServletResponse` 的输入流只能读取一次。通过 `ContentCachingRequestWrapper` 和 `ContentCachingResponseWrapper` 进行包装，可以多次读取内容，便于日志记录和其他需要重复读取的操作。

- **日志实体的构建**

  - `HttpLogEntityBuilder.build` 方法

    ：

    - **功能**：从包装后的请求和响应对象中提取必要的信息，如请求URI、方法、参数、响应状态、响应数据等。
    - **封装**：将这些信息封装到 `HttpLogEntity` 对象中，提供统一的日志记录接口。

- **日志的打印**

  - **方式**：调用 `httpLogEntity.print()` 方法，使用 `Slf4j` 提供的日志记录器将信息记录到日志文件或控制台。
  - **格式**：根据 `HttpLogEntity` 的实现，日志内容结构化、易于阅读和分析。

- **响应体的复制**

  - **必要性**：由于响应内容被缓存，必须将其重新写回到实际的响应对象中，否则客户端将无法接收到响应。
  - **方法**：调用 `copyBodyToResponse` 方法，将缓存的响应内容复制回 `HttpServletResponse`。

## **3. 整体流程**

1. **请求到达过滤器**
   - 客户端发送HTTP请求，进入 `HttpLogFilter` 过滤器。
2. **请求和响应的包装**
   - 请求被 `ContentCachingRequestWrapper` 包装，响应被 `ContentCachingResponseWrapper` 包装，以便缓存内容。
3. **执行业务逻辑**
   - 通过 `filterChain.doFilter` 方法，请求被传递给下一个过滤器或目标资源（如控制器）进行处理。
4. **构建和记录日志**
   - 请求处理完成后，使用 `HttpLogEntityBuilder` 构建 `HttpLogEntity` 对象，记录所有相关的请求和响应信息。
   - 调用 `print` 方法，将日志信息输出到日志系统中。
5. **复制响应体**
   - 将缓存的响应内容复制回实际的 `HttpServletResponse` 对象，确保客户端接收到完整的响应。