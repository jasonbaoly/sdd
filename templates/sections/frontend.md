# 领域 Section：Web 前端（React / Vue / Next.js）

将以下内容填入 plan.md 的"领域设计"章节。

---

### 5.1 页面结构与路由

```
{路由}  — {说明}
```

| 路由 | 页面 | 认证 | 说明 |
|------|------|------|------|
| `{/path}` | {PageName} | 是/否 | {职责} |

### 5.2 页面布局

{关键页面的 wireframe（ASCII 图或 Figma 链接），标注交互区域和组件边界}

### 5.3 组件拆分

```
components/
  {feature}/
    {ComponentA}.tsx    — {职责}
    {ComponentB}.tsx    — {职责}
```

### 5.4 状态管理

| Store / Context | 管理的状态 | 持久化 |
|-----------------|-----------|--------|
| {store 名} | {状态字段} | localStorage / sessionStorage / 无 |

### 5.5 API 依赖

| 页面/组件 | 接口 | 方法 | 说明 |
|-----------|------|------|------|
| {组件名} | `{/api/path}` | GET/POST | {用途} |

> 如服务端 spec 已存在，直接引用：`详见 specs/{NNN}-{feature}/spec.md § API 端点`

### 5.6 响应式与浏览器兼容

| 断点 | 布局变化 |
|------|----------|
| Mobile (< 768px) | {布局说明} |
| Tablet (768-1024px) | {布局说明} |
| Desktop (> 1024px) | {布局说明} |

**浏览器支持**: {最低版本要求}
