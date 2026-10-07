# 亲成报价系统

吊牌 / 织唛 / 唛头类印刷品的**多纸张、多工艺、多批量档位**快速报价工具。支持本地免登录使用，也支持网页版多人协作（账号分级 + 云端共享默认报价数据）。

- 线上：https://kokalabel-quote.pages.dev
- 当前版本：**v10.8.0**
- 详细转手资料：见 [`转手文档/`](./转手文档/README.md)

## 技术栈

| 层 | 技术 |
|---|---|
| 前端 | 原生 HTML / CSS / JavaScript（**无框架、无构建**） |
| 表格 | SheetJS（本地化 `js/vendor/xlsx.full.min.js`，离线可用） |
| 后端 | Cloudflare Pages Functions |
| 存储 | localStorage（本地）+ Cloudflare KV（账号 / 会话 / 全局默认数据） |

> 刻意不使用框架与构建工具 —— 项目要求「双击 `index.html` 即可离线使用」。

## 快速开始

```bash
# 本地运行（免登录，数据存本机 localStorage）
双击 index.html

# 跑回归测试（改代码前后各一次，期望 21 项全绿）
node ../_verify/v1080.test.cjs
```

## 目录结构

```
.
├─ index.html          主页面（计算器 / 报价表组查询 / 个人主页，单页三视图）
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
├─ docs/               项目文档（架构 / 页面 / 开发 / 部署 / 样式）
├─ tests/              历史测试脚本
├─ 转手文档/           转手交接文档全集
├─ CHANGELOG.md        版本变更日志
└─ 问题日志.md         缺陷跟踪
```

## 文档索引

| 文档 | 内容 |
|---|---|
| [转手文档/](./转手文档/README.md) | ★ 转手交接文档全集（新接手者从这里开始） |
| [AGENTS.md](./AGENTS.md) | 项目协作与代码开发规范 |
| [TODO.md](./TODO.md) | 开发计划与当前进度 |
| [docs/PROJECT-SPEC.md](./docs/PROJECT-SPEC.md) | 项目定位、产品目标与功能范围 |
| [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) | 项目架构、目录结构与数据组织 |
| [docs/PAGE-STRUCTURE.md](./docs/PAGE-STRUCTURE.md) | 页面结构与关键 DOM 锚点 |
| [docs/DEVELOPMENT.md](./docs/DEVELOPMENT.md) | 开发与验证流程 |
| [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md) | 构建与部署说明 |
| [docs/UI-STYLE.md](./docs/UI-STYLE.md) | UI 样式与设计规范 |
| [CHANGELOG.md](./CHANGELOG.md) | 版本变更记录 |
| [问题日志.md](./问题日志.md) | 缺陷跟踪与已修项 |

> 说明：本项目没有「组件库 / 官网」形态，故未设 `COMPONENT-GUIDELINES.md` 与 `REGISTRY.md`；
> 原 `DESIGN.md` 的职责由 `docs/UI-STYLE.md` 承担（本项目只有一套应用 UI，没有官网）。

## 许可与归属

内部使用项目。品牌显示文案为「**亲成**」；
代码内部标识符（`KOKA` 对象、`KOKA_AUTH` KV、`tagPricing_` 存储前缀）**保持原名勿改**（关系用户数据与云端兼容）。
