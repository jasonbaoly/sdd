## Spec-Driven Development (SDD)
SDD 是所有项目的工程规范。在 AI 对话中引用本文件后，以下规则生效。

### 模板位置
模板存放在本文件同级的 `templates/` 目录：
```
sdd/
  SDD.md               # 本文件（规则定义）
  templates/
    spec.md             # 通用骨架（§1-4 所有项目必填）
    plan.md             # 实现计划模板（含技术栈 + 领域设计）
    tasks.md            # 任务拆分模板
    sections/           # 领域 section，按项目类型选填 plan.md 的"领域设计"章节
      mobile.md         # iOS / Android 客户端
      backend.md        # Java / Python / Go 服务端
      frontend.md       # React / Vue / Next.js Web 前端
```

### 流程
- **SDD-1 (MUST)** 变更分为两个通道，禁止跳过所在通道的步骤直接写代码：

| | 标准通道（默认） | 轻量通道 |
|---|---|---|
| 适用条件 | 所有变更 | 涉及文件 ≤ 3 且为 bugfix / Code Review 修复 / 小幅重构 / 从已有 spec 拆出的子任务 |
| 流程 | 需求澄清 → `spec.md` → `plan.md` → `tasks.md` → 编码 → 验收 | 需求澄清 → `spec.md`（仅 §1 + §4） → `tasks.md` → 编码 → 验收 |
| 标注 | — | `tasks.md` 头部标注 `**通道**: 轻量（无 plan）` |

- **SDD-2 (MUST)** 编码前必须先阅读当前 task 对应的 `spec.md`、`plan.md`（如有）、`tasks.md`。

### 文档三件套职责
| 文档 | 职责 | 关键内容 |
|------|------|----------|
| `spec.md` | 定义"做什么" | §1-4（用户故事、功能、参数、验收标准），仅描述需求与行为，不含技术选型与领域结构 |
| `plan.md` | 定义"怎么做" | 技术栈、领域设计（按项目类型选填）、实现策略、关键决策、Phase 拆分、涉及文件、测试策略 |
| `tasks.md` | 定义"具体步骤" | 每个 task 的状态、涉及文件、操作步骤、验收命令 |

> **边界判据**：改动是否应 bump `spec.md` 版本号？改用户故事/验收标准 → 改 spec；改技术栈、数据模型结构、API 路径命名 → 改 plan。

### 领域推断
- **SDD-3 (MUST)** 生成 spec 前，必须确认项目的 AI 编码工具规则文件（如 `CLAUDE.md`、`.cursorrules`、`.cursor/rules/` 等）中已明确声明技术栈。若未声明，则**中止 SDD 流程**，要求用户先在规则文件中补充技术栈声明后再继续。
- **SDD-4 (MUST)** 生成 plan 时，根据规则文件中的技术栈声明，自动选择 `templates/sections/` 下对应的领域模板填入"领域设计"章节：
  - iOS / Android / Flutter / React Native → `templates/sections/mobile.md`
  - React / Vue / Next.js / Web 前端 → `templates/sections/frontend.md`
  - Java / Python / Go / 服务端框架 → `templates/sections/backend.md`
  - 全栈单仓（如 Next.js 含 API Routes） → `templates/sections/frontend.md` + `templates/sections/backend.md`

### 偏离处理（Living Specs）

按偏离规模分级处理：

| 级别 | 定义 | 处理方式 |
|------|------|----------|
| 微调 | 字段名、常量值、文案措辞等不影响架构的变更 | 编码完成后批量同步到 spec/tasks |
| 变更 | 增删接口、改变数据流、新增依赖 | 暂停编码，更新 spec + tasks，告知用户后继续 |
| 推翻 | plan 中的关键决策被否定 | 暂停编码，按 spec → plan → tasks 逐层更新，用户确认后继续 |

**判断标准：如果这个偏离会让 plan.md 的读者产生错误预期，则至少是"变更"级别。**

- **SDD-5 (MUST)** 按上表"处理方式"列执行；未按要求暂停 / 更新 / 确认即继续编码视为违规。

### Spec 目录结构

命名格式：`{全局序号}[.{子序号}]-{需求描述 | 产品版本号}`

