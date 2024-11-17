#### com.imooc.pan.core.utils

工具包 其中包含以下类：

##### AES128Util.java

- 用 AES/CBC/PKCS5Padding进行加密，包括四个函数，分别对bye[],String进行加解密

- ### 1. AES（高级加密标准）

  - **AES（Advanced Encryption Standard）** 是一种对称加密算法，用于加密和解密数据。它是由美国国家标准与技术研究院（NIST）在2001年公布的，成为了全球广泛使用的标准。
  - AES 支持三种密钥长度：128 位、192 位和 256 位。使用较长的密钥通常可以提供更高的安全性。
  - AES 是一种快速且安全的加密算法，广泛用于保护敏感数据，如金融信息和个人隐私。

  ### 2. CBC（密码块链接模式）

  - **CBC（Cipher Block Chaining）** 是一种加密模式，它用于将多个数据块串联在一起进行加密。
  - 在 CBC 模式中，每个明文块在加密前都会与前一个密文块进行异或（XOR）运算。这意味着每个块的加密结果都依赖于前一个块，这样可以增加加密的安全性。
  - 第一个块通常需要一个初始化向量（IV），这是一个随机生成的值，确保同样的明文在每次加密时产生不同的密文。
  - CBC 模式可以防止某些攻击（如重放攻击），但如果 IV 管理不当，可能会导致安全问题。

  ### 3. PKCS5Padding（填充方案）

  - **PKCS5Padding** 是一种填充方案，用于确保明文的长度是块大小的整数倍。在 AES 中，块大小为 16 字节。
  - 如果明文的长度不是块大小的整数倍，PKCS5Padding 会在明文的末尾添加一定数量的填充字节，以使其满足块大小的要求。每个填充字节的值都等于填充字节的数量。
    - 例如，如果要填充 3 个字节，填充内容为 `03 03 03`。
  - 填充使得算法能够处理任意长度的输入数据，确保加密和解密过程的正确性。

- ```java
      public static byte[] aesEncrypt(byte[] content) {
          try {
              SecretKeySpec secretKeySpec = new SecretKeySpec(P_KEY.getBytes(), AES_STR);
              Cipher cipher = Cipher.getInstance(INSTANCE_STR);
              IvParameterSpec iv = new IvParameterSpec(IV.getBytes());
              cipher.init(Cipher.ENCRYPT_MODE, secretKeySpec, iv);
              byte[] encrypted = cipher.doFinal(content);
              return encrypted;
          } catch (Exception e) {
              e.printStackTrace();
          }
          return null;
      }
  ```

##### IdUtil.java

- 使用**雪花算法** (Snowflake Algorithm) 生成唯一 ID 的实现，可为分布式系统生成全局唯一ID。

- 生成机器ID，获取每个网络接口的信息.toSting后取hashcode。

- ```java
  private static long getMachineNum() {
      long machinePiece;
      StringBuilder sb = new StringBuilder();
      Enumeration<NetworkInterface> e = null;
      try {
          e = NetworkInterface.getNetworkInterfaces();
      } catch (SocketException e1) {
          e1.printStackTrace();
      }
      while (e.hasMoreElements()) {
          NetworkInterface ni = e.nextElement();
          sb.append(ni.toString());
      }
      machinePiece = sb.toString().hashCode();
      return machinePiece;
  }
  ```

- 获取当前最新时间戳，确保时间没有异常回退

- ```java
  private static long tilNextMillis(long lastTimestamp) {
      long timestamp = timeGen();
      while (timestamp <= lastTimestamp) {
          timestamp = timeGen();
      }
      return timestamp;
  }
  ```

- ID生成算法，先获取当前时间戳如果小于上次时间戳，则表示时间戳获取出现异常，获取当前时间戳如果等于上次时间戳说明还处在同一毫秒内，则在序列号加1；否则序列号赋值为0，从0开始。
- 时间戳41位，数据中心ID5位，工作ID5位，序列号12位，共同组成64位Long的id好，可以最大程度上确保唯一性。
- 时间戳的位数
  - **41位**：可以表示约69年的时间（`2^41 / (1000 * 60 * 60 * 24 * 365) ≈ 69`），足够满足大部分应用的需求。
- 数据中心ID和工作ID的位数
  - **各5位**：可以表示最多32个数据中心和32个工作节点。这在大多数分布式系统中是足够的。如果需要更多的数据中心或工作节点，可以增加位数，适当调整其他部分的位数。
