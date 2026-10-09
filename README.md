# 亲成报价系统

吊牌 / 织唛 / 唛头类印刷品的**多纸张、多工艺、多批量档位**快速报价工具。支持本地免登录使用，也支持网页版多人协作（账号分级 + 云端共享默认报价数据）。

- 线上：https://kokalabel-quote.pages.dev
- 当前版本：**v11.2**（版本号规则：两段式 `vx.y` + 逢十进一，换算记录见 CHANGELOG v11.0 段）
- 详细转手资料：见 [`转手文档/`](./转手文档/README.md)

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
| [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md) | 构建与部署说明（含本机 wrangler SOP） |
| [docs/UI-STYLE.md](./docs/UI-STYLE.md) | UI 样式与设计规范 |
| [CHANGELOG.md](./CHANGELOG.md) | 版本变更记录 |
| [问题日志.md](./问题日志.md) | 缺陷跟踪与已修项 |

## 关键纪律（改代码前必读）

1. **改代码只改本仓库**，部署前生成干净部署目录（排除 docs/转手文档/tests 等内部文件）。
2. **不引入框架、不引入构建步骤**——保持「双击 `index.html` 可离线运行」。
3. 品牌显示文案为「**亲成**」；`KOKA` / `tagPricing_` / `koka*` 等内部标识符**保持原名勿改**（关系用户数据与云端兼容）。
4. 版本号：**两段式 `vx.y` + 逢十进一**（y≥10 进位）；`dataVersion`（迁移基线）与界面版本号含义不同，勿连带修改。
5. 改完必须验证并跑回归（见 [docs/DEVELOPMENT.md](./docs/DEVELOPMENT.md)），部署 **必须用 wrangler v3**（本机可用实例与完整 SOP 见 [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md)）。

## 许可与归属

内部使用项目。
