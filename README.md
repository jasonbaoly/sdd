# SDD — Spec-Driven Development

SDD 是面向所有项目的工程规范，定义「需求 → 设计 → 任务 → 编码」的标准化流程，用于在 AI 协同开发中保持一致的产出质量。

## 仓库结构

```
sdd/
  SDD.md               # 规则定义（在 AI 对话中引用后生效）
  templates/
    spec.md            # 通用需求骨架（§1-4 所有项目必填）
    plan.md            # 实现计划模板（技术栈 + 领域设计）
    tasks.md           # 任务拆分模板
    sections/          # 按项目类型选填的领域 section
      mobile.md        # iOS / Android 客户端
      backend.md       # Java / Python / Go 服务端
      frontend.md      # React / Vue / Next.js Web 前端
```

## 文档三件套

| 文档 | 职责 | 关键内容 |
|------|------|----------|
| `spec.md` | 做什么 | 用户故事、功能、参数、验收标准 |
| `plan.md` | 怎么做 | 技术栈、领域设计、实现策略、Phase 拆分 |
| `tasks.md` | 具体步骤 | 每个 task 的状态、涉及文件、操作、验收 |

## 接入方式

把整个仓库目录引入你的项目，下面两种方式二选一。

### 方式 1：Git Submodule（推荐，便于跟随上游更新）

```bash
cd your-project
git submodule add git@github.com:jasonbaoly/sdd.git sdd
```

在项目根目录的 `CLAUDE.md`（或同等规则文件）中加一行：

```markdown
工程规范见 @sdd/SDD.md，模板见 sdd/templates/
```

更新规范：

```bash
cd your-project/sdd && git pull origin main
cd .. && git add sdd && git commit -m "chore: bump sdd"
```

### 方式 2：直接拷贝

```bash
cp -r /path/to/sdd your-project/sdd
```

同样在 `CLAUDE.md` 中引用 `@sdd/SDD.md`。

---

## 使用流程

1. 在项目 `CLAUDE.md` 中引用 `@sdd/SDD.md`，使规则生效。
2. 直接向 AI 描述需求，AI 会按 `SDD.md` 流程自动澄清需求、套用 `sdd/templates/` 下的模板、依次产出 `specs/{NNN}-{feature}/` 下的 spec → plan → tasks，并在每一步与你对齐。
3. AI 编码前会先阅读当前 task 对应的三件套，再开始实现并跑验收。

详细流程、通道划分（标准 / 轻量）、偏离处理（Living Specs）参见 [SDD.md](./SDD.md)。
