# 基础信息

|      |      |
|------|------|
| 名称 | SmsSendService |
| 编码语言 | .java |
| 代码路径 | staffjoy/sms-svc/src/main/java/xyz/staffjoy/sms/service/SmsSendService.java |
| 包名 | xyz.staffjoy.sms.service |
| 依赖项 | ['com.aliyuncs.IAcsClient', 'com.aliyuncs.dysmsapi.model.v20170525.SendSmsRequest', 'com.aliyuncs.dysmsapi.model.v20170525.SendSmsResponse', 'com.aliyuncs.exceptions.ClientException', 'com.github.structlog4j.ILogger', 'com.github.structlog4j.SLoggerFactory', 'io.sentry.SentryClient', 'io.sentry.context.Context', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.scheduling.annotation.Async', 'org.springframework.stereotype.Service', 'xyz.staffjoy.sms.config.AppConfig', 'xyz.staffjoy.sms.props.AppProps', 'xyz.staffjoy.sms.dto.SmsRequest'] |
| 概述说明 | 短信服务类，异步发送短信，记录日志和异常。 |

# 说明

SmsSendService是一个异步发送短信的服务类，使用阿里云短信服务。通过Autowired注入AppProps、IAcsClient和SentryClient依赖。sendSmsAsync方法接收SmsRequest参数，设置接收号码、签名、模板代码和参数后调用阿里云接口发送短信。成功时记录日志，失败时通过Sentry记录错误信息并记录错误日志。处理过程中捕获ClientException异常并记录。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| SmsSendService | class | 异步发送短信服务，记录成功或失败日志并上报异常。 |



## 类 SmsSendService

|      |      |
|------|------|
| 访问范围 | @Service;public |
| 类型 | class |
| 名称 | SmsSendService |
| 说明 | 异步发送短信服务，记录成功或失败日志并上报异常。 |


### UML类图

```mermaid
classDiagram
    class SmsSendService {
        -static final ILogger logger
        -AppProps appProps
        -IAcsClient acsClient
        -SentryClient sentryClient
        +sendSmsAsync(SmsRequest smsRequest) void
    }

    class AppProps {
        +getAliyunSmsSignName() String
    }

    class IAcsClient {
        <<Interface>>
        +getAcsResponse(SendSmsRequest request) SendSmsResponse
    }

    class SentryClient {
        +getContext() Context
        +sendMessage(String message) void
        +sendException(Exception ex) void
    }

    class SmsRequest {
        +getTo() String
        +getTemplateCode() String
        +getTemplateParam() String
    }

    class SendSmsRequest {
        +setPhoneNumbers(String numbers) void
        +setSignName(String name) void
        +setTemplateCode(String code) void
        +setTemplateParam(String param) void
    }

    class SendSmsResponse {
        +getCode() String
        +getRequestId() String
    }

    class Context {
        +addTag(String key, String value) void
    }

    SmsSendService --> AppProps : 依赖
    SmsSendService --> IAcsClient : 依赖
    SmsSendService --> SentryClient : 依赖
    SmsSendService --> SmsRequest : 处理
    SmsSendService --> SendSmsRequest : 创建
    IAcsClient --> SendSmsResponse : 返回
    SentryClient --> Context : 使用
```

这段代码展示了一个短信发送服务(SmsSendService)的类结构，它通过阿里云短信服务(IAcsClient)异步发送短信，并使用Sentry进行错误监控。服务依赖AppProps获取配置，通过IAcsClient接口发送请求，并利用SentryClient记录异常。主要处理流程包括：构建短信请求(SendSmsRequest)、处理响应(SendSmsResponse)以及错误处理时记录上下文(Context)。整个设计体现了依赖注入和异步处理的特性。


### 内部方法调用关系图

```mermaid
graph TD
    A["SmsSendService"]
    B["属性: ILogger logger"]
    C["属性: AppProps appProps"]
    D["属性: IAcsClient acsClient"]
    E["属性: SentryClient sentryClient"]
    F["异步方法: sendSmsAsync(SmsRequest smsRequest)"]
    G["创建SendSmsRequest对象"]
    H["设置请求参数: phoneNumbers/signName/templateCode/templateParam"]
    I["调用acsClient.getAcsResponse"]
    J["判断响应码是否为'OK'"]
    K["记录成功日志"]
    L["Sentry记录错误信息"]
    M["记录失败日志"]
    N["捕获ClientException异常"]
    O["Sentry记录异常"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J -- 是 --> K
    J -- 否 --> L
    L --> M
    I -. 异常 .-> N
    N --> O
    O --> M
```

这段代码流程图展示了SmsSendService处理短信发送的完整流程。该服务通过异步方式发送短信请求，首先配置短信参数并调用阿里云短信接口，然后根据响应结果分别处理成功和失败情况。成功时记录业务日志，失败时通过Sentry收集错误信息并记录异常日志。整个流程包含请求构建、接口调用、响应处理和异常捕获四个主要阶段，体现了完整的错误监控和日志记录机制。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| acsClient | IAcsClient | 自动注入AcsClient实例 |
| appProps | AppProps | 自动注入AppProps配置类实例。 |
| sentryClient | SentryClient | 自动注入SentryClient实例。 |
| logger = SLoggerFactory.getLogger(SmsSendService.class) | ILogger | 静态日志记录器，用于SmsSendService类的日志输出。 |

### 方法列表 Method List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| sendSmsAsync | void | 异步发送短信，成功记录日志，失败上报错误。 |




