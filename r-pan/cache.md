# cache

@Cacheable
@cacheable:将方法运行的结果进行缓存，以后再获取相同的数据时，直接从缓存中获取，不再调用方法。使用示例如下:

```java
@Cacheable(cacheNames ={"emp" })
public Employee getEmpById(Integer id){
	Employee emp=employeeMapper.getEmpById(id);
	return emp;
}
```

@cacheable 注解的属性:
cacheNames/value ；指定缓存的名字，缓存使用CacheManager管理多个缓存组件Cache，这些Cache组件就是根据这个名字进行区分的。对缓存的真正CRUD操作在Cache中定义，每个缓存组件Cache都有自己唯一的名字，通过cacheNames或者value属性指定，相当于是将缓存的键值对进行分组，缓存的名字是一个数组，也就是说可以将一个缓存键值对分到多个组里面
key；缓存数据时的key的值，默认是使用方法参数的值，可以使用SpEL表达式计算key的值
keyGererator；缓存的生成策略，和key二选一，都是生成键的，keyGenerator可自定义
cacheManager；指定缓存管理器(如ConcurrentHashMap、Redis等)
cacheResolver；和cacheManager功能一样，和cacheManager二选- 
condition；指定缓存的条件(满足什么条件时才缓存)，可用SpEL表达式(如id>0，表示当入参id大于0时才缓存)
unless；否定缓存，即满足unless指定的条件时，方法的结果不进行缓存，使用unless时可以在调用的方法获取到结果之后再进行判断(如result==null，表示如果结果为nul时不缓存)
sync；是否使用异步模式进行缓存



# CaffeineCache

```java
@SpringBootConfiguration
@EnableCaching
@Slf4j
public class CaffeineCacheConfig {
    @Autowired
    private CaffeineCacheProperties properties;

    @Bean
    public CacheManager caffeineCacheManager(){
        CaffeineCacheManager cacheManager = new CaffeineCacheManager(CacheConstants.R_PAN_CACHE_NAME);
        cacheManager.setAllowNullValues(properties.getAllowNullValue());
        Caffeine<Object,Object> caffeineBuilder = Caffeine.newBuilder()
                .initialCapacity(properties.getInitCacheCapacity())
                .maximumSize(properties.getMaxCacheCapacity());
        cacheManager.setCaffeine(caffeineBuilder);
        log.info("the caffeine cache manager is loaded successfully!");
        return cacheManager;
    }
}
```

### 注入属性

```
@Autowired
private CaffeineCacheProperties properties;
```

通过 Spring 的依赖注入，将 `CaffeineCacheProperties` 类型的属性注入到该类中。这个类可能包含缓存的相关配置，例如允许空值、初始容量和最大容量等。

### 创建 CacheManager Bean

```
@Bean
public CacheManager caffeineCacheManager(){
```

定义一个 `caffeineCacheManager` 方法，返回一个 `CacheManager` 的实例，并用 `@Bean` 注解标记，表示该方法生成的对象会被 Spring 容器管理。

### 配置 Caffeine Cache Manager

```
CaffeineCacheManager cacheManager = new CaffeineCacheManager(CacheConstants.R_PAN_CACHE_NAME);
cacheManager.setAllowNullValues(properties.getAllowNullValue());
```

- 创建 `CaffeineCacheManager` 实例，并指定缓存名称（从 `CacheConstants` 中获取）。
- 设置是否允许缓存空值（通过 `CaffeineCacheProperties` 属性获取）。

### 设置 Caffeine 构建器

```
Caffeine<Object,Object> caffeineBuilder = Caffeine.newBuilder()
        .initialCapacity(properties.getInitCacheCapacity())
        .maximumSize(properties.getMaxCacheCapacity());
cacheManager.setCaffeine(caffeineBuilder);
```

- 使用 Caffeine 的构建器设置初始容量和最大容量。
- 将配置好的 Caffeine 构建器设置到缓存管理器中。







# RedisCache

