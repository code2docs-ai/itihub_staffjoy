# 基础信息

|      |      |
|------|------|
| 名称 | dto |
| 编码语言 | .java |
| 代码路径 | staffjoy/company-api/src/main/java/xyz/staffjoy/company/dto |
| 包名 | staffjoy.docs.company-api.src.main.java.xyz.staffjoy.company.dto |
| 概述说明 | Java数据传输对象类集合，包含公司、团队、职位、班次等管理相关DTO，使用Lombok简化代码，支持建造者模式。 |

# 说明

## 概述
该代码模块是StaffJoy公司服务中的company-api模块的数据传输对象(DTO)部分，主要用于处理与公司、团队、员工、班次和职位相关的数据传输。模块采用Java语言开发，大量使用Lombok注解简化代码，包含响应对象、请求对象和列表数据结构等多种DTO类型。所有响应类均继承自BaseResponse基类，保持统一的响应结构。

## 主要业务场景
1. **公司管理**：处理公司信息的创建、查询和列表展示，包含CompanyDto、CompanyList等数据结构。
2. **团队管理**：处理团队信息的创建、查询和列表展示，包含TeamDto、TeamList及相关请求响应对象。
3. **员工目录管理**：处理员工信息的存储和查询，包含DirectoryEntryDto、DirectoryList等数据结构。
4. **班次管理**：处理班次的创建、查询、批量发布和列表展示，包含ShiftDto、ShiftList及相关请求对象。
5. **职位管理**：处理职位信息的创建、查询和列表展示，包含JobDto、JobList等数据结构。
6. **关联关系管理**：处理员工与团队/公司的关联关系，包含Association、WorkerOfList等数据结构。
7. **管理员管理**：处理公司管理员信息的查询和列表展示，包含AdminOfList、AdminEntries等数据结构。
8. **统计报表**：处理排班统计数据的展示，如GrowthGraphResponse中的排班人数统计。

模块中的DTO类普遍采用建造者模式，支持分页查询(limit/offset)，并包含严格的数据校验逻辑(如非空检查、格式验证等)，确保数据传输的有效性和安全性。


### 包内部结构视图

```mermaid
graph TD
    dto --> GetAdminOfResponse.java
    dto --> DirectoryList.java
    dto --> ListTeamResponse.java
    dto --> CompanyList.java
    dto --> ListCompanyResponse.java
    dto --> TeamList.java
    dto --> Association.java
    dto --> GenericCompanyResponse.java
    dto --> AdminOfList.java
    dto --> ListWorkerResponse.java
    dto --> GenericShiftResponse.java
    dto --> BulkPublishShiftsRequest.java
    dto --> GenericShiftListResponse.java
    dto --> DirectoryEntryDto.java
    dto --> CompanyDto.java
    dto --> TimeZoneList.java
    dto --> ScheduledPerWeek.java
    dto --> JobList.java
    dto --> ListDirectoryResponse.java
    dto --> JobDto.java
    dto --> ShiftListRequest.java
    dto --> CreateShiftRequest.java
    dto --> GetWorkerOfResponse.java
    dto --> AdminEntries.java
    dto --> ShiftList.java
    dto --> WorkerDto.java
    dto --> CreateJobRequest.java
    dto --> NewDirectoryEntry.java
    dto --> GetAssociationResponse.java
    dto --> ListAdminResponse.java
    dto --> GenericJobResponse.java
    dto --> GenericWorkerResponse.java
    dto --> CreateTeamRequest.java
    dto --> GenericTeamResponse.java
    dto --> WorkerShiftListRequest.java
    dto --> TeamDto.java
    dto --> ListJobResponse.java
    dto --> WorkerOfList.java
    dto --> GenericDirectoryResponse.java
    dto --> ShiftDto.java
    dto --> AssociationList.java
    dto --> GrowthGraphResponse.java
    dto --> DirectoryEntryRequest.java
    dto --> WorkerEntries.java
```

