# AGENTS.md · 项目协作与代码开发规范

> 面向在本仓库工作的开发者 / AI Agent。**动手前先读本文。**

---

## 一、铁律

1. **改代码只改 `KOKALabel报价系统-v7.8/`（即本仓库）**，它是唯一真相源。
   部署前从本仓库生成干净部署目录，不要在部署目录里改代码。
2. **不引入框架、不引入构建步骤** —— 项目必须保持「双击 `index.html` 可离线运行」。
3. **改完必须跑回归测试**：`node ../_verify/v1080.test.cjs`（21 项，全绿为准）。
4. **改 DOM 结构时，必须全局 grep 同步 `els.*` 引用** —— 否则会出现"点击无反应"类隐蔽 bug（见第五节）。
5. **不要改动 `js/vendor/xlsx.full.min.js`**（第三方库）。

---

## 二、行尾符：全项目 LF

编辑工具与 python 写文件都可能**静默把 LF 转成 CRLF**，导致 `diff` 出现上万行噪音。

```python
# 检查（必须全为 0）
d = open(f, 'rb').read(); print(d.count(b'\r\n'))

# 写文件时显式指定，不要用裸 write_text()
open(p, 'w', encoding='utf-8', newline='\n').write(s)
# 或
p.write_bytes(s.encode('utf-8'))
```

仓库需设 `git config core.autocrlf false`。

---

## 三、版本号管理

- **禁用 `sed` 全局替换** —— 曾多次静默失败。用 python 精确替换。
- 注意**无空格变体**：`"version": "10.7.0"` 与 `version: "10.7.0",` 都要覆盖。
- 需要替换的位置：`index.html`（title / `?v=` 缓存参数 / setting-desc）、`login.html`（`?v=`）、
  `js/app.js`（头注释 + `version:` 字段）、`js/data.js` / `css/style.css` / `js/auth.js`（头注释）、`CHANGELOG.md`（新增段落）。
- **历史修复注释里的旧版本号要保留**（如 `// v10.7.1 修复：...`），只改"当前版本"标记。

### ⚠️ `dataVersion` ≠ 界面版本号

`localStorage.dataVersion` 是**迁移基线**，与界面显示的版本号含义不同。
**不要用改版本号的脚本连带改它**，否则迁移守卫被污染，后续迁移会全部跳过。

新增迁移的写法（放在 `js/data.js` 尾部迁移区）：

```js
(function migrateVXXXX() {
  const dataVersion = loadFromStorage("dataVersion", "");
  if (verCompare(dataVersion, "X.Y.Z") >= 0) return;   // 已升过就跳过
  // ... 只补缺失项，不覆盖用户已有数据 ...
  saveToStorage("dataVersion", "X.Y.Z");
  console.info("[vX.Y.Z] ...");
})();
```

---

## 四、编码约定

- 严格模式（`"use strict"`）全程开启。
- 逻辑集中在 `js/app.js`，数据集中在 `js/data.js`；**计算与渲染分离** ——
  核心是纯函数 `calculate(inputs)`，不要在计算里操作 DOM。
- 计价相关的判断统一走既有辅助函数：
  `hasExactTier()`（档位是否有价）、`currentPriceListBleeds()`（是否出血）、`isFreeRope()`（免费吊绳）。
- 新增报价表级字段时，**必须同时写迁移**（`PRICE_LISTS` 存在 localStorage，老用户没有新字段）。
- 用户可见文案用「亲成」；`KOKA` / `tagPricing_` / `koka*` 等内部标识符**保持原名**。

---

## 五、已知高危模式（务必规避）

| 模式 | 后果 | 规避 |
|---|---|---|
| 改 HTML `id` 但不同步 JS | `getElementById` 返回 null，被 `if (els.x)` 静默吞掉 → "点击无反应" | 改 id 后全局 grep 旧名 |
| 重渲染 DOM 后忘记重绑事件 | 该区域交互全部失效 | 重渲染 = 重绑，封装成同一个函数 |
| 重渲染时用默认值覆盖用户当前选择 | 用户的选择被悄悄改掉，且后续计算基于错误值 | 重渲染前记住当前值，渲染后还原 |
| 用 `0` 表示"未设值" | 「合法的 0 价」被误判为缺价 | 用 `null` 表示缺失，`0` 表示真实价格 0 |
| 移动/删除 DOM 后不校验标签配对 | 结构错乱 | 做标签栈配对校验 |
| 用 `sed` 改版本号 | 静默失败 | 用 python 精确替换 |

---

## 六、验证方式

**优先用 jsdom**，不要用 Edge headless：

```bash
node ../_verify/v1080.test.cjs
```

### 为什么不用 Edge headless
`--dump-dom` 在 **DOMContentLoaded 就快照**，body 末尾的内联探针脚本**根本不执行**；
`--screenshot` + `--virtual-time-budget` 同样不跑 → 会把**正常代码误判成"功能坏了"**。

### jsdom 要点
1. **必须在同一进程内起 http 服务器**，用 `JSDOM.fromURL(...)`；
   直接传 HTML 字符串或假 `url:` → 子资源（`js/*.js`、`css/*.css`）全部 404。
2. 需要 stub `window.fetch` 与 `XMLHttpRequest`（`auth.js` 会请求 `/api/*`，本机无 Functions）。
3. **顶层 `let`/`const` 不挂 `window`** —— `window.ROPE_CONFIG` 恒为 `undefined`，
   要用 `window.eval("ROPE_CONFIG[0]")` 在页面上下文取值（`function` 声明才会挂 window）。

### 静态检查清单
- [ ] `node --check` 前端 JS（`js/*.js`）+ `functions/**/*.js` 全部通过
- [ ] 全项目 CRLF = 0
- [ ] `els` 悬空引用 = 0（改过 id 后必查）
- [ ] HTML 标签栈配对
- [ ] 清理临时脚本，确认无凭据残留

---

## 七、提交与发布

提交用 **plumbing**（本机 `git merge` 会 SIGTERM 挂起）：

```bash
git add -A
TREE=$(git write-tree)
NEW=$(git commit-tree "$TREE" -p <parent_sha> -F "E:/报价系统7.9/_msg.txt")
printf '%s\n' "$NEW" > .git/refs/heads/main
printf '%s\n' "$NEW" > .git/refs/tags/vX.Y.Z
git pack-refs --all
```

- 提交信息文件**必须用 Windows 原生路径**（原生 `git.exe` 不认 MSYS 的 `/tmp`）。
- 所有写 `.git` 的操作需**关闭沙箱**（否则 `.git/refs/**` 的写入会被静默丢弃）。
- 推送前先读 [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md) 的网络注意事项。

发布顺序：**改版本号 → 写 CHANGELOG → 跑测试 → 提交推送 → 生成干净部署源 → 部署 → 线上校验**。

---

## 八、凭据

- **不要把 token / 密码写进代码或注释**。
- 仓库为私有，但提交前仍应确认无凭据残留：
  `grep -rl "ghp_\|cfut_" . --exclude-dir=node_modules`
- 历史凭据（曾出现在对话或临时脚本中的）**一律视为已泄露**，用后即轮换。
