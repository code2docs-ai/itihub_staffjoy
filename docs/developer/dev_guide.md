[回到首页](../../README.md)

# 项目开发指南

## 项目概览
### 项目基本信息
- **名称:** [待填充]
- **GroupId:** [待填充 - Maven项目]
- **ArtifactId:** [待填充 - Maven项目]
- **Version:** [待填充]
- **构建工具:** [Maven/Gradle]
- **主要编程语言:** Java/Kotlin/Scala
- **核心依赖:** 
  - [依赖1]: [用途]
  - [依赖2]: [用途]

## 先决条件
- **JDK 版本:** 1.8+/11+/17+ (根据项目要求)
- **构建工具版本:** 
  - Maven: 3.6.3+
  - Gradle: 7.x+/8.x+
- **其他依赖:** [如数据库驱动、消息中间件等]

## 构建指南
### Maven 构建
- 构建命令:
  - 清理构建: `mvn clean`
  - 编译项目: `mvn compile`
  - 运行测试: `mvn test`
  - 打包项目: `mvn package`
  - 安装到本地仓库: `mvn install`
- 打包目录: `target/` 目录下生成:
  - `project-name-version.jar` (普通JAR)
  - `project-name-version.war` (Web应用)

### Gradle 构建
- 构建命令:
  - 清理构建: `./gradlew clean`
  - 编译项目: `./gradlew compileJava`
  - 运行测试: `./gradlew test`
  - 打包项目: `./gradlew jar` 或 `./gradlew bootJar`
- 打包目录: `build/libs/` 目录下生成:
  - `project-name-version.jar` (普通JAR)
  - `project-name-version-plain.jar` (非可执行JAR)

## 依赖管理
### 主要依赖
- [按功能分类列出主要依赖]

### 添加/修改依赖
- **Maven:** 编辑`pom.xml`的`<dependencies>`部分

## 工程结构

```
staffjoy/
├── pom.xml                  # Maven项目配置文件
├── LICENSE                  # 开源许可证文件
├── docker-compose.yml       # Docker容器编排配置
├── .gitignore               # Git忽略规则
├── .env.example             # 环境变量示例文件
├── README.md                # 项目说明文档
├── sms-api/                 # 短信服务API接口定义
│   ├── pom.xml
│   └── src/main/java/xyz/staffjoy/sms/
│       ├── SmsConstant.java
│       ├── client/SmsClient.java
│       └── dto/SmsRequest.java
├── doc/                     # 项目文档
│   ├── syllabus.md
│   ├── reference.md
│   ├── ppts/                # 演示文稿
│   └── images/              # 文档图片资源
├── sms-svc/                 # 短信微服务实现
│   ├── pom.xml
│   ├── Dockerfile
│   └── src/main/java/xyz/staffjoy/sms/
│       ├── SmsApplication.java
│       ├── controller/SmsController.java
│       ├── service/SmsSendService.java
│       └── config/AppConfig.java
├── frontend/                # 前端项目
│   ├── myaccount/           # 用户账户SPA（React应用）
│   │   ├── src/
│   │   │   ├── actions/     # Redux动作
│   │   │   ├── reducers/    # Redux状态管理
│   │   │   └── components/  # React组件
│   ├── app/                 # 主应用SPA
│   └── resources/           # 共享资源
├── account-svc/             # 账户微服务
│   ├── src/main/java/xyz/staffjoy/account/
│   │   ├── model/Account.java
│   │   ├── repo/AccountRepo.java
│   │   └── service/AccountService.java
├── account-api/             # 账户服务API定义
├── k8s/                     # Kubernetes部署配置
│   ├── uat/                 # 用户验收环境配置
│   ├── test/                # 测试环境配置
│   └── prod/                # 生产环境配置
├── company-svc/             # 公司服务微服务
│   └── src/main/java/xyz/staffjoy/company/
│       ├── model/Team.java
│       ├── service/TeamService.java
│       └── repo/TeamRepo.java
├── mail-api/                # 邮件服务API定义
├── mail-svc/                # 邮件微服务实现
├── common-lib/              # 公共库
│   └── src/main/java/xyz/staffjoy/common/
│       ├── auth/            # 认证相关
│       ├── error/           # 异常处理
│       └── validation/      # 验证工具
├── bot-svc/                 # 聊天机器人服务
├── config/                  # 全局配置文件
├── web-app/                 # Web应用（Spring Boot）
│   ├── src/main/java/xyz/staffjoy/web/
│   │   ├── controller/      # MVC控制器
│   │   └── view/            # 视图模板
│   └── src/main/resources/templates/
├── ical-svc/                # 日历服务
├── whoami-api/              # 用户身份API
├── whoami-svc/              # 用户身份服务
├── faraday/                 # API网关服务
│   ├── src/main/java/xyz/staffjoy/faraday/
│   │   ├── core/http/       # HTTP代理逻辑
│   │   └── config/          # 路由配置
└── company-api/             # 公司服务API定义
```

命名规约：
1. 服务模块采用`-svc`后缀表示微服务实现，`-api`后缀表示服务接口定义
2. 前端SPA采用功能命名（如myaccount/app）
3. 配置文件使用环境后缀（dev/test/prod）

分层结构：
1. 微服务架构：每个业务域独立服务（account/company/sms等）
2. 前后端分离：frontend包含React SPA，web-app处理服务端渲染
3. 接口与实现分离：*-api定义protobuf/DTO，*-svc包含业务逻辑

扩展设计：
1. 通过common-lib实现跨服务共享代码
2. k8s目录支持多环境部署
3. faraday网关实现统一入口和路由转发
4. 配置中心化设计（config目录）