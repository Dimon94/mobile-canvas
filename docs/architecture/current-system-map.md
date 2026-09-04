# Current System Map

Status: active

本文件描述当前可验证的系统边界、模块所有权、依赖方向和验证入口。不要写目标愿景冒充现状。

## 当前事实

- 仓库：mobile-canvas（fork 自 lnkiai/m3e-canvas，MIT；upstream remote 保留用于合并上游更新）。
- 阶段：线上内测与功能快速迭代。交付阶段与延后边界由根 repo://AGENTS.md `<stage>` 拥有。
- 业务目标：浏览器内拼移动端设计屏（M3 Expressive + iOS 26 Liquid Glass 两套 Design System）、连线、预览、导出 prompt 给 AI 编码工具。
- 技术栈：Next.js 16（App Router，静态导出 `output: export`）+ React 19 + TypeScript + Tailwind CSS 4。真相见 repo://package.json 与 repo://next.config.ts。
- 运行入口：`npm run dev`（开发）/ `npm run build`（静态导出到 `out/`）。
- 部署拓扑：GitHub Pages，repo://.github/workflows/deploy.yml（push main 自动构建，`NEXT_PUBLIC_BASE_PATH=/mobile-canvas`）。
- 持久状态 owner：浏览器 localStorage，读写归 repo://lib/project.ts；无后端。
- 外部系统：可选 AI 供应商（OpenAI/Claude/Gemini/DeepSeek），BYOK，浏览器直连，seam 在 repo://lib/ai.ts；Google Fonts（Material Symbols 图标字体）。

## 模块

| Module | Owns | Does not own | Public interface | Depends on | Verification |
| --- | --- | --- | --- | --- | --- |
| app/ | 路由与页面壳（单页） | 画布逻辑 | `app/page.tsx` | components/, lib/ | npm run build |
| components/ | 画布 UI：Palette、Inspector、Layers、Theme、Preview、Prompt 面板 | 状态持久化 | 各组件导出 | lib/, app/ | npm run build |
| lib/ | 项目状态、形状、主题、prompt 生成、i18n | UI 呈现 | lib/*.ts 导出 | 无（叶子） | npm run typecheck |

依赖方向：`app -> components -> lib`。localStorage 只经 lib/project.ts 读写。

## 链路

端到端业务链路：

    用户 -> Canvas（拖放 Part 进 Screen）-> 连线/预览 -> Prompt Export -> 外部 AI 编码工具

工具链路：

    agent -> gh CLI -> GitHub（issue / PR / Actions 读写）

## 不变量

任何实现不能破坏的根约束：

- 无后端、无账号：任何功能不得引入服务器端状态；持久化只走 localStorage（lib/project.ts）。
- 静态导出约束：不得使用 Next.js 服务端能力（SSR/ISR/Route Handlers 运行时）。
- LICENSE 与 NOTICE 归属 lnkiai 保留；上游合并只从 upstream remote 进。

## 目标记录格式

新模块首次落地时，补充以下信息：

| Module | Owns | Does not own | Public interface | Depends on | Verification |
| --- | --- | --- | --- | --- | --- |
| Unknown | Unknown | Unknown | Unknown | Unknown | Unknown |

同时记录依赖方向：

    caller -> public interface -> owning module -> external seam

缓存、日志、UI 和 read model 只能从 canonical owner 读取。它们不能反向写入 owner。

## 更新触发

以下变化必须更新本文件：

- 项目目标或顶层模块变化。
- 新增或删除服务、应用、数据库或外部系统。
- 状态所有权、跨模块依赖或公开入口变化。
- 架构迁移阶段、切换顺序或兼容边界变化。

难回退或存在真实取舍的决定同时写入 repo://docs/adr/。

## 验证

canonical 命令（来自 repo://package.json scripts）：

- `npm run build`：静态导出，基线验证命令。
- `npm run typecheck`：tsc --noEmit。
- CI：repo://.github/workflows/ci.yml。

## PROTOCOL

根合同（本文件「当前事实」「链路」「不变量」）变化时同步：

- repo://CONTEXT.md（统一语言）。
- 根 repo://AGENTS.md（指针与触发）。
- 命中模块的 AGENTS.md（L2）与文件内 L3-L6 合同。
- 架构取舍写 repo://docs/adr/ 新 ADR，不改写已 accepted 的结论。
