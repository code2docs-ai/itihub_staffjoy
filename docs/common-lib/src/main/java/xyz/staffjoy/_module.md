# 基础信息

|      |      |
|------|------|
| 名称 | staffjoy |
| 编码语言 | .java |
| 代码路径 | staffjoy/common-lib/src/main/java/xyz/staffjoy |
| 包名 | staffjoy.docs.common-lib.src.main.java.xyz.staffjoy |
| 概述说明 | 代码模块管理应用运行环境配置，区分开发、测试、生产环境并提供相应参数。包含EnvConstant和EnvConfig类。 |

# 说明

# Staffjoy Common Lib 模块总结

## 概述
Staffjoy Common Lib 是一个Java基础库模块，为Staffjoy项目提供了一系列通用的功能组件和工具类。该模块采用Spring框架构建，包含了环境管理、安全认证、异常处理、数据验证、日志审计等核心功能，为微服务架构提供了统一的基础设施支持。模块采用分层设计，各子模块职责明确，通过标准化接口和注解驱动的方式实现功能扩展。

## 主要业务场景

### 1. 环境管理与配置
- **多环境支持**：通过`EnvConstant`和`EnvConfig`管理开发、测试、UAT和生产环境配置
- **全局配置**：`StaffjoyConfig`统一初始化对象映射、环境配置和错误监控组件
- **属性管理**：`StaffjoyProps`绑定以"staffjoy.common"为前缀的配置属性

### 2. 安全认证与授权
- **身份验证**：`AuthContext`处理HTTP请求头中的用户身份信息
- **权限控制**：`AuthorizeInterceptor`实现基于`@Authorize`注解的细粒度权限拦截
- **会话管理**：`Sessions`类提供用户登录和令牌生成功能
- **加密签名**：`Sign`类处理JWT令牌生成验证，`Hash`类提供HmacSHA256消息认证

### 3. 异常处理与监控
- **自定义异常**：`ServiceException`封装业务错误，优化堆栈性能
- **全局拦截**：`GlobalExceptionTranslator`捕获多种异常并返回标准化响应
- **错误监控**：集成Sentry系统，`SentryClientAspect`实现调试模式拦截

### 4. 数据验证与处理
- **格式验证**：提供电话号码、时区、星期名称等验证器及对应注解
- **分组校验**：通过`Group1`和`Group2`接口支持分场景验证规则
- **HTTP响应**：`ResultCode`和`BaseResponse`标准化HTTP状态码和响应结构

### 5. 服务管理与工具
- **服务目录**：`ServiceDirectory`维护9个核心服务的静态映射表
- **安全级别**：`SecurityConstant`定义公开/认证/管理员三级访问控制
- **异步处理**：`ContextCopyingDecorator`实现多线程请求上下文复制
- **头像生成**：通过Gravatar服务根据邮箱自动生成用户头像

### 6. 日志与审计
- **审计日志**：`LogEntry`结构化记录用户操作变更详情
- **日志切面**：`SentryClientAspect`实现方法调用的调试日志记录

该模块作为基础架构层，为Staffjoy项目提供了标准化、可复用的解决方案，确保各微服务在环境管理、安全认证、异常处理等方面的一致性，同时通过灵活的配置和注解机制支持业务定制需求。


### 包内部结构视图

```mermaid
graph TD
    staffjoy --> common
    common --> env
    common --> async
    common --> aop
    common --> auditlog
    common --> validation
    common --> api
    common --> config
    common --> error
    common --> services
    common --> auth
    common --> utils
    common --> crypto
    env --> EnvConstant.java
    env --> EnvConfig.java
    async --> ContextCopyingDecorator.java
    aop --> SentryClientAspect.java
    auditlog --> LogEntry.java
    validation --> PhoneNumberValidator.java
    validation --> Group2.java
    validation --> TimezoneValidator.java
    validation --> DayOfWeekValidator.java
    validation --> PhoneNumber.java
    validation --> Timezone.java
    validation --> Group1.java
    validation --> DayOfWeek.java
    api --> ResultCode.java
    api --> BaseResponse.java
    config --> StaffjoyConfig.java
    config --> StaffjoyWebConfig.java
    config --> StaffjoyProps.java
    config --> StaffjoyRestConfig.java
    error --> ServiceException.java
    error --> GlobalExceptionTranslator.java
    services --> SecurityConstant.java
    services --> Service.java
    services --> ServiceDirectory.java
    auth --> AuthContext.java
    auth --> AuthorizeInterceptor.java
    auth --> AuthConstant.java
    auth --> Sessions.java
    auth --> PermissionDeniedException.java
    auth --> Authorize.java
    auth --> FeignRequestHeaderInterceptor.java
    utils --> Helper.java
    utils --> MD5Util.java
    crypto --> Sign.java
    crypto --> Hash.java
```

该流程图展示了staffjoy/common-lib项目的模块结构，以common为根节点，向下展开12个主要子模块（env/async/aop等），每个子模块包含对应的实现类文件。其中validation模块包含最多实现类（9个），其次是config和auth模块（各5个）。整体结构清晰展现了通用库的功能划分，涵盖环境配置、异步处理、AOP、验证、API基础等常见功能组件。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [common](common/_module.md) | package | 代码模块管理应用运行环境配置，区分开发、测试、生产环境并提供相应参数。包含EnvConstant和EnvConfig类。 |


