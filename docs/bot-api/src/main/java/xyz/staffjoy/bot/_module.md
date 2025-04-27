# 基础信息

|      |      |
|------|------|
| 名称 | bot |
| 编码语言 | .java |
| 代码路径 | staffjoy/bot-api/src/main/java/xyz/staffjoy/bot |
| 包名 | staffjoy.docs.bot-api.src.main.java.xyz.staffjoy.bot |
| 概述说明 | BotClient接口用于与Bot服务通信，包含7个POST方法处理短信、入职和班次提醒等功能。DTO类简化请求数据封装，涉及班次、入职和问候场景。BotConstant类管理服务配置和消息模板。 |

# 说明

## 概述
该代码模块是`staffjoy/bot-api`模块的核心组件，主要实现与机器人服务（Bot Service）的交互功能。模块包含以下三部分核心内容：

1. **Feign客户端接口**（`BotClient`）：  
   通过Feign声明式HTTP客户端与Bot服务进行通信，配置了服务发现名称和基础路径，提供7个标准化POST方法覆盖全部业务场景。

2. **数据传输对象**（DTO包）：  
   使用Lombok注解简化代码，包含8个请求体类，分别处理班次提醒、员工入职、问候等业务场景的数据封装，所有字段均实现非空校验。

3. **配置常量类**（`BotConstant`）：  
   集中管理服务名称、消息模板代码（含3种短信模板和2种邮件模板）、时间格式、班次显示规范等全局参数，维护10天的班次提醒窗口配置。

## 主要业务场景
### 1. 员工入职流程
- **触发场景**：新员工加入公司时
- **实现方式**：
  - 通过`OnboardWorkerRequest` DTO传递公司ID和用户ID
  - 调用`BotClient.onboardWorker()`方法
  - 使用`BotConstant.ONBOARDING_SMS_*`模板代码发送入职短信
  - 可选发送`WELCOME_EMAIL`模板的欢迎邮件

### 2. 班次管理提醒
- **新增班次提醒**：
  - 单班次：`AlertNewShiftRequest` + `alertNewShift()`
  - 批量班次：`AlertNewShiftsRequest` + `alertNewShifts()`
  - 使用`NEW_SHIFT_SMS_TEMPLATE`模板
  - 按`BotConstant.SHIFT_DISPLAY_FORMAT`格式化班次信息

- **移除班次提醒**：
  - 单班次：`AlertRemovedShiftRequest` + `alertRemovedShift()`
  - 批量班次：`AlertRemovedShiftsRequest` + `alertRemovedShifts()`
  - 使用`REMOVED_SHIFT_SMS_TEMPLATE`模板

- **班次变更提醒**：
  - 通过`AlertChangedShiftRequest`传递新旧班次对比
  - 调用`alertChangedShift()`方法
  - 使用`CHANGED_SHIFT_SMS_TEMPLATE`模板

### 3. 问候消息
- 通过`GreetingRequest`指定目标用户
- 调用`sendGreeting()`方法发送基础问候
- 可扩展使用`GREETING_SMS_TEMPLATE`模板

所有通信均返回标准化`BaseResponse`，时间相关处理均遵循`BotConstant.DATETIME_FORMAT`规范，提醒功能默认覆盖未来10天内的班次（通过`DAY_ON_BOARDING_WINDOW`控制）。


### 包内部结构视图

```mermaid
graph TD
    bot --> client
    bot --> dto
    bot --> BotConstant.java
    client --> BotClient.java
    dto --> AlertNewShiftsRequest.java
    dto --> OnboardWorkerRequest.java
    dto --> AlertRemovedShiftsRequest.java
    dto --> AlertRemovedShiftRequest.java
    dto --> AlertNewShiftRequest.java
    dto --> AlertChangedShiftRequest.java
    dto --> GreetingRequest.java
```

该流程图展示了bot模块的代码结构，顶层目录包含client子目录、dto子目录和BotConstant.java文件。client目录下仅包含BotClient.java实现类，而dto目录下则包含7个不同的请求数据传输对象类文件，涉及班次提醒、入职问候等业务场景。所有节点均采用末端路径命名，层级关系清晰。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [client](client/_module.md) | package | Feign客户端接口，提供短信问候、员工入职、班次变动提醒等功能。 |
| [BotConstant.java](BotConstant.md) | file | 机器人服务常量类，包含短信和邮件模板代码及内容，用于员工排班通知。 |
| [dto](dto/_module.md) | package | 定义多个Java类，用于处理班次提醒、员工入职等请求，使用Lombok简化代码，包含非空字段验证。 |