```java
@SpringBootConfiguration
@EnableCaching
@Slf4j
@ComponentScan(value = RPanConstants.BASE_COMPONENT_SCAN_PATH + ".cache.redis.test")
public class RedisCacheConfig {


    /**
     * 定制连接和操作redis的客户端工具
     * @param redisConnectionFactory
     * @return
     */
    @Bean
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        Jackson2JsonRedisSerializer<Object> jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Object>(Object.class);
        RedisTemplate<String,Object> redisTemplate = new RedisTemplate<>();
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        redisTemplate.setConnectionFactory(redisConnectionFactory);
        redisTemplate.setKeySerializer(stringRedisSerializer);
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(stringRedisSerializer);
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);

        return redisTemplate;
    }

    /**
     * 定制化redis的缓存管理器
     * @param redisConnectionFactory
     * @return
     */
    @Bean
    public CacheManager redisCacheManager(RedisConnectionFactory redisConnectionFactory){
        RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration
                .defaultCacheConfig()
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(new StringRedisSerializer()))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class)));
        RedisCacheManager cacheManager = RedisCacheManager
                .builder(RedisCacheWriter.lockingRedisCacheWriter(redisConnectionFactory))
                .cacheDefaults(redisCacheConfiguration)
                .transactionAware()
                .build();
        log.info("the redis cache manager is load successfully");
        return  cacheManager;
    }
}
```

### `redisTemplate` 方法

```
@Bean
public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
```

#### 功能：

创建和配置 `RedisTemplate`，用于操作 Redis 数据库。

#### 主要步骤：

1. **创建 Jackson2JsonRedisSerializer**：

   ```
   Jackson2JsonRedisSerializer<Object> jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
   ```

   用于将对象序列化为 JSON 格式。

2. **创建 RedisTemplate 实例**：

   ```
   RedisTemplate<String,Object> redisTemplate = new RedisTemplate<>();
   ```

3. **设置连接工厂**：

   ```
   redisTemplate.setConnectionFactory(redisConnectionFactory);
   ```

4. **配置序列化器**：

   - 键的序列化器

     ```
     StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
     redisTemplate.setKeySerializer(stringRedisSerializer);
     ```

   - 值的序列化器

     ```
     
     redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
     ```

   - 哈希键和哈希值的序列化器

     ```
     redisTemplate.setHashKeySerializer(stringRedisSerializer);
     redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
     ```

5. **返回配置好的 RedisTemplate**：

   ```
   return redisTemplate;
   ```

### 3. `redisCacheManager` 方法

```
@Bean
public CacheManager redisCacheManager(RedisConnectionFactory redisConnectionFactory) {
```

#### 功能：

创建和配置 `CacheManager`，用于管理 Redis 缓存。

#### 主要步骤：

1. **创建 RedisCacheConfiguration**：

   ```
   RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration
           .defaultCacheConfig()
           .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(new StringRedisSerializer()))
           .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(new Jackson2JsonRedisSerializer<>(Object.class)));
   ```

   这里配置了默认的缓存配置，包括键和值的序列化方式。

2. **创建 RedisCacheManager**：

   ```
   RedisCacheManager cacheManager = RedisCacheManager
           .builder(RedisCacheWriter.lockingRedisCacheWriter(redisConnectionFactory))
           .cacheDefaults(redisCacheConfiguration)
           .transactionAware()
           .build();
   ```

   - 使用 `RedisCacheWriter` 来处理并发写入。
   - 配置默认的缓存设置。
   - 设置为支持事务。

3. **日志记录**：

   ```
   log.info("the redis cache manager is load successfully");
   ```

4. **返回配置好的 CacheManager**：

   ```
   return cacheManager;
   ```

### 总结

这个配置类主要用于初始化和配置 Redis 缓存及其操作工具。`redisTemplate` 方法用于创建用于与 Redis 交互的模板，而 `redisCacheManager` 方法用于创建缓存管理器，提供对 Redis 缓存的管理能力。整个配置提供了灵活的序列化选项和缓存管理功能，适用于需要高性能缓存的 Spring Boot 应用。
