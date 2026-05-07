# 醫療通脹計算器

內部 sales tool：客戶面前可視化「**通脹點樣食人**」嘅醫療總開支。

---

## 🎯 用途

呢個 tool 幫你喺客戶面前展示：
- 「你由今日到 X 歲，醫療總開支係幾錢？」
- 「通脹幫你食咗幾錢？」
- 「最後一年保費比首年貴幾多倍？」

**Saving plan 配置由顧問同客戶傾**（用保險公司 broker portal 出 quote），呢個 tool **唔做** cash value 計算。

---

## 🚀 點開

雙擊 `index.html` → 用 browser 打開。

唔需要 server，唔需要 npm install，純 HTML + JS。

可以放 iCloud Drive 比 team share。

---

## 📊 點用

1. 輸入客戶年齡 / 目標年齡 / 性別
2. 揀醫療 plan
3. 拉動醫療通脹率 slider（0-12%）
4. 圖表 + 數字即時更新

---

## 🛠️ 點替換真實 plan 數據

打開 `data/medical-plans.js`，將 `MEDICAL_PLANS` 入面嘅示範 plan 替換為真實數據：

```js
'plan_a': {
  id: 'plan_a',
  name: '你嘅 Plan 名',
  premiums: {
    male:   { 30: XXXX, 40: XXXX, 50: XXXX, 60: XXXX, 70: XXXX, 80: XXXX, 90: XXXX },
    female: { 30: XXXX, 40: XXXX, 50: XXXX, 60: XXXX, 70: XXXX, 80: XXXX, 90: XXXX },
  }
}
```

- 數字係**現時**該年齡嘅保費（HKD/年），唔需要計通脹（Tool 自己 compound）
- 年齡點越多越準（30/40/50/60/70/80/90 已經夠用）
- 中間年齡會 linear interpolate

---

## 🧮 計算原理

純通脹複利計算：

```
每年保費 = 基本保費 × (1 + 通脹率)^年數
累計醫療開支 = 加總全部年保費
通脹額外開支 = 含通脹累計 - 假設 0% 通脹累計
```

**100% 準確**，冇 actuarial assumption。

---

## ⚠️ Disclaimer

呢個工具只係**客戶開支推算工具**，**唔可以**用作：
- 保險公司正式 quote
- 保證未來保費
- 銷售文件

實際保費以保險公司年度公布為準。

---

## 📁 檔案結構

```
醫療融資工具_2026-05-07/
├── index.html              ← 主程式（打開呢個）
├── data/
│   └── medical-plans.js    ← 醫療 plan 數據
├── README.md               ← 呢份文件
└── PROJECT.md              ← 開發 plan
```

---

## 🧰 技術 stack

- 純 HTML + CSS + Vanilla JS
- Chart.js 4.4 (CDN)
- 唔需要 build / npm / server
