# 4.2 强大且灵活的规则 - CRON表达式

## 什么是CRON表达式

CRON表达式类似于正则表达式，是通过一系列字母、数字、符号来组成一个可以表达时间的字符串。CRON的特点是其表达式语义明确且功能强大，是开发定时任务以及配置定时任务必须掌握的技能。

## CRON表达式详解

CRON表达式的格式为：秒 分 小时 日期 月份 星期 年（可选）构成，其每一个值的详情如下表格所示。

| 字段       | 允许值                          | 允许的特殊字符 |
| ---------- | ------------------------------- | -------------- |
| 秒         | 0-59的整数                      | ,-*/           |
| 分         | 0-59的整数                      | ,-*/           |
| 小时       | 0-23的整数                      | ,-*/           |
| 日期       | 1-31的整数（根据具体月份调整）  | ,-*/?LW        |
| 月份       | 1-12的整数（或者JAN-DEC）       | ,-*/           |
| 星期       | 1-7的整数（或者SUN-SAT, SUN=1） | ,-*/?L#        |
| 年（可选） | 1970-2099                       | ,-*/           |

## 特殊字符解释

- "?": 标识不确定的值
- ",": 指定数个值
- "-": 指定数值范围
- "/": 指定一个值的增加幅度。n/m表示从n开始，每次增加m
- "L": 用在日表示一个月中的最后一天，用在周表示该月最后一个星期X
- "W": 指定离给定日期最近的工作日（周一到周五）
- "#": 表示该月第几个周X。6#3表示该月第3个周五





### 1. `ScheduleConfig` 类

这个类用于配置定时任务执行器。它使用了 Spring 的注解，使得 `ThreadPoolTaskScheduler` 的实例可以被 Spring 管理。

```
@SpringBootConfiguration
public class ScheduleConfig {

    @Bean
    public ThreadPoolTaskScheduler taskScheduler(){
        ThreadPoolTaskScheduler taskScheduler = new ThreadPoolTaskScheduler();
        return taskScheduler;
    }
}
```

- **主要功能**：定义一个线程池定时任务调度器，允许创建和调度定时任务。

### 2. `ScheduleManager` 类

这个类是定时任务的管理器，负责启动、停止和更新定时任务。

```
@Component
@Slf4j
public class ScheduleManager {

    @Autowired
    private ThreadPoolTaskScheduler taskScheduler;

    private Map<String,ScheduleTaskHolder> cache = new ConcurrentHashMap<>();

    public String startTask(ScheduleTask scheduleTask, String cron) {
        ScheduledFuture<?> scheduledFuture = taskScheduler.schedule(scheduleTask, new CronTrigger(cron));
        String key = UUIDUtil.getUUID();
        ScheduleTaskHolder holder = new ScheduleTaskHolder(scheduleTask, scheduledFuture);
        cache.put(key, holder);
        log.info("{} 启动成功! 唯一标识为 :{}", scheduleTask.getName(), key);
        return key;
    }

    public void stopTask(String key) {
        // 逻辑省略
    }

    public String changeTask(String key, String cron) {
        // 逻辑省略
    }
}
```

- 主要功能

  ：

  - `startTask`: 启动一个新的定时任务，并将其存储在缓存中。
  - `stopTask`: 根据唯一标识停止定时任务。
  - `changeTask`: 更新定时任务的执行时间。

### 3. `ScheduleTask` 接口

这是一个简单的接口，表示一个可运行的定时任务。

```
public interface ScheduleTask extends Runnable {
    String getName();
}
```

- **主要功能**：定义一个获取任务名称的方法，要求实现此接口的类必须实现 `Runnable` 接口，表示它是一个可以被调度的任务。

### 4. `ScheduleTaskHolder` 类

这个类用于缓存定时任务及其结果。

```
@Data
@NoArgsConstructor
@AllArgsConstructor
public class ScheduleTaskHolder implements Serializable {
    private ScheduleTask scheduleTask;
    private ScheduledFuture scheduledFuture;
}
```

- **主要功能**：存储一个定时任务及其对应的 `ScheduledFuture`（用于控制定时任务的状态）。

如果一个类想实现定时任务 可以通过下面的方式进行实现

### 1. 实现示例

为了使用 `ScheduleTask` 接口，你需要创建一个具体的任务类。例如：

```
package com.imooc.pan.schedule;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyScheduledTask implements ScheduleTask {
    private static final Logger logger = LoggerFactory.getLogger(MyScheduledTask.class);

    @Override
    public String getName() {
        return "My Scheduled Task";
    }

    @Override
    public void run() {
        // 定时任务的具体逻辑
        logger.info("Executing My Scheduled Task...");
    }
}
```

### 2. 使用 `ScheduleManager`

在使用 `ScheduleManager` 启动定时任务时，你会创建这个实现类的实例并传递给 `startTask` 方法：

```
ScheduleTask myTask = new MyScheduledTask();
String cronExpression = "0/10 * * * * ?"; // 每10秒执行一次
String taskId = scheduleManager.startTask(myTask, cronExpression);
```