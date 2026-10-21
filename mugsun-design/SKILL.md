---
name: mugsun-design
description: 为 mugsun-boot 设计新的业务模块（实体、Mapper、Service、Controller、Flyway 迁移、租户与数据权限、PC 页面）。当用户说「新建模块」「加一张业务表」「设计一个 CRUD」「mugsun 里怎么建表」时使用。
---

# mugsun-design：新模块设计

## 触发词

新建模块、新增业务表、设计 CRUD、加一个管理页面、mugsun 建表、new module、add entity。

## 必问参数

动手前逐项确认，缺哪项就问哪项：

1. 模块名与表名：包名（如 `com.mugsun.boot.<模块>`）和表名前缀。
2. 是否按租户隔离：是则表里要有 `tenant_id`。
3. 是否需要行级数据权限：是则表里要有部门归属列和本人归属列（如 `dept_id`、`create_by`）。
4. 字段清单：字段名、类型、是否必填、是否唯一（唯一键在租户内还是全局）。
5. 权限码：形如 `<模块>:<资源>:<动作>`。
6. 是否需要前端页面：需要的话放在 PC 管理台哪个菜单下。

## 默认值

- 实体继承 `com.mugsun.core.mybatis.base.BaseEntity`（雪花主键 `id`、`createTime`、`updateTime`、逻辑删除 `isDeleted`），不要自己再声明这几列。
- 默认按租户隔离：实体带 `tenantId` 字段，表带 `tenant_id` 列。
- 默认不开数据权限；要开时才登记受控表。
- 数据库按三套方言同号写迁移：PostgreSQL、达梦、openGauss。
- 接口统一返回 `R<T>`（`R.data(...)`、`R.success(...)`、`R.fail(...)`）。

## 不适用于

- 修改已发布的 Flyway 迁移：已发布的迁移不得修改，只能新增。
- mugsun-core 的 starter 开发：那是基础库，不放业务模块。
- 埋点 SDK、移动端组件库的设计。
- 微服务拆分（mugsun 走单体加可选模块）。

## 步骤

1. 建表迁移：在 `db/migration`、`db/migration-dm`、`db/migration-og` 三个目录各新增同号的 `Vn__<说明>.sql`，版本号接在现有最大号之后；脚本尽量写成幂等。唯一索引要考虑 `tenant_id` 和逻辑删除列。
2. 实体：`@Table("<表名>")`，`extends BaseEntity`，放在 `<模块>/entity`。
3. Mapper：`<模块>/mapper`，继承 MyBatis-Flex 的 `BaseMapper<实体>`。
4. Service：`<模块>/service`，业务逻辑和事务在这一层。insert / update 前调用实体的 `sanitizeForInsert()` / `sanitizeForUpdate()`，不信任请求体里的审计字段。
5. Controller：`<模块>/controller`，每个接口用 `@SaCheckPermission("<权限码>")` 声明权限，返回 `R<T>`。
6. 租户：查询默认由框架自动加租户条件。确实要跨租户时，只用 `TenantContext.ignore(...)` 或 `TenantContext.execute(租户, ...)` 包住最小范围；异步任务靠 `TenantTaskDecorator` 透传租户，不要自己存 ThreadLocal。
7. 数据权限：需要行级权限时，在启动期调用 `DataScopeTables.register(表名, 部门列, 本人列)` 登记表，并在查询方法上加 `@DataScope`。
8. 前端页面：在 PC 管理台按现有页面的目录和接口封装方式新增页面，四种状态（空表单提交、英文、暗色、390px 宽）都要能用。
9. 测试：至少覆盖租户隔离（换租户查不到）、权限（无权限码返回 403）和唯一约束。

## 产出

- 一份设计说明：表结构、索引、权限码、是否租户隔离、是否数据权限。
- 三套方言的迁移文件、实体、Mapper、Service、Controller、测试，必要时加前端页面。
- 一份待办：发版时把新迁移追加进已发布清单。
