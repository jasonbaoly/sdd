# Plan {NNN}: {功能名称}

**基于**: spec.md v{X.X}
**生成日期**: {YYYY-MM-DD}

---

## 粒度规则（plan vs tasks 边界）

> **plan.md = "怎么做"的策略层**，tasks.md = "具体步骤"的执行层。
> 两者不应重复，plan 写到"做什么 + 为什么"即止。

### plan.md 应该包含
- 整体实现策略和阶段划分
- 每个 Phase 的目标和交付物（一句话）
- 关键技术决策及权衡理由
- 模块间的数据流 / 接口契约（伪代码或流程图级别，不是完整实现）
- 涉及文件清单（路径 + 一句话职责）
- 风险点和降级方案
- 测试策略

### plan.md 禁止包含
- ❌ 具体代码实现（完整的代码块）
- ❌ Shell / Git 命令
- ❌ Commit message
- ❌ Step-by-step 操作序列（如 "Step 1: 写 XX 函数 → Step 2: ..."）
- ❌ 逐字段的代码结构（应引用 spec.md 章节，而非重复其内容）

如需描述接口形状，使用**简写伪代码**而非可运行的代码块。例如：
```
createVideoTask(modelId, mode, prompt, params, imageUrls, videoRefs, audioRefs) → taskId
```

---

## 技术栈

{仅列出与本 plan 相关的技术选型，引用项目规则文件（CLAUDE.md 等）中的全局技术栈声明；与全局一致则直接引用，有偏离时在此说明理由}

---

## 领域设计（按项目类型选填）

根据项目类型，从 `sdd/templates/sections/` 中选择对应模板填写：
- iOS / Android / Flutter / React Native → `sections/mobile.md`
- React / Vue / Next.js / Web 前端 → `sections/frontend.md`
- Java / Python / Go / 服务端框架 → `sections/backend.md`

> 领域设计描述"怎么组织实现"（屏幕导航、数据模型、API 依赖、平台特性等），不描述"做什么"。
> 视觉源清单（UI 还原类）仍填在此处，供各 Phase 引用。

---

## 实现策略

{一句话描述整体策略，如"由内到外"、"先 API 后 UI"等}

```
Phase 1: {阶段名}
Phase 2: {阶段名}
...
```

---

## Phase 1: {阶段名}

**目标**：{一句话描述本阶段交付物}

### 关键决策
- {技术选型、架构权衡、设计取舍——写清"选了什么"和"为什么"，不写具体实现}

### 数据流 / 接口契约
```
{请求 → 处理 → 响应的流程图或伪代码签名，不写函数体}
```

### 涉及文件
- `{path/to/file}`（新增 | 改造 — {一句话职责}）

### UI 还原标准（涉及视觉还原时必填）
- 资产来源：`Assets.xcassets/FigmaAssets/`（或项目约定的资产目录），不得临时从 PNG 导入
- 禁止自绘静态 Shape / Path / SF Symbol 替代设计稿资产（参考 `~/.claude/rules/UI_RESTORATION.md`）
- 必须复用的共享组件：{例如 `NavBar` / `PrimaryButton` / `SecondaryButton`}
- 参照视觉源：{列出本 Phase 涉及的屏号及其 md + png 路径}

### 风险 / 注意事项
- {可选：降级方案、兼容性、数据迁移要点等}

---

{重复 Phase 结构...}

---

## 测试策略

| 层级 | 工具 | 覆盖范围 |
|------|------|----------|
| 单元测试 | {工具} | {覆盖的模块} |
| 集成测试 | {工具} | {覆盖的端点/流程} |
| E2E / UI 测试 | {工具} | {覆盖的用户场景} |
