# 领域 Section：移动客户端（iOS / Android）

将以下内容填入 plan.md 的"领域设计"章节。

---

### 5.1 屏幕列表与导航流

```
{屏幕A} → {屏幕B} → {屏幕C}
              ↓
          {屏幕D}（Modal）
```

| 屏幕 | 导航方式 | 说明 |
|------|----------|------|
| {屏幕名} | Push / Modal / Tab / Replace | {职责} |

### 5.2 屏幕布局

{关键屏幕的 wireframe（ASCII 图或 Figma 链接），标注交互区域}

### 5.3 本地数据模型

{客户端本地持久化的实体定义（CoreData / Room / SQLite）}

```
{Entity}
  {字段}  {类型}  {约束/说明}
```

> 注意：与服务端 Schema 的映射关系需在此标注，如字段命名差异、类型转换。

### 5.4 API 依赖

| 屏幕/功能 | 接口 | 方法 | 说明 |
|-----------|------|------|------|
| {屏幕名} | `{/api/path}` | GET/POST | {用途} |

> 如服务端 spec 已存在，直接引用：`详见 specs/{NNN}-{feature}/spec.md § API 端点`

### 5.5 离线与缓存策略

| 场景 | 策略 | 说明 |
|------|------|------|
| 无网络时浏览 | 缓存优先 / 仅提示 / 降级 | {具体行为} |
| 无网络时操作 | 本地队列 → 恢复后同步 / 拒绝 | {具体行为} |
| 缓存过期 | {TTL 或条件} | {刷新机制} |

### 5.6 平台特性

| 特性 | 是否需要 | 说明 |
|------|----------|------|
| 推送通知 | 是/否 | {触发条件、payload 格式} |
| 深链 / Universal Link | 是/否 | {URL scheme、路由规则} |
| 系统权限（相机/相册/定位等） | 是/否 | {申请时机、拒绝后降级} |
| 后台任务 | 是/否 | {场景：上传/下载/定位} |
| Widget / 快捷操作 | 是/否 | {内容} |

### 5.7 验收命令参考（供 tasks.md 填写验收行时选用）

按项目实际栈选对应栏。多栈仓库（如 Flutter 项目同时跑 iOS / Android）两栏都要覆盖。

| 类型 | iOS（SwiftUI / UIKit） | Flutter | Android 原生（Compose / View） |
|------|------------------------|---------|-------------------------------|
| 编译通过 | `xcodebuild -project {Project}.xcodeproj -scheme {Scheme} -sdk iphonesimulator build` | `flutter build ios --debug --no-codesign`（iOS 输出）/ `flutter build apk --debug`（Android 输出） | `./gradlew :app:assembleDebug` |
| 单元 / UI 测试 | `xcodebuild test -project {Project}.xcodeproj -scheme {Scheme} -sdk iphonesimulator -destination 'platform=iOS Simulator,name=iPhone 17'` | `flutter test`（widget / unit）/ `flutter test integration_test`（端到端） | `./gradlew :app:testDebugUnitTest` / `./gradlew :app:connectedDebugAndroidTest` |
| 静态检查 | `swift build` / `swiftc -typecheck` / SwiftLint（若引入） | `flutter analyze`（必跑）/ `dart format --output=none --set-exit-if-changed .`（CI 模式） | `./gradlew :app:lintDebug` / ktlint（若引入） |
| 视觉还原 | 模拟器截图对照 `specs/screens/assets/XX.png` 逐项核对（不得只用 "Preview 渲染正常" 兜底） | `flutter run -d <device-id>` 启动后 `flutter screenshot --type=device` 抓图，与 `specs/screens/assets/XX.png` 像素级对照 | `adb shell screencap` 抓图后对照 `specs/screens/assets/XX.png` |
| 真机行为 | 仅用于权限弹窗 / 推送 / 相机 等模拟器不支持的场景，需在 Task 验收中标明 "真机手测" | 同左：相机、推送、生物识别、深链等需真机手测；Flutter `flutter run -d <real-device-id>` 抓现象 | 同左 |

> Flutter 项目的视觉还原同样禁止只用 "热重载看着像" 兜底——必须截图后与 PNG 对照，2pt 误差是上限。

### 5.8 视觉源（涉及 UI 还原时必填，填入 plan.md 的"领域设计"章节）

路径约定见 `sdd/SDD.md` § 设计源目录约定（SDD-6）。

| 屏号 | Spec md | 视觉 PNG |
|------|---------|----------|
| {XX} | `specs/screens/XX-....md` | `specs/screens/assets/XX.png` |

> 只列 md 不列 png 不合格。每个屏都要成对列出。

### 5.9 视觉还原条款（涉及 UI 还原时合入 spec.md §4 验收标准）

用**可像素级核对**的 EARS 条款描述每个屏的结构 / 尺寸 / 资产 / 禁止项，允许 ±2pt 误差。禁止写"按 Figma 还原"这类空话。示例：

- When 进入 XX 屏, the system shall 视频层从屏幕物理顶部铺到下方 700pt，误差 ±2pt。
- When XX 屏渲染完成, the system shall 使用 `Assets.xcassets/FigmaAssets/{AssetName}` 作为背景，而非自绘 Shape。
- The system shall not 使用 SF Symbol 替代设计稿里的 {具体图标节点}。
