# Report Renderer Skill

**版本：1.1 ｜ 更新：2026-03-23**

把 `report_data.json` 渲染成指定格式報告的專用 skill。單一職責，不分析，不補填，只渲染。

---

## 這個 skill 做什麼

strategy-case-report skill 完成分析後輸出截點 JSON，本 skill 接手把 JSON 渲染成最終報告。

```
strategy-case-report   →   report_data.json   →   report-renderer
    分析 context              截點                    輸出 context
```

分工的好處：分析和輸出各自在乾淨的 context 裡執行，版型規則的注意力不被分析內容稀釋。同一份 JSON 也可以多次渲染成不同格式。

---

## 安裝方式

### Claude.ai
1. 下載 `report-renderer-v1.zip`
2. 前往 Claude.ai → Settings → Customize → Skills
3. 上傳 ZIP

### Claude Code
```
your-project/
└── .claude/
    └── skills/
        └── report-renderer/
            ├── SKILL.md
            └── references/
```

---

## 使用方式

1. 從 strategy-case-report skill 的第一段取得 `report_data.json`
2. 開啟新對話
3. 貼入 JSON，說明輸出格式：

> 「幫我把這份 JSON 渲染成 16:9 Slide PDF」

或直接在 JSON 的 `meta` 欄位指定輸出媒介，貼入後說「渲染」即可。

---

## 輸出路徑

| 路徑 | 說明 | 觸發關鍵字 |
|------|------|-----------|
| A1 | A4 印刷報告 PDF | 「A4」「印刷版」「閱讀版」|
| A2 | 16:9 簡報 PDF | 「slide」「簡報」「投影」「提案用」|
| B | HTML artifact | 「HTML」「互動版」|
| D | 純文字稿 Markdown | 「純文字」「文字稿」|

---

## 三欄狀態的視覺呈現

report-renderer 的核心功能是把分析的信心層級誠實地呈現在報告裡：

| 狀態 | 說明 | 視覺處理 |
|------|------|---------|
| `confirmed` | 有直接資料支撐 | 正文，無標注 |
| `estimated` | 代理推論，有假設條件 | 正文 + 「初步判讀」標注 |
| `unknown` | 目前無法判斷 | 不進正文，移至來源附錄 |

讀者能從視覺層級直接看出哪些判斷是確定的、哪些是有條件的、哪些是分析的盲區。

---

## 檔案結構

```
report-renderer/
├── SKILL.md                    # 主流程（三個 Phase + Hard Rules）
└── references/
    ├── versioning.md           # 版本角色、優先序、配套關係
    ├── render-mapping.md       # JSON → 頁面元素映射規則（本 skill 核心）
    ├── output-a4.md            # A4 版型規格（路徑 A1）
    ├── output-slide.md         # 16:9 Slide 版型規格（路徑 A2）
    ├── visual-ref.md           # 視覺系統決策規格
    ├── plain-text-format.md    # 純文字稿格式（路徑 D）
    └── report-data-schema.md   # JSON 結構定義（供驗證用）
```

---

## 版本變更記錄

| 版本 | 日期 | 主要變更 |
|------|------|---------|
| v1.1 | 2026-03-23 | 補強文件治理：優化 Slide spec、收斂 visual-ref scope、移除失效引用、統一 report-renderer 內部版本與配套說明。 |
| v1.0 | 2026-03-23 | 初始版本。三 Phase 流程（驗證 / 視覺決策 / 渲染）；render-mapping.md 定義完整 JSON → 頁面映射規則；三欄狀態渲染規則；Recap 頁扎根強度 icon；結尾頁知識邊界聲明 |
