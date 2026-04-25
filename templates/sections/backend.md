# 领域 Section：服务端（Java / Python / Go）

将以下内容填入 plan.md 的"领域设计"章节。

---

### 5.1 API 端点

| 端点 | 方法 | 说明 | 认证 |
|------|------|------|------|
| `{/api/path}` | GET/POST/PUT/DELETE | {用途} | 是/否 |

### 5.2 请求/响应格式

#### {端点名}
**Request**:
```json
{
  "field": "type — 说明"
}
```

**Response (200)**:
```json
{
  "field": "type — 说明"
}
```

**Error Response**:
```json
{
  "error": "string — 错误描述",
  "code": "string — 错误码"
}
```

### 5.3 数据库模型

```sql
CREATE TABLE {table_name} (
  {column}  {type}  {constraints}  -- {说明}
);
```

> 或使用 ORM 语法（Prisma / SQLAlchemy / JPA Entity），与项目技术栈一致。

### 5.4 外部服务依赖

| 服务 | 用途 | 接口方式 | 降级策略 |
|------|------|----------|----------|
| {服务名} | {用途} | REST / gRPC / SDK | {超时/失败时的处理} |

### 5.5 安全设计

| 关注点 | 方案 | 说明 |
|--------|------|------|
| 认证 | JWT / Session / OAuth | {具体实现} |
| 鉴权 | RBAC / ABAC | {角色/权限模型} |
| 数据加密 | 传输层 / 存储层 | {具体方案} |
| 速率限制 | {策略} | {阈值、维度} |

### 5.6 部署与运维

| 项目 | 方案 |
|------|------|
| 部署方式 | K8s / ECS / Lambda / VM |
| CI/CD | {工具和流程} |
| 监控/告警 | {指标、告警规则} |
| 日志 | {格式、收集方式} |
