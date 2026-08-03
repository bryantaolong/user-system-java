# 用户系统

## 项目简介

本项目为基于 Spring Boot 3 的用户管理系统，支持用户注册、登录、信息管理、权限控制、数据导出等功能。后端采用 PostgreSQL 作为主数据库，Redis 用于缓存和分布式场景，支持 JWT 无状态认证和基于角色的权限控制。

## 技术栈

### 后端

* Java 17
* Spring Boot 3.5.4
* MyBatis
* PostgreSQL 17.x / MySQL 8.0.x
* Redis
* Spring Security
* EasyExcel（阿里巴巴 Excel 导出）
* Lombok
* JJWT（JWT 令牌）
* Maven 3.9.x

### 前端

* Vue 3
* TypeScript
* Vite
* Arco Design Vue
* Vue Router
* Pinia
* Axios

## 项目结构

```
backend/
  src/
    main/
      java/com/bryan/system/
        config/         # 配置类（安全、Redis、MyBatis 等）
        controller/     # RESTful 控制器
        domain/         # 实体类、请求/响应对象、VO
        filter/         # JWT 认证过滤器
        handler/        # 全局异常处理器
        mapper/         # MyBatis 映射接口
        service/        # 业务逻辑层
        util/           # 工具类（JWT、HTTP 等）
      resources/
        application.yaml
        application-dev.yaml
        application-mysql.yaml
        mapper/         # MyBatis 映射 XML
    test/
      java/com/bryan/system/
  pom.xml
  mvnw

frontend/
  src/
    api/               # API 请求模块
    assets/            # 静态资源
    components/        # 公共组件
    views/             # 页面级 Vue 组件
    router/            # Vue Router 路由配置
    store/             # Pinia 状态管理
    utils/             # 工具函数
    App.vue
    main.ts
  package.json
  tsconfig.json
  vite.config.ts
```

## 环境要求

* JDK 17+
* Maven 3.9.9+
* Node.js 18+
* PostgreSQL 17.x / MySQL 8.0.x
* Redis 6.x 或更高

## 配置说明

* 在 `backend/src/main/resources/application-dev.yaml` 中更新数据库和 Redis 配置。
* 通用配置（日志、MyBatis 等）在 `backend/src/main/resources/application.yaml`。
* 数据库建表脚本见 [`sql/create_table.sql`](sql/create_table.sql)。

## 启动方式

### 后端

> 以 PostgreSQL 为例

1. 初始化 PostgreSQL 数据库，执行建表脚本：

   ```sh
   psql -U postgres -d postgres -f sql/create_table.sql
   ```

2. 启动 Redis 服务。
3. 使用 Maven 构建并运行项目：

   ```sh
   cd backend
   ./mvnw spring-boot:run
   ```

   或运行打包后的 jar 包：

   ```sh
   cd backend
   ./mvnw clean package
   java -jar target/user-system-0.0.1-SNAPSHOT.jar
   ```

### 前端

1. 安装依赖：

   ```sh
   cd frontend
   npm install
   ```

2. 启动开发服务器：

   ```sh
   npm run dev
   ```

3. 构建生产版本：

   ```sh
   npm run build
   ```

开发服务器默认运行在 `http://localhost:5173`。确保后端已启动，并且 CORS 已配置为允许前端源访问。

## 常用接口

### 认证

* 用户注册：`POST /api/auth/register`
* 用户登录：`POST /api/auth/login`
* 验证令牌：`GET /api/auth/validate`
* 获取当前用户：`GET /api/auth/me`
* 修改密码：`PUT /api/auth/password`
* 注销账号：`DELETE /api/auth`
* 退出登录：`GET /api/auth/logout`

### 用户管理

* 创建用户：`POST /api/users`（管理员权限）
* 查询所有用户：`GET /api/users`（管理员权限）
* 根据ID查询用户：`GET /api/users/:userId`（管理员权限）
* 根据用户名查询用户：`GET /api/users/username/:username`（管理员权限）
* 搜索用户：`POST /api/users/search`（管理员权限）
* 更新用户：`PUT /api/users/:userId`
* 修改用户角色：`PUT /api/users/roles/:userId`（管理员权限）
* 重置密码：`PUT /api/users/password/:userId`（管理员权限）
* 封禁用户：`PUT /api/users/block/:userId`（管理员权限）
* 解封用户：`PUT /api/users/unblock/:userId`（管理员权限）
* 删除用户：`DELETE /api/users/:userId`（管理员权限）
* 导出用户数据：`GET /api/users/export`（管理员权限）

### 用户资料

* 上传头像：`POST /api/user-profiles/avatar`
* 根据用户ID查询资料：`GET /api/user-profiles/:userId`
* 根据真实姓名查询资料：`GET /api/user-profiles/name/:realName`
* 获取当前用户资料：`GET /api/user-profiles/me`
* 更新用户资料：`PUT /api/user-profiles`

### 角色管理

* 获取所有角色：`GET /api/user-roles`（管理员权限）

### 系统日志

* 查看最新日志：`GET /api/admin/logs`（管理员权限）
* 查看日志文件列表：`GET /api/admin/logs/files`（管理员权限）

## 其他说明

* JWT 密钥建议在生产环境通过配置文件注入，避免硬编码。
* 全局异常处理在 [`GlobalExceptionHandler`](backend/src/main/java/com/bryan/system/handler/GlobalExceptionHandler.java)。
* 逻辑删除字段为 `deleted`：0 表示未删除，1 表示已删除。

## License

本项目采用 MIT 协议。详见 [LICENSE](LICENSE)。
