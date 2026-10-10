# 亲成报价系统

吊牌 / 织唛 / 唛头类印刷品的**多纸张、多工艺、多批量档位**快速报价工具。支持本地免登录使用，也支持网页版多人协作（账号分级 + 云端共享默认报价数据）。

- 线上：https://kokalabel-quote.pages.dev
- **当前版本：v12.8**（版本号的**唯一真相源** = 本仓库 `index.html` 的 `<title>` 与 `js/data.js` 的 `version` 字段）
- 仓库：`https://github.com/zly62612364469/kokalabel-quote`（私有）｜ 本地工作副本：`E:\Wb\报价优化\kokalabel-quote\`
- 详细交接资料：见 [`docs/handover/`](./docs/handover/README.md)

> ⚠️ **版本判断须知（给 AI Agent）**
> 1. 同级目录 `E:\Wb\报价优化\KOKALabel-v11.0\` ~ `KOKALabel-v12.8\` 是**历史部署快照**，代码陈旧。
>    它们**不是**当前版本，不要读取、不要修改、不要据此判断版本号。
> 2. 旧文档中的 `KOKALabel报价系统-v7.8\` 与 `E:\报价系统7.9\` 路径**已废弃**，现为 `kokalabel-quote\` 与 `E:\Wb\报价优化\`。
> 3. 判断当前版本请只读本仓库：`grep -o '亲成报价系统 v[0-9.]*' index.html`。
> 4. 版本号规则：两段式 `vx.y` + 逢十进一（换算记录见 CHANGELOG v11.0 段）。

## 技术栈

| 层 | 技术 |
|---|---|
| 前端 | 原生 HTML / CSS / JavaScript（**无框架、无构建**） |
| 表格 | SheetJS（本地化 `js/vendor/xlsx.full.min.js`，离线可用） |
| 后端 | Cloudflare Pages Functions |
| 存储 | localStorage（本地）+ Cloudflare KV（账号 / 会话 / 全局默认数据） |

> 刻意不使用框架与构建工具 —— 项目要求「双击 `index.html` 即可离线使用」。

## 页面结构（web 端 5 项菜单）

| 菜单 | 内容 |
|---|---|
| 智能报价计算器 | 主入口：多纸张报价、吊绳/邮费费用**就地点击修改**（临时覆盖、联动计算、一键重置）、报价归档（保存/浮窗查看历史） |
| 数据管理 | **报价历史**（第一顺位）/ 价格配置导入导出 / 在线修改 / 快照与统计报表 |
| 报价设置 | 客户毛利系数、金额小数位、公司信息、默认选项、配置导出 |
| 报价表组查询 | 价格矩阵查询、就地编辑、备注、表头行内编辑 |
| 个人主页 | 账号信息、功能入口（移动端由此进入数据管理/报价设置）、系统信息、全局恢复 |

> 移动端（垂直纵深式）顶部菜单保持 3 项（计算器 / 报价表组查询 / 个人主页），
> 数据管理与报价设置由个人主页「功能入口」进入。本地 `file://` 版保持原 3 项结构。

## 快速开始

```bash
# 本地运行（免登录，数据存本机 localStorage）
双击 index.html
```

## 目录结构

```
.
├─ index.html          主页面（5 视图单页 + 报价历史/详情浮窗）
├─ login.html          登录页
├─ js/
│   ├─ data.js         默认报价数据 + 版本迁移链
│   ├─ app.js          业务逻辑（计算 / 渲染 / 交互）
│   ├─ auth.js         账号与云同步（http/https 才生效）
│   ├─ login.js        登录页脚本
│   └─ vendor/         SheetJS（勿动）
├─ css/style.css       样式
├─ functions/          Cloudflare Pages Functions（登录 / 账号 / 报价云端共享）
├─ assets/             亲成图标全套 + logo
├─ docs/               项目文档（架构 / 页面 / 开发 / 部署 / 样式 / 缺陷跟踪）
│   ├─ handover/       交接文档全集（新接手者从这里开始）
│   └─ plans/          计划文档（含 archive/ 历史归档）
├─ tests/              历史测试脚本
├─ CHANGELOG.md        版本变更日志
└─ TODO.md             开发计划与进度
```

## 文档索引

**入口（根目录）**

| 文档 | 内容 |
|---|---|
| [docs/handover/README.md](./docs/handover/README.md) | ★ **交接文档全集**（新接手者从这里开始） |
| [README.md](./README.md) | 项目说明（本文件） |
| [AGENTS.md](./AGENTS.md) | 项目协作与代码开发规范 |
| [CHANGELOG.md](./CHANGELOG.md) | 版本变更日志（最新在下 / 最新在上） |
| [TODO.md](./TODO.md) | 开发计划与当前进度 |

**项目文档（docs/）**

| 文档 | 内容 |
|---|---|
| [PROJECT-SPEC.md](./docs/PROJECT-SPEC.md) | 项目定位、产品目标与功能范围 |
| [ARCHITECTURE.md](./docs/ARCHITECTURE.md) | 项目架构、目录结构与数据组织 |
| [PAGE-STRUCTURE.md](./docs/PAGE-STRUCTURE.md) | 页面结构与关键 DOM 锚点 |
| [CORE-LOGIC.md](./docs/CORE-LOGIC.md) | **核心计算逻辑与前端不可见机制白皮书**（计价引擎 / 后端 API / 存储 / 优化建议） |
| [DEVELOPMENT.md](./docs/DEVELOPMENT.md) | 开发与验证流程（含本机路径与 jsdom 回归） |
| [DEPLOYMENT.md](./docs/DEPLOYMENT.md) | 构建与部署说明（含本机 wrangler SOP） |
| [UI-STYLE.md](./docs/UI-STYLE.md) | UI 样式与设计规范 |
| [UI-OPTIMIZATION-PLAN.md](./docs/UI-OPTIMIZATION-PLAN.md) | UI 优化方案（书面） |
| [ISSUES.md](./docs/ISSUES.md) | 缺陷跟踪与已修项 |
| [plans/](./docs/plans/) | 计划文档（`archive/` 为历史归档） |

## 关键纪律（改代码前必读）

1. **改代码只改本仓库**，部署前生成干净部署目录（排除 docs/、tests/、AGENTS.md 等内部文件）。
2. **不引入框架、不引入构建步骤**——保持「双击 `index.html` 可离线运行」。
3. 品牌显示文案为「**亲成**」；`KOKA` / `tagPricing_` / `koka*` 等内部标识符**保持原名勿改**（关系用户数据与云端兼容）。
4. 版本号：**两段式 `vx.y` + 逢十进一**（y≥10 进位）；`dataVersion`（迁移基线）与界面版本号含义不同，勿连带修改。
5. 改完必须验证并跑回归（见 [docs/DEVELOPMENT.md](./docs/DEVELOPMENT.md)），部署 **必须用 wrangler v3**（本机可用实例与完整 SOP 见 [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md)）。

## 许可与归属

内部使用项目。