该流程图展示了company-api模块中dto目录下的所有文件结构。dto作为根节点，包含40个直接子节点，均为不同类型的DTO类文件，涵盖公司、团队、员工、班次等业务实体的数据传输对象定义。这些文件共同构成了Staffjoy项目的核心数据传输层结构。

# 文件列表 File List

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [ListCompanyResponse.java](ListCompanyResponse.md) | file | Java类ListCompanyResponse继承BaseResponse，包含公司列表属性及常用注解。 |
| [CompanyList.java](CompanyList.md) | file | 公司列表类，包含公司集合、限制和偏移量。 |
| [ListTeamResponse.java](ListTeamResponse.md) | file | Java类ListTeamResponse继承BaseResponse，包含TeamList属性，使用Lombok注解生成getter/setter等方法。 |
| [DirectoryList.java](DirectoryList.md) | file | 类DirectoryList包含账户列表、限制和偏移量字段，支持无参、全参和构建器构造。 |
| [GetAdminOfResponse.java](GetAdminOfResponse.md) | file | Java类GetAdminOfResponse继承BaseResponse，包含AdminOfList属性，提供Getter/Setter和构造方法。 |
| [TimeZoneList.java](TimeZoneList.md) | file | Java类TimeZoneList，包含时区列表，支持构造和构建。 |
| [CompanyDto.java](CompanyDto.md) | file | 公司DTO类，含ID、名称、时区等字段，支持分组校验。 |
| [GenericShiftListResponse.java](GenericShiftListResponse.md) | file | Java类GenericShiftListResponse继承BaseResponse，包含ShiftList字段，提供Getter/Setter、全参/无参构造方法，重写toString和equals/hashCode。 |
| [BulkPublishShiftsRequest.java](BulkPublishShiftsRequest.md) | file | 批量发布班次请求类，含公司、团队、用户、岗位ID及班次时间范围，支持构造器与空参。 |
| [GenericShiftResponse.java](GenericShiftResponse.md) | file | Java类GenericShiftResponse继承BaseResponse，包含ShiftDto字段及常用注解。 |
| [ListWorkerResponse.java](ListWorkerResponse.md) | file | Java类ListWorkerResponse继承BaseResponse，包含WorkerEntries字段，提供Getter/Setter和构造方法。 |
| [AdminOfList.java](AdminOfList.md) | file | Java类AdminOfList，含userId和companies字段，支持无参、全参构造和Builder模式。 |
| [GenericCompanyResponse.java](GenericCompanyResponse.md) | file | Java类GenericCompanyResponse继承BaseResponse，包含CompanyDto属性，使用Lombok注解简化代码。 |
| [Association.java](Association.md) | file | Association类包含账户、团队列表和管理员状态，支持构建器模式。 |
| [CreateJobRequest.java](CreateJobRequest.md) | file | 创建任务请求类，含公司ID、团队ID、名称和颜色字段，非空校验。 |
| [WorkerDto.java](WorkerDto.md) | file | WorkerDto类包含公司、团队和用户ID字段，使用Lombok注解简化代码。 |
| [ShiftList.java](ShiftList.md) | file | ShiftList类包含班次列表和起止时间。 |
| [GetWorkerOfResponse.java](GetWorkerOfResponse.md) | file | Java类GetWorkerOfResponse继承BaseResponse，包含WorkerOfList字段，使用Lombok注解简化代码。 |
| [CreateShiftRequest.java](CreateShiftRequest.md) | file | 创建班次请求类，含公司、团队ID，起止时间，用户和岗位ID，校验时间有效性和最大时长。 |
| [ShiftListRequest.java](ShiftListRequest.md) | file | Java类ShiftListRequest，含公司、团队、用户、岗位ID及班次起止时间，验证时间逻辑。 |
| [JobDto.java](JobDto.md) | file | JobDto类包含id、公司ID、团队ID、名称、归档状态和颜色字段，使用Lombok注解简化代码。 |
| [ListDirectoryResponse.java](ListDirectoryResponse.md) | file | Java类ListDirectoryResponse继承BaseResponse，包含目录列表属性及常用注解。 |
| [JobList.java](JobList.md) | file | Java类JobList，包含jobs列表，使用Lombok注解简化代码。 |
| [GenericTeamResponse.java](GenericTeamResponse.md) | file | Java类GenericTeamResponse继承BaseResponse，包含TeamDto属性及常用注解。 |
| [CreateTeamRequest.java](CreateTeamRequest.md) | file | 创建团队请求类，含公司ID、名称、时区、周起始日和颜色字段，带验证注解。 |
| [GenericWorkerResponse.java](GenericWorkerResponse.md) | file | Java类GenericWorkerResponse继承BaseResponse，包含WorkerDto属性及常用注解。 |
| [GenericJobResponse.java](GenericJobResponse.md) | file | Java类GenericJobResponse继承BaseResponse，包含JobDto属性，使用Lombok注解生成方法。 |
| [ListAdminResponse.java](ListAdminResponse.md) | file | Java类ListAdminResponse继承BaseResponse，包含AdminEntries字段，使用Lombok注解。 |
| [GetAssociationResponse.java](GetAssociationResponse.md) | file | Java类GetAssociationResponse继承BaseResponse，包含AssociationList属性及常用注解。 |
| [ShiftDto.java](ShiftDto.md) | file | ShiftDto类包含班次ID、公司ID、团队ID、起止时间等字段，验证起止时间逻辑和最大时长限制。 |
| [GenericDirectoryResponse.java](GenericDirectoryResponse.md) | file | Java类GenericDirectoryResponse继承BaseResponse，包含DirectoryEntryDto属性及常用注解。 |
| [WorkerOfList.java](WorkerOfList.md) | file | WorkerOfList类：含userId和默认空teams列表的构建类。 |
| [ListJobResponse.java](ListJobResponse.md) | file | Java类ListJobResponse继承BaseResponse，包含JobList字段及常用注解。 |
| [WorkerEntries.java](WorkerEntries.md) | file | WorkerEntries类包含公司ID、团队ID和工人列表，支持无参、全参和构建器构造。 |
| [DirectoryEntryRequest.java](DirectoryEntryRequest.md) | file | 目录请求类，含公司ID和用户ID字段，支持全参无参构造和建造器模式。 |
| [GrowthGraphResponse.java](GrowthGraphResponse.md) | file | GrowthGraphResponse类包含每周排班人数和当前在岗人数。 |
| [AssociationList.java](AssociationList.md) | file | 类AssociationList包含账户列表和分页参数。 |
| [TeamDto.java](TeamDto.md) | file | 团队数据传输对象，含ID、公司ID、名称、时区、周起始日、颜色等字段，带校验注解。 |
| [WorkerShiftListRequest.java](WorkerShiftListRequest.md) | file | WorkerShiftListRequest类：含公司、团队、员工ID及班次时间范围校验。 |
| [NewDirectoryEntry.java](NewDirectoryEntry.md) | file | Java类NewDirectoryEntry定义，包含公司ID、名称、邮箱、电话和内ID字段，使用Lombok注解简化代码。 |
| [AdminEntries.java](AdminEntries.md) | file | Java类AdminEntries，含companyId和admins列表，支持全参无参构造和Builder模式。 |
| [ScheduledPerWeek.java](ScheduledPerWeek.md) | file | Java类ScheduledPerWeek，含week和count字段，支持全参无参构造及Builder模式。 |
| [DirectoryEntryDto.java](DirectoryEntryDto.md) | file | 目录条目数据传输对象，包含用户ID、公司ID、姓名、邮箱、电话等必填字段。 |
| [TeamList.java](TeamList.md) | file | Java类TeamList，使用Lombok注解生成构造器和建造者模式，包含默认初始化的TeamDto列表。 |


