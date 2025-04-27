# 基础信息

|      |      |
|------|------|
| 名称 | ICalService |
| 编码语言 | .java |
| 代码路径 | staffjoy/ical-svc/src/main/java/xyz/staffjoy/ical/service/ICalService.java |
| 包名 | xyz.staffjoy.ical.service |
| 依赖项 | ['com.github.structlog4j.ILogger', 'com.github.structlog4j.SLoggerFactory', 'io.sentry.SentryClient', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.stereotype.Service', 'xyz.staffjoy.common.auth.AuthConstant', 'xyz.staffjoy.common.env.EnvConfig', 'xyz.staffjoy.common.error.ServiceException', 'xyz.staffjoy.company.client.CompanyClient', 'xyz.staffjoy.company.dto', 'xyz.staffjoy.ical.model.Cal', 'java.time.Instant', 'java.time.temporal.ChronoUnit'] |
| 概述说明 | ICalService类通过公司客户端获取用户团队、公司及班次信息，构建Cal对象返回。异常时记录日志并抛出。 |

# 说明

ICalService是一个Spring服务类，用于获取用户相关的日历信息。它通过CompanyClient获取用户团队信息、公司信息和排班列表，并构建包含公司名称和排班列表的Cal对象。过程中会捕获异常并记录日志，通过SentryClient上报错误，最后抛出ServiceException。服务依赖EnvConfig配置，使用静态日志记录器。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| ICalService | class | ICalService通过公司客户端获取用户团队、公司及班次信息，构建Cal对象返回。异常时记录日志并抛出。 |



## 类 ICalService

|      |      |
|------|------|
| 访问范围 | @Service;public |
| 类型 | class |
| 名称 | ICalService |
| 说明 | ICalService通过公司客户端获取用户团队、公司及班次信息，构建Cal对象返回。异常时记录日志并抛出。 |


### UML类图

```mermaid
classDiagram
    class ICalService {
        -CompanyClient companyClient
        -SentryClient sentryClient
        -EnvConfig envConfig
        +Cal getCalByUserId(String userId)
        -void handleErrorAndThrowException(String errMsg)
        -void handleErrorAndThrowException(Exception ex, String errMsg)
    }

    class CompanyClient {
        <<Interface>>
        +GenericWorkerResponse getWorkerTeamInfo(String auth, Object unused, String userId)
        +GenericCompanyResponse getCompany(String auth, String companyId)
        +GenericShiftListResponse listWorkerShifts(String auth, WorkerShiftListRequest request)
    }

    class SentryClient {
        <<Interface>>
        +void sendMessage(String msg)
        +void sendException(Exception ex)
    }

    class EnvConfig {
        // 环境配置类
    }

    class WorkerShiftListRequest {
        +String companyId
        +String teamId
        +String workerId
        +Instant shiftStartAfter
        +Instant shiftStartBefore
        +static Builder builder()
    }

    class Cal {
        +String companyName
        +List~Shift~ shiftList
        +static Builder builder()
    }

    ICalService --> CompanyClient : 依赖\n调用公司服务API
    ICalService --> SentryClient : 依赖\n错误日志上报
    ICalService --> EnvConfig : 依赖\n读取环境配置
    ICalService --> WorkerShiftListRequest : 创建\n构建查询请求
    ICalService --> Cal : 创建\n构建返回结果
```

该类图展示了ICalService的核心结构和依赖关系。ICalService通过CompanyClient获取员工团队信息、公司数据和排班列表，使用SentryClient进行错误上报，最终构建包含公司名称和排班列表的Cal对象返回。图中清晰体现了服务层与外部接口的交互方式，包括WorkerShiftListRequest请求对象的构建和Cal结果对象的组装过程，以及统一的异常处理机制。


### 内部方法调用关系图

```mermaid
graph TD
    A["ICalService类"]
    B["属性: CompanyClient companyClient"]
    C["属性: SentryClient sentryClient"]
    D["属性: EnvConfig envConfig"]
    E["方法: getCalByUserId(String userId)"]
    F["方法: handleErrorAndThrowException(String errMsg)"]
    G["方法: handleErrorAndThrowException(Exception ex, String errMsg)"]
    
    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    
    E --> H["调用companyClient.getWorkerTeamInfo"]
    H --> I{"是否成功?"}
    I -->|是| J["获取WorkerDto"]
    I -->|否| K["调用handleErrorAndThrowException"]
    J --> L["调用companyClient.getCompany"]
    L --> M{"是否成功?"}
    M -->|是| N["获取CompanyDto"]
    M -->|否| O["调用handleErrorAndThrowException"]
    N --> P["构建WorkerShiftListRequest"]
    P --> Q["调用companyClient.listWorkerShifts"]
    Q --> R{"是否成功?"}
    R -->|是| S["获取ShiftList"]
    R -->|否| T["调用handleErrorAndThrowException"]
    S --> U["构建并返回Cal对象"]
```

```mermaid
sequenceDiagram
    participant Client
    participant ICalService
    participant CompanyClient
    participant SentryClient
    
    Client->>ICalService: getCalByUserId(userId)
    ICalService->>CompanyClient: getWorkerTeamInfo(auth, null, userId)
    alt 调用成功
        CompanyClient-->>ICalService: workerResponse
        ICalService->>CompanyClient: getCompany(auth, companyId)
        alt 调用成功
            CompanyClient-->>ICalService: genericCompanyResponse
            ICalService->>CompanyClient: listWorkerShifts(auth, request)
            alt 调用成功
                CompanyClient-->>ICalService: shiftListResponse
                ICalService-->>Client: 返回Cal对象
            else 调用失败
                ICalService->>SentryClient: sendException(ex)
                ICalService--x Client: 抛出ServiceException
            end
        else 调用失败
            ICalService->>SentryClient: sendMessage(errMsg)
            ICalService--x Client: 抛出ServiceException
        end
    else 调用失败
        ICalService->>SentryClient: sendException(ex)
        ICalService--x Client: 抛出ServiceException
    end
```

这段代码是ICalService类的实现，主要功能是通过用户ID获取日历信息。流程包括：1)获取员工团队信息；2)获取公司信息；3)获取员工排班列表；4)构建日历对象返回。整个过程有完善的错误处理机制，任何步骤失败都会记录日志并抛出异常。时序图展示了客户端调用服务、服务调用公司客户端、错误处理等完整交互流程。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| logger = SLoggerFactory.getLogger(ICalService.class) | ILogger | 静态日志记录器实例，用于ICalService类。 |
| companyClient | CompanyClient | 自动注入公司客户端实例 |
| envConfig | EnvConfig | 自动注入环境配置对象envConfig。 |
| sentryClient | SentryClient | 自动注入SentryClient实例。 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| getCalByUserId | Cal | 通过用户ID获取团队、公司和排班信息，构建并返回日历对象。 |
| handleErrorAndThrowException | void | 处理错误并抛出异常：记录日志、发送至Sentry、抛出ServiceException。 |
| handleErrorAndThrowException | void | 处理错误并抛出异常：记录日志、发送至Sentry、抛出服务异常。 |




