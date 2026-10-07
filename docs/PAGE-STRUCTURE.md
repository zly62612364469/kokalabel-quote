# PAGE-STRUCTURE.md · 页面结构与关键 DOM 锚点

> 当前版本 v10.8.0 ｜ 更新 2026-10-07
> 本项目只有 **3 个视图**（单页切换），没有独立的"页面"与"组件详情页"。

---

## 一、视图切换机制

侧边导航按钮带 `data-page` 属性，点击切换 `<section class="page">` 的 `active` 类：

```html
<nav class="sidenav">
  <button class="nav-btn active" data-page="calculator">智能报价计算器</button>
  <button class="nav-btn"        data-page="table">报价表组查询</button>
  <button class="nav-btn"        data-page="profile">个人主页</button>
</nav>
```

| `data-page` | 容器 id | 默认 |
|---|---|---|
| `calculator` | `#page-calculator` | ✅ 显示 |
| `table` | `#page-table` | |
| `profile` | `#page-profile` | |

另有全局浮层：
- `#personalizePanel` —— 个性化面板（**内联下拉，非弹窗、无遮罩**，点外部关闭）

---

## 二、视图 1：智能报价计算器（`#page-calculator`）

主入口，左侧参数区 + 右侧结果区。

### 参数区（自上而下）

| 区块 | 关键 id | 说明 |
|---|---|---|
| 报价表快捷切换 | `#quickPriceListSelector` | 顶栏快捷下拉 |
| 纸张数量 / 档位 / 地区 | `#sheetCount` `#tier` `#region` | |
| 纸张卡片 | `#sheetsWrap`（动态） | 每张纸独立选材质/尺寸/工艺 |
| 纸张选择器 | — | 下拉 + 数字分页（`#paperPageInfo`） |
| 吊绳类型 | `#ropeFold` `#ropeFoldHead` `#ropeFoldSummary` `#rope` | **可折叠**，默认折叠；`#rope` 内为单选 |
| 直接系数模式 | `#customCoeffBar` `#customCoeffInput` | 直接系数模式专用 |

### 结果区

| 区块 | 关键 id | 说明 |
|---|---|---|
| 报价结果卡片 | — | 标题「报价结果」 |
| 各纸张明细表 | `#resSheetTable` | 显示代码 `▲▼` 切换器（`onCodeSwitch`） |
| 吊绳费 | `#resRopePrice` `#ropePriceWrap` `#ropePriceTable` | |
| 邮费 | `#resShipping` `#shippingPriceWrap` `#shippingPriceTable` | |
| 面积系数行 | `#areaCoefficientRow` | 面积 > 10000 时显示 |
| 价格表（可编辑） | `#priceTable` `#craftTable` `#directCoeffTable` `#batchDirectTable` | **点单元格就地编辑** |
| 保存面板 | `.quote-save-panel` | 报价历史 / 快照 |
| 自定义价格卡 | `#customPriceCard` | 临时系数 |

---

## 三、视图 2：报价表组查询（`#page-table`）

查看与维护价格矩阵。

| 区块 | 关键 id | 说明 |
|---|---|---|
| 报价表切换器 | `#priceListSelector` `#deletePriceListBtn` | |
| **报价表备注**（v10.8.0） | `#priceListNote` `#priceListNoteBody` | 独立框体，位于切换器下方；无备注整块 `hidden` |
| 搜索框 | `#searchInput` | 按代码或面积搜索 |
| 纸张信息 | `#paperDiscount` `#tableMeta` | |
| 纸张分页 | `#paperPageInfo` | |
| 价格矩阵表 | `#priceTable` `#craftTable` `#directCoeffTable` `#batchDirectTable` | 内联编辑 |
| 新增规格行 | `#addSpecRowBtn` | |
| Excel | `#exportExcelBtn` `#importExcelBtn` `#importExcelFile` `#downloadTemplateBtn` `#excelImportStatus` | 纸张表 |
| 吊绳/邮费表 | `#ropePriceTable` `#shippingPriceTable` + 各自导入导出按钮 | |

---

## 四、视图 3：个人主页（`#page-profile`）

| 区块 | 关键 id | 说明 |
|---|---|---|
| 仓库链接 | — | |
| 报价设置 | `#defaultPaper` `#defaultRope` `#defaultTier` `#defaultSizeType` `#defaultQuoteToggle` `#hidePaperIndex` `#decimalPlaces` | |
| 公司信息 | `#companyName` `#companyPhone` | |
| 计算规则说明 | — | 静态说明文本 |
| **数据管理** | `#uploadDefaultBtn` | ★「上传为默认数据」（仅管理员可见） |
| 快照与统计 | `#createSnapshotBtn` `#exportSnapshotsBtn` `#clearSnapshotBtn` `#exportStatsBtn` | |
| 报价历史 | `#historyTable` `#historyEmpty` `#clearHistoryBtn` `#historyDetailDialog` | |
| 导入导出 | `#exportDataBtn` `#importDataBtn` `#exportLocalBackupBtn` `#importLocalBackupBtn` | |
| 更多功能 | — | 占位功能入口 |
| 账号管理 | — | 仅超管可见 |

### 个性化面板（`#personalizePanel`）

| 元素 | 说明 |
|---|---|
| `#fontScaleGroup` | 预设按钮（小 / 默认 / 大 / 特大） |
| `#fontScaleInput` `#fontScaleApply` | 手输 px（8~32） |
| `#fontScaleCurrent` | 「当前字号：16px（100%）」 |
| `#fontScaleDefaults` | 系统默认字号 |
| `#fontScaleReset` | 恢复默认 |

---

## 五、⚠️ 改 DOM 时的强制检查

`js/app.js` 顶部有一个集中的 `els` 对象，形如：

```js
const els = {
  rope: document.getElementById("rope"),
  personalizePanel: document.getElementById("personalizePanel"),
  priceListNote: document.getElementById("priceListNote"),
  // ...
};
```

**任何 HTML `id` 的改动都必须：**

1. 全局 grep 旧 id —— 确认 `app.js` 里没有残留引用
2. 同步更新 `els` 中的对应项

否则 `getElementById` 返回 `null`，被 `if (els.xxx)` 静默吞掉，
表现为**「点击没反应」「功能卡死」**且**控制台不报错** —— 这是本项目最隐蔽的一类 bug
（v10.2 就因此产生了"个性化面板卡死"，直到 v10.7.1 才定位）。

> 排查技巧：先确认"元素到底存不存在"，再怀疑逻辑。

---

## 六、DOM 生成方式

- **静态**：`index.html` 里写死骨架（导航、卡片容器、固定表单）
- **动态渲染**：`app.js` 的 `render*` 函数用模板字符串写入 `innerHTML`
  （如 `renderPriceTable()` / `renderSheets()` / `renderRopeRadios()`）
- ⚠️ **`innerHTML` 重写 = 该区域内所有事件监听器全部丢失**，
  重渲染后必须重新绑定（统一封成 `rebuild*` 函数，内含 render + bind）。
