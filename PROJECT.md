# 醫療通脹計算器 — Project Plan

**啟動日**：2026-05-07
**重大簡化**：2026-05-07（同日）— 由「醫療融資（含 saving plan 模擬）」改為「純醫療通脹可視化」
**目標用戶**：保險 team 內部（幾個熟同事）
**用途**：客戶面前 demo「**通脹點樣食人**」嘅醫療總開支

---

## 🎯 核心概念

純粹用通脹複利公式，計算客戶終身醫療開支總和。

**回答嘅問題**：
- 客戶 X 歲到 Y 歲累計醫療保費係幾錢？
- 通脹幫你食咗幾錢（vs 假設 0% 通脹）？
- 最後一年保費比首年貴幾多倍？

**Saving plan 配置由顧問同客戶傾**（用保險公司 broker portal），呢個 tool **唔做** cash value 計算。

---

## 📦 Scope

### MVP（已完成）
- 4 個醫療 plan，dropdown 揀
- 客戶資料：年齡 / 目標年齡 / 性別
- 醫療通脹率 slider（0-12%）
- Hero card：客戶終身醫療總開支（大字）
- Stat cards：首年保費 / 最後一年保費（含倍數）/ 通脹額外開支
- 兩張圖（Chart.js，即時反應）：
  - 圖 1：每年醫療保費（含通脹 vs 無通脹 baseline）
  - 圖 2：累計醫療開支（含通脹 vs 無通脹 baseline）
- Footer disclaimer

### 已移除（簡化決定）
- ~~儲蓄 plan dropdown~~
- ~~Cash value 計算~~
- ~~Partial surrender model~~
- ~~槓桿倍數~~

### v2（如果有需要）
- PDF / 圖片 export（客戶帶走）
- 多醫療 plan 並排對比
- 客戶版（動畫、品牌色、講解文字）
- 手機 responsive 優化

---

## 🛠️ 技術選擇

- **單一 `index.html`**（內嵌 CSS + JS）
- **Chart.js**（CDN）出圖
- 數據 hardcode 喺 `data/*.js`
- 開 browser 直接用，可放 iCloud Drive 比 team share

---

## 🧮 計算 Model：純通脹複利

```
每年保費 = 基本保費 × (1 + 通脹率)^年數
累計開支（含通脹） = 加總全部年保費
累計開支（無通脹） = 基本保費 × 年數
通脹額外開支 = 含通脹 - 無通脹
```

**100% 準確**，純算術，冇 actuarial assumption。

## 📜 設計演化（為咩變成而家咁）

1. **初版設計**：完整醫療融資工具，包括 saving plan partial surrender 模擬
2. **問題**：要保險公司 quote「baked-in withdrawal」嘅 illustration，broker portal 唔一定支援
3. **嘗試簡化**：用 proportional share model 自己 simulate（85% accurate）→ 用戶拒絕「唔接受唔準」
4. **最終 reframe**：拎走 saving plan 部分，純 focus 醫療成本可視化
   - Tool 角色變成 sales hook（嚇人嘅累計數字）
   - Saving plan advice 由顧問同客戶傾
   - 100% 準確，工作量大減

---

## 📁 檔案結構

```
醫療融資工具_2026-05-07/
├── index.html              ← 單檔 MVP
├── data/
│   └── medical-plans.js    ← 4 個醫療 plan
├── README.md               ← 點用 + 點加新 plan 數據
└── PROJECT.md              ← 呢份 plan 文件
```

---

## 📊 你要準備嘅資料

### 醫療 plan（4 個）
每個 plan 提供：
- Plan 名稱（例：BUPA Top Care）
- 唔同年齡保費表：30 / 40 / 50 / 60 / 70 / 80 / 90 歲，男 / 女
- 數字係**現時**該年齡嘅當年保費，唔需要加通脹（tool 自己 compound）

---

## 🚀 開發步驟

1. ✅ 建 project folder
2. ✅ 寫 PROJECT.md
3. ⏳ 寫示範數據檔（用假數據，比烈文驗證 UI/UX）
4. ⏳ 砌 index.html 骨架 + 算法 + 圖表
5. ⏳ 寫 README.md
6. 烈文驗收 demo（用假數據）→ 調 UI / 文案
7. 烈文 paste 真實 plan 數據 → 替換 `data/*.js`
8. 驗證計算結果合理（對比保險公司 quote）
9. Done，team 可以用

---

## ⚠️ Disclaimer（要寫入 footer）

> 本工具為內部初步評估；cash value 計算為近似模型，實際數字以保險公司正式 illustration 為準。
> 結果不構成投資建議或保險銷售文件。
