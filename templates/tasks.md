# Tasks {NNN}: {功能名称}

**基于**: spec.md v{X.X} + plan.md
**规则**: 完成 task 后运行验收命令，通过后标记 ✅

---

## 粒度规则（tasks 的写作边界）

> **tasks.md = 执行层**：告诉执行者"做什么、按什么顺序、用什么关键数据、怎么验收"。
> 执行者（AI agent 或人类）负责根据 spec + plan 的上下文自行完成具体实现。

### Task 拆分原则
- **一个 Task = 一个可独立验收的交付物**（一个 API、一个组件、一个模块改造）
- **一个 Task ≈ 一次有意义的 git commit**——如果无法用一句话描述 commit 做了什么，task 太大了
- 一个 Task 涉及的文件建议 **≤ 5 个**，超出则拆分
- Task 之间标注前置依赖（`依赖: Task X.X`），无依赖则可并行

### 操作描述应该包含
- ✅ **执行流程**：按顺序列出要做的事（新建/改造哪些文件、调用哪些模块）
- ✅ **关键数据**：枚举值、字段名、API 路径、配置常量等不能靠猜的硬编码信息
- ✅ **核心逻辑提示**：算法要点、联动规则、分支条件、边界情况
- ✅ **spec/plan 引用**：`详见 spec.md §X.X` / `联动规则见 plan.md Phase X`

### 操作描述禁止包含
- ❌ 完整的代码实现（函数体、组件 JSX、SQL 全文）
- ❌ import 语句
- ❌ Commit message
- ❌ 与本 Task 无关的背景解释（那是 plan.md 的职责）

### 验收标准原则
- 每个 Task 至少一条**可执行的验收命令**（如 `pnpm typecheck`、`pnpm test`、curl 请求）
- 补充**行为验收**描述预期结果（如"返回 400 + INVALID_FORMAT"）
- 验收应覆盖 spec.md 对应的 EARS 验收条件
- **标记完成前必须执行验收命令并记录实际结果**（见 SDD-13）

---

## Phase 1: {阶段名}

### Task 1.1: {任务名称}
**状态**: ⬚ 未开始
**依赖**: 无 | Task X.X
**涉及文件**:
- `{path/to/file}`（新增 | 改造）

**前置阅读**（UI 还原类 Task 必填，其他类型可省略；路径约定见 `sdd/SDD.md` § 设计源目录约定）:
- `specs/screens/XX-....md`
- `specs/screens/assets/XX.png`
- `~/.claude/rules/UI_RESTORATION.md`

**操作**:
1. {做什么 + 关键数据/逻辑提示，不写具体代码}
2. {做什么 + 引用 spec/plan 章节}

**验收**（UI 还原类 Task 必须含"视觉"一行；验收命令参考 `sections/mobile.md §5.7`）:
- [ ] `{可执行的验证命令}` — 预期: {预期结果} | 实际: {执行后填写}
- [ ] {行为描述} — 预期: {预期结果} | 实际: {执行后填写}
- [ ] 视觉: 模拟器截图逐项对照 spec §4.N / `specs/screens/assets/XX.png` 通过 — 实际: {执行后填写}

**备注**（可选，跨 session 或发现重要信息时填写）:
- {简短记录关键决策、偏离原因、对后续 task 的影响}

---

{重复 Task 结构...}

---

## 总验收闸门

所有 Task 完成后，在此集中核对全局性验收项：

- [ ] 所有 Task 标记 ✅ 且验收结果已回填
- [ ] `xcodebuild ... build` 无新增 warning / error
- [ ] 视觉还原整体截图对比通过（UI 还原类 feature 必填，逐屏对照 `specs/screens/assets/*.png`）
- [ ] spec/plan 的偏离已按 SDD-5 同步回文档
