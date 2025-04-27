# 基础信息

|      |      |
|------|------|
| 名称 | ServiceHelper |
| 编码语言 | .java |
| 代码路径 | staffjoy/company-svc/src/main/java/xyz/staffjoy/company/service/helper/ServiceHelper.java |
| 包名 | xyz.staffjoy.company.service.helper |
| 依赖项 | ['com.github.structlog4j.ILogger', 'com.github.structlog4j.SLoggerFactory', 'io.sentry.SentryClient', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.scheduling.annotation.Async', 'org.springframework.stereotype.Component', 'org.springframework.util.StringUtils', 'xyz.staffjoy.account.client.AccountClient', 'xyz.staffjoy.account.dto.TrackEventRequest', 'xyz.staffjoy.bot.client.BotClient', 'xyz.staffjoy.bot.dto', 'xyz.staffjoy.common.api.BaseResponse', 'xyz.staffjoy.common.auth.AuthContext', 'xyz.staffjoy.common.env.EnvConfig', 'xyz.staffjoy.common.error.ServiceException', 'xyz.staffjoy.company.config.AppConfig', 'xyz.staffjoy.company.dto.ShiftDto', 'java.time.Instant', 'java.util.List', 'java.util.Map'] |
| 概述说明 | 服务助手类，提供异步事件跟踪、员工入职、班次通知等功能，处理错误并记录日志。 |

# 说明

ServiceHelper是一个Spring组件类，提供异步事件跟踪和通知功能。它包含多个异步方法，使用AccountClient和BotClient进行远程调用。主要功能包括跟踪用户事件、处理员工入职、发送新班次/取消班次/变更班次的提醒通知。所有方法都通过Async注解异步执行，统一使用handleErrorAndThrowException处理异常和错误响应，并在生产环境下通过SentryClient上报错误。类中还包含环境配置检查和日志记录功能，确保错误信息被妥善处理和记录。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| ServiceHelper | class | ServiceHelper类提供异步事件跟踪、工人入职和班次通知功能，处理错误并记录日志。 |



## 类 ServiceHelper

|      |      |
|------|------|
| 访问范围 | @SuppressWarnings("Duplicates");@Component;public |
| 类型 | class |
| 名称 | ServiceHelper |
| 说明 | ServiceHelper类提供异步事件跟踪、工人入职和班次通知功能，处理错误并记录日志。 |


### UML类图

```mermaid
classDiagram
    class ServiceHelper {
        -AccountClient accountClient
        -BotClient botClient
        -SentryClient sentryClient
        -EnvConfig envConfig
        +trackEventAsync(String event) void
        +onboardWorkerAsync(OnboardWorkerRequest onboardWorkerRequest) void
        +alertNewShiftAsync(AlertNewShiftRequest alertNewShiftRequest) void
        +alertRemovedShiftAsync(AlertRemovedShiftRequest alertRemovedShiftRequest) void
        +buildShiftNotificationAsync(Map~String, List~ShiftDto~~ notifs, boolean published) void
        +updateShiftNotificationAsync(ShiftDto origShiftDto, ShiftDto shiftDtoToUpdte) void
        +handleErrorAndThrowException(ILogger log, String errMsg) void
        +handleErrorAndThrowException(ILogger log, Exception ex, String errMsg) void
    }

    class AccountClient {
        <<Interface>>
        +trackEvent(TrackEventRequest trackEventRequest) BaseResponse
    }

    class BotClient {
        <<Interface>>
        +onboardWorker(OnboardWorkerRequest onboardWorkerRequest) BaseResponse
        +alertNewShift(AlertNewShiftRequest alertNewShiftRequest) BaseResponse
        +alertRemovedShift(AlertRemovedShiftRequest alertRemovedShiftRequest) BaseResponse
        +alertNewShifts(AlertNewShiftsRequest alertNewShiftsRequest) BaseResponse
        +alertRemovedShifts(AlertRemovedShiftsRequest alertRemovedShiftsRequest) BaseResponse
        +alertChangedShift(AlertChangedShiftRequest alertChangedShiftRequest) BaseResponse
    }

    class SentryClient {
        <<Interface>>
        +sendMessage(String message) void
        +sendException(Exception ex) void
    }

    class EnvConfig {
        -boolean debug
        +isDebug() boolean
    }

    class ILogger {
        <<Interface>>
        +error(String msg) void
        +error(String msg, Exception ex) void
    }

    ServiceHelper --> AccountClient : 依赖
    ServiceHelper --> BotClient : 依赖
    ServiceHelper --> SentryClient : 依赖
    ServiceHelper --> EnvConfig : 依赖
    ServiceHelper --> ILogger : 依赖
```

这段代码展示了一个名为`ServiceHelper`的服务类，它通过异步方法处理多种业务逻辑，包括事件跟踪、工人入职、班次通知等。该类依赖多个客户端接口（如`AccountClient`、`BotClient`）和配置类（`EnvConfig`），并通过`SentryClient`进行错误上报。所有异步方法都遵循相似的错误处理模式，使用`handleErrorAndThrowException`方法统一处理异常。类图清晰地展示了这些依赖关系和接口定义。


### 内部方法调用关系图

```mermaid
graph TD
    A["类ServiceHelper"]
    B["属性: AccountClient accountClient"]
    C["属性: BotClient botClient"]
    D["属性: SentryClient sentryClient"]
    E["属性: EnvConfig envConfig"]
    F["方法: trackEventAsync(String event)"]
    G["方法: onboardWorkerAsync(OnboardWorkerRequest)"]
    H["方法: alertNewShiftAsync(AlertNewShiftRequest)"]
    I["方法: alertRemovedShiftAsync(AlertRemovedShiftRequest)"]
    J["方法: buildShiftNotificationAsync(Map<String, List<ShiftDto>>, boolean)"]
    K["方法: updateShiftNotificationAsync(ShiftDto, ShiftDto)"]
    L["方法: handleErrorAndThrowException(ILogger, String)"]
    M["方法: handleErrorAndThrowException(ILogger, Exception, String)"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J
    A --> K
    A --> L
    A --> M

    F --> F1["获取userId"]
    F --> F2{"userId为空?"}
    F2 -->|是| F3["返回"]
    F2 -->|否| F4["构建TrackEventRequest"]
    F4 --> F5["调用accountClient.trackEvent"]
    F5 --> F6{"成功?"}
    F6 -->|是| F7["结束"]
    F6 -->|否| F8["调用handleErrorAndThrowException"]

    G --> G1["调用botClient.onboardWorker"]
    G1 --> G2{"成功?"}
    G2 -->|是| G3["结束"]
    G2 -->|否| G4["调用handleErrorAndThrowException"]

    H --> H1["调用botClient.alertNewShift"]
    H1 --> H2{"成功?"}
    H2 -->|是| H3["结束"]
    H2 -->|否| H4["调用handleErrorAndThrowException"]

    I --> I1["调用botClient.alertRemovedShift"]
    I1 --> I2{"成功?"}
    I2 -->|是| I3["结束"]
    I2 -->|否| I4["调用handleErrorAndThrowException"]

    J --> J1["遍历notifs"]
    J1 --> J2{"published为真?"}
    J2 -->|是| J3["构建AlertNewShiftsRequest"]
    J3 --> J4["调用botClient.alertNewShifts"]
    J4 --> J5{"成功?"}
    J5 -->|是| J6["结束"]
    J5 -->|否| J7["调用handleErrorAndThrowException"]
    J2 -->|否| J8["构建AlertRemovedShiftsRequest"]
    J8 --> J9["调用botClient.alertRemovedShifts"]
    J9 --> J10{"成功?"}
    J10 -->|是| J11["结束"]
    J10 -->|否| J12["调用handleErrorAndThrowException"]

    K --> K1{"origShiftDto未发布且shiftDtoToUpdte已发布?"}
    K1 -->|是| K2["构建AlertNewShiftRequest"]
    K2 --> K3["调用botClient.alertNewShift"]
    K3 --> K4{"成功?"}
    K4 -->|是| K5["结束"]
    K4 -->|否| K6["调用handleErrorAndThrowException"]
    K1 -->|否| K7{"origShiftDto已发布且shiftDtoToUpdte未发布?"}
    K7 -->|是| K8["构建AlertRemovedShiftRequest"]
    K8 --> K9["调用botClient.alertRemovedShift"]
    K9 --> K10{"成功?"}
    K10 -->|是| K11["结束"]
    K10 -->|否| K12["调用handleErrorAndThrowException"]
    K7 -->|否| K13{"origShiftDto和shiftDtoToUpdte都未发布?"}
    K13 -->|是| K14["返回"]
    K13 -->|否| K15{"origShiftDto和shiftDtoToUpdte的userId相同?"}
    K15 -->|是| K16["构建AlertChangedShiftRequest"]
    K16 --> K17["调用botClient.alertChangedShift"]
    K17 --> K18{"成功?"}
    K18 -->|是| K19["结束"]
    K18 -->|否| K20["调用handleErrorAndThrowException"]
    K15 -->|否| K21{"origShiftDto和shiftDtoToUpdte的userId不同?"}
    K21 -->|是| K22["构建AlertRemovedShiftRequest"]
    K22 --> K23["调用botClient.alertRemovedShift"]
    K23 --> K24{"成功?"}
    K24 -->|是| K25["结束"]
    K24 -->|否| K26["调用handleErrorAndThrowException"]
    K21 --> K27["构建AlertNewShiftRequest"]
    K27 --> K28["调用botClient.alertNewShift"]
    K28 --> K29{"成功?"}
    K29 -->|是| K30["结束"]
    K29 -->|否| K31["调用handleErrorAndThrowException"]

    L --> L1["记录错误日志"]
    L1 --> L2{"非调试模式?"}
    L2 -->|是| L3["发送错误信息到sentry"]
    L2 -->|否| L4["抛出ServiceException"]
    L3 --> L4

    M --> M1["记录错误日志"]
    M1 --> M2{"非调试模式?"}
    M2 -->|是| M3["发送异常到sentry"]
    M2 -->|否| M4["抛出ServiceException"]
    M3 --> M4
```

这段代码是一个Spring组件类ServiceHelper，主要功能是异步处理各种事件跟踪和通知任务。它包含多个异步方法，如trackEventAsync、onboardWorkerAsync、alertNewShiftAsync等，这些方法通过调用不同的客户端（如accountClient、botClient）来执行具体操作，并统一处理异常情况。类中还包含两个错误处理方法handleErrorAndThrowException，用于记录日志、发送错误信息到Sentry并抛出异常。整体设计采用异步处理模式，通过@Async注解实现，提高了系统的响应速度和吞吐量。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| accountClient | AccountClient | 自动注入AccountClient实例 |
| sentryClient | SentryClient | 自动注入SentryClient实例。 |
| envConfig | EnvConfig | 自动注入EnvConfig配置实例 |
| logger = SLoggerFactory.getLogger(ServiceHelper.class) | ILogger | 静态日志记录器实例，用于ServiceHelper类。 |
| botClient | BotClient | 自动注入BotClient实例。 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| alertRemovedShiftAsync | void | 异步处理移除班次提醒，调用外部接口并处理异常和失败响应。 |
| trackEventAsync | void | 异步方法：验证用户ID后发送事件跟踪请求，失败则报错。 |
| alertNewShiftAsync | void | 异步通知新班次，处理异常和失败响应。 |
| onboardWorkerAsync | void | 异步处理员工入职请求，调用botClient并处理异常和失败响应。 |
| buildShiftNotificationAsync | void | 异步通知用户班次变更，发布或移除时调用不同接口处理异常和响应。 |
| updateShiftNotificationAsync | void | 异步处理班次变更通知，包括新增、删除和修改，失败时抛出异常。 |
| handleErrorAndThrowException | void | 处理错误并抛出异常：记录错误，非调试时发送至Sentry，抛出ServiceException。 |
| handleErrorAndThrowException | void | 处理错误并抛出异常：记录日志，非调试时发送至Sentry，抛出ServiceException。 |




