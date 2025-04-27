# 基础信息

|      |      |
|------|------|
| 名称 | MailSendService |
| 编码语言 | .java |
| 代码路径 | staffjoy/mail-svc/src/main/java/xyz/staffjoy/mail/service/MailSendService.java |
| 包名 | xyz.staffjoy.mail.service |
| 依赖项 | ['com.aliyuncs.IAcsClient', 'com.aliyuncs.dm.model.v20151123.SingleSendMailRequest', 'com.aliyuncs.dm.model.v20151123.SingleSendMailResponse', 'com.aliyuncs.exceptions.ClientException', 'com.github.structlog4j.ILogger', 'com.github.structlog4j.IToLog', 'com.github.structlog4j.SLoggerFactory', 'io.sentry.SentryClient', 'io.sentry.context.Context', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.scheduling.annotation.Async', 'org.springframework.stereotype.Service', 'xyz.staffjoy.common.env.EnvConfig', 'xyz.staffjoy.common.env.EnvConstant', 'xyz.staffjoy.mail.MailConstant', 'xyz.staffjoy.mail.config.AppConfig', 'xyz.staffjoy.mail.dto.EmailRequest'] |
| 概述说明 | 邮件发送服务类，支持异步发送，非生产环境仅限内部邮箱，记录日志和异常。 |

# 说明

MailSendService是一个异步邮件发送服务类，使用@Async注解实现异步处理。它通过IAcsClient调用阿里云邮件服务API发送邮件。在非生产环境（dev/uat）中会拦截非jskillcloud.com后缀的收件地址，并自动在邮件主题前添加环境标识。发送过程会记录详细日志，包括邮件主题、收件人和HTML内容。若发送失败，会将异常信息上报至Sentry监控系统并记录错误日志。服务依赖EnvConfig获取环境配置，使用SentryClient进行异常监控。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| MailSendService | class | 邮件发送服务类，支持异步发送，非生产环境仅限特定域名，含日志和异常处理。 |



## 类 MailSendService

|      |      |
|------|------|
| 访问范围 | @Service;public |
| 类型 | class |
| 名称 | MailSendService |
| 说明 | 邮件发送服务类，支持异步发送，非生产环境仅限特定域名，含日志和异常处理。 |


### UML类图

```mermaid
classDiagram
    class MailSendService {
        -ILogger logger
        -EnvConfig envConfig
        -IAcsClient acsClient
        -SentryClient sentryClient
        +sendMailAsync(EmailRequest req) void
    }

    class EnvConfig {
        <<Interface>>
        +String getName() String
    }

    class IAcsClient {
        <<Interface>>
        +SingleSendMailResponse getAcsResponse(SingleSendMailRequest req) SingleSendMailResponse
    }

    class SentryClient {
        <<Interface>>
        +Context getContext() Context
        +void sendException(Exception ex)
    }

    class EmailRequest {
        +String getSubject() String
        +String getTo() String
        +String getHtmlBody() String
        +void setSubject(String subject)
    }

    class SingleSendMailRequest {
        +void setAccountName(String name)
        +void setFromAlias(String alias)
        +void setAddressType(int type)
        +void setToAddress(String address)
        +void setReplyToAddress(boolean flag)
        +void setSubject(String subject)
        +void setHtmlBody(String body)
    }

    class SingleSendMailResponse {
        +String getRequestId() String
    }

    class ClientException {
        <<Exception>>
    }

    class Context {
        +void addTag(String key, String value)
    }

    MailSendService --> EnvConfig : 依赖
    MailSendService --> IAcsClient : 依赖
    MailSendService --> SentryClient : 依赖
    MailSendService --> EmailRequest : 使用
    IAcsClient --> SingleSendMailRequest : 参数
    IAcsClient --> SingleSendMailResponse : 返回
    SentryClient --> Context : 创建
    SentryClient --> ClientException : 处理
```

这段代码展示了一个邮件发送服务类MailSendService，它通过异步方式发送邮件。该类依赖EnvConfig获取环境配置，使用IAcsClient与邮件服务API交互，并通过SentryClient记录异常。核心方法sendMailAsync会校验非生产环境只允许发送到特定域名，构造邮件请求对象后调用阿里云邮件服务，成功时记录日志，失败时通过Sentry上报异常。整个设计体现了环境隔离、异步处理和错误监控等关键特性。


### 内部方法调用关系图

```mermaid
graph TD
    A["MailSendService类"]
    B["属性: ILogger logger"]
    C["属性: EnvConfig envConfig"]
    D["属性: IAcsClient acsClient"]
    E["属性: SentryClient sentryClient"]
    F["异步方法: sendMailAsync(EmailRequest req)"]
    G["创建日志上下文: IToLog logContext"]
    H["环境检查: !EnvConstant.ENV_PROD"]
    I["修改邮件主题: prepend env"]
    J["拦截非生产环境邮件: !endsWith('@jskillcloud.com')"]
    K["构建邮件请求: SingleSendMailRequest"]
    L["发送邮件: acsClient.getAcsResponse"]
    M["成功处理: 记录日志"]
    N["异常处理: ClientException"]
    O["Sentry异常上报: sentryClient.sendException"]
    P["错误日志记录: logger.error"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    F --> G
    F --> H
    H -->|是| I
    H -->|是| J
    J -->|拦截| P
    H -->|否| K
    K --> L
    L --> M
    L --> N
    N --> O
    N --> P
```

这段流程图展示了MailSendService类的核心邮件发送流程。首先初始化日志上下文，然后进行环境检查，非生产环境会修改主题并拦截非公司域名的邮件。正常流程会构建邮件请求并通过阿里云客户端发送，成功则记录日志，失败则通过Sentry上报异常并记录错误日志。整个过程采用异步执行方式，关键步骤都包含完善的日志记录和异常处理机制。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| envConfig | EnvConfig | 自动注入EnvConfig配置实例 |
| acsClient | IAcsClient | 自动注入阿里云客户端实例 |
| logger = SLoggerFactory.getLogger(MailSendService.class) | ILogger | 私有日志记录器，用于邮件发送服务类。 |
| sentryClient | SentryClient | 自动注入SentryClient实例。 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| sendMailAsync | void | 异步发送邮件，非生产环境仅限特定域名，记录日志和异常。 |