- 序列号的位数
  - **12位**：可以在同一毫秒内生成最多4096个ID，适用于高并发场景。

- ```java
  public synchronized static Long get() {
      long timestamp = timeGen();
      // 获取当前时间戳如果小于上次时间戳，则表示时间戳获取出现异常
      if (timestamp < lastTimestamp) {
          System.err.printf("clock is moving backwards.  Rejecting requests until %d.", lastTimestamp);
          throw new RuntimeException(String.format("Clock moved backwards.  Refusing to generate id for %d milliseconds", lastTimestamp - timestamp));
      }
  
      // 获取当前时间戳如果等于上次时间戳
      // 说明：还处在同一毫秒内，则在序列号加1；否则序列号赋值为0，从0开始。
      // 0 - 4095
      if (lastTimestamp == timestamp) {
          sequence = (sequence + 1) & sequenceMask;
          if (sequence == 0) {
              timestamp = tilNextMillis(lastTimestamp);
          }
      } else {
          sequence = 0;
      }
      //将上次时间戳值刷新
      lastTimestamp = timestamp;
  
      /**
       * 返回结果：
       * (timestamp - twepoch) << timestampLeftShift) 表示将时间戳减去初始时间戳，再左移相应位数
       * (datacenterId << datacenterIdShift) 表示将数据id左移相应位数
       * (workerId << workerIdShift) 表示将工作id左移相应位数
       * | 是按位或运算符，例如：x | y，只有当x，y都为0的时候结果才为0，其它情况结果都为1。
       * 因为个部分只有相应位上的值有意义，其它位上都是0，所以将各部分的值进行 | 运算就能得到最终拼接好的id
       */
      return ((timestamp - startTimestamp) << timestampLeftShift) |
              (dataCenterId << dataCenterIdShift) |
              (workerId << workerIdShift) |
              sequence;
  }
  ```

##### JwtUtil.java

- **JWT**（**J**SON **W**eb **T**oken）是一种用于在各方之间安全地传输声明（claims）的紧凑且自包含的方式。它通常用于身份验证和授权机制中，允许服务器生成一个包含用户信息的令牌，并将其发送给客户端。客户端在后续的请求中携带这个令牌，以便服务器验证用户身份和权限。

- 生成token函数如下，返回的JWT遵循<Header>.<Payload>.<Signature>

- ```java
  public static String generateToken(String subject, String claimKey, Object claimValue, Long expire) {
      String token = Jwts.builder()
              .setSubject(subject)
              .claim(claimKey, claimValue)
              .claim(RENEWAL_TIME, new Date(System.currentTimeMillis() + expire / TWO_LONG))
              .setExpiration(new Date(System.currentTimeMillis() + expire))
              .signWith(SignatureAlgorithm.HS256, JWT_PRIVATE_KEY)
              .compact();
      return token;
  }
  ```

- `Jwts.builder()`：创建一个JWT构建器。

  `.setSubject(subject)`：设置JWT的主题（通常是用户标识）。

  `.claim(claimKey, claimValue)`：添加一个自定义声明，键为 `claimKey`，值为 `claimValue`。

  `.claim(RENEWAL_TIME, new Date(System.currentTimeMillis() + expire / TWO_LONG))`：添加一个名为 `RENEWAL_TIME` 的自定义声明，值为当前时间加上 `expire` 时间的一半，表示刷新时间。

  `.setExpiration(new Date(System.currentTimeMillis() + expire))`：设置JWT的过期时间为当前时间加上 `expire` 毫秒。

  `.signWith(SignatureAlgorithm.HS256, JWT_PRIVATE_KEY)`：使用HS256算法和指定的秘钥对JWT进行签名。

  `.compact()`：将构建器中的信息压缩成一个紧凑的JWT字符串。

  `return token;`：返回生成的JWT字符串。

- 用于解析和验证一个JWT（JSON Web Token），并提取其中指定的声明（claim）值

- 

- ```java
  public static Object analyzeToken(String token, String claimKey) {
      if (StringUtils.isBlank(token)) {
          return null;
      }
      try {
          Claims claims = Jwts.parser()
                  .setSigningKey(JWT_PRIVATE_KEY)
                  .parseClaimsJws(token)
                  .getBody();
          return claims.get(claimKey);
      } catch (Exception e) {
          return null;
      }
  }
  ```