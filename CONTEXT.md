# mobile-canvas Context

本文件保存项目统一语言：已经确认的业务概念、边界和避免使用的同义词。

项目 fork 自 lnkiai/m3e-canvas（MIT）。首个需求调研已完成：产品为浏览器内多设计系统移动端画布，M3 Expressive 与 iOS 26 Liquid Glass 两套设计内容，项目级全局切换。

## 使用规则

- 本文件按两遍读。开工只读「使用规则」与「词汇索引」；当前任务涉及的词条，按索引锚点或 `grep -n '^\*\*'` 定位后单独读入。
- 代码、测试、issue、ADR 和文档使用本文件的 canonical term。
- 一个概念只保留一个 canonical term。
- 新概念入门依次判断：代码和运行状态是否已表达该概念；需求或 accepted ADR 是否确认边界；它是否只是现有概念的同义词。前两项有证据且第三项为否时，才新增词条。
- 需要区分近义词时，写清边界和 Avoid 列表。
- 不确定的定义标为 Unknown。
- 工程词汇（Module、Interface、Seam、Adapter、Ownership 等）的 canonical 定义归 repo://docs/agents/architecture-standards.md 第 2 节。

## 词汇索引

每个已确认词条一行：canonical term + 一句话定义。完整定义、边界和 Avoid 列表见「词条详情」同名锚点。

**Canvas**
: 浏览器内无限画布，承载多个 Screen；拖放、缩放、连线、对齐都在其上发生。实现入口 repo://app/page.tsx。
_Avoid_: 白板、画板、editor（作术语时）。

**Screen**
: 画布上的一个设备屏幕（412×892 phone 或 1280×800 desktop），有名字、背景、z-order 部件列表；同名双尺寸 Screen 在 Prompt 中合并为一个响应式屏幕。
_Avoid_: 页面、page、frame。

**Part**
: 拖入 Screen 的单个设计控件（按钮、导航栏、卡片等），有位置、样式属性和可选的跳转目标。M3 册渲染见 repo://components/M3Node.tsx。
_Avoid_: 组件（与 React component 混淆时）、widget、element。

**Design System**
: 一套完整设计内容：Part 册（Palette）+ 主题轴 + Prompt 文案变体。当前两个：`m3`（Material 3 Expressive，fork 自带）与 `ios`（iOS 26 Liquid Glass，待建）。项目级全局切换，不同屏不混用。
_Avoid_: 主题（theme 只是 Design System 的一个轴）、风格。

**Palette**
: 某 Design System 下可选 Part 的清单与拖入面板。iOS 册三个内容来源：Apple 官方组件（HIG / iOS 26+27）、ChunUI 风格、Telegram iOS 风格。
_Avoid_: 控件库、素材库。

**Prompt Export**
: 把整个设计或单屏导出为给 AI 编码工具的自然语言 prompt（日/英/中三语），是本产品对外的核心合同。
_Avoid_: 代码导出（不导出代码，只导出 prompt）。

## 词条详情

词条格式：

    **Canonical Term**
    : 定义、边界和使用场景。
    _Avoid_: 同义词和禁用写法。

## 待确认词汇

以下内容为 Unknown。项目形成真实需求时再补充：

- 产品目标和成功指标（当前仅有方向：多设计系统移动端画布）。
- 用户角色和权限边界（当前无账号体系，单机 localStorage）。