```
specs/
  001-feature-name/           # 简单需求
    spec.md
    plan.md
    tasks.md
  002-v1.2.0/                 # 版本迭代需求
    spec.md
    plan.md
    tasks.md
  002.1-user-profile/         # v1.2.0 的子需求 1
  002.2-api-optimization/     # v1.2.0 的子需求 2（可能是不绑版本的服务端独立变更）
  003-backend-refactor/       # 不绑版本的独立需求
```

- 全局序号从 001 递增，不重复
- 一个需求拆出多个独立子需求时，子序号从 1 递增（如 `002.1`、`002.2`）
- 子需求各有独立的 spec/plan/tasks 三件套，生命周期独立
- 第二部分是需求描述或产品版本号，二选一即可

### 设计源目录约定（UI 还原类项目适用）

涉及 UI 还原的项目，设计源（Figma / Sketch / XD 等设计工具导出物）统一落在：

```
specs/
  screens/
    XX-{名称}.md           # 结构化描述：节点 ID、React/Tailwind、尺寸/坐标/颜色变量等几何与语义信息
    assets/
      XX-{名称}.png        # 最终视觉截图：渐变/发光/阴影/blend mode 等 md 无法表达的视觉效果
    README.md              # 可选：设计源索引与变更日志
```

- **SDD-6 (MUST)** 涉及 UI 还原的 feature，每个屏必须同时提供 `specs/screens/XX.md` 和 `specs/screens/assets/XX.png`，二者缺一不可。只有 md 没有 png 不合格（md 中 React/Tailwind 描述无法表达渐变、blend mode、噪点等视觉效果）。
- **SDD-7 (MUST)** 可复用的图标 / 矢量 / 栅格资产（非整屏截图）落在项目原生资产目录，而非 `specs/screens/assets/`：
  - iOS: `{Project}/Assets.xcassets/`
  - Android: `res/drawable/`
  - Web: `public/images/` 或 `src/assets/`
- **SDD-8 (SHOULD)** 更换设计同步工具时（如切换 Figma → Sketch、更换同步 skill）保持本目录约定，避免下游 CLAUDE.md / 模板 / 已有 spec 大规模改动。

> 本小节规定"放在哪里"；具体"如何在 spec/plan/tasks 中引用视觉源"见各项目类型的 section 模板（如 `templates/sections/mobile.md` §5.8 视觉源 / §5.9 视觉还原条款）。

### 需求澄清
- **SDD-9 (MUST)** 生成 spec 前，AI 必须先完成需求澄清。澄清方式和深度取决于需求来源：

| 需求来源 | 澄清方式 | spec 侧重点 |
|----------|----------|-------------|
| PRD | AI 读取 PRD，提取结构化需求，仅针对 PRD 未覆盖的技术细节提问 | §1-§2 引用/转写 PRD，§4 补充技术验收标准 |
| 简要描述 | **互动式**（全新功能）或**假设式**（扩展已有功能）| 完整填写 §1-§4 |
| 口头需求 | **互动式**：逐个提问灰色地带，必须覆盖非目标和边界 | 完整填写 §1-§4 |

  - **互动式**：针对功能的灰色地带逐个提问
  - **假设式**：AI 先读相关代码和已有 spec，提出实现假设，用户只纠正错误
- **SDD-9a (MUST)** 澄清完成的标志：用户明确确认可以写 spec。未经确认不得开始生成 spec.md。

### 生成规则
- **SDD-10 (MUST)** `plan.md` 基于 `spec.md` 生成，必须标注 `基于: spec.md vX.X`。
- **SDD-11 (MUST)** `tasks.md` 基于 `plan.md`（或 spec.md，轻量通道时）生成，每个 task 必须包含：状态、涉及文件、操作步骤、验收命令。
- **SDD-12 (SHOULD)** spec 变更与 code 变更分开提交，便于追溯和回滚。

### 验收规则
- **SDD-13 (MUST)** Task 标记完成前，必须执行该 task 的所有验收命令，并在 tasks.md 中记录实际结果。验收失败的 task 禁止标记完成。
- **SDD-14 (SHOULD)** 当 task 跨多个 session 完成，或实现中发现影响后续 task 的重要信息时，在对应 task 下记录简短备注。
