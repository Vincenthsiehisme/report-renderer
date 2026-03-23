# A4 印刷報告版型規格

> 此檔案在 Phase 6.5 決定輸出媒介為 A4 時、或 Phase 7 路徑 A1 執行前讀取。

---

## 報告結構

整份報告包含：封面 → 方法頁 → 案例總覽頁 → 3 案 × 7 頁分析 → 對照 Recap 頁 → 結論頁 → 來源附錄
**總頁數預估：27–30 頁**

每案 7 頁固定結構：
1. Case in one line（含關鍵 metrics）
2. The real problem
3. The tension
4. The strategic reframe
5. The system
6. Why it lands
7. What to steal

---

## 頁面容器

```css
.page {
  width: 170mm;        /* A4 扣除左右邊距 */
  height: 297mm;
  padding: 0;
  page-break-after: always;
  overflow: visible;   /* A4 允許內容自然流動 */
}
/* 可用內容高度：297mm - 18mm（上）- 20mm（下）= 259mm */
/* 實際安全高度：247mm（保留底部緩衝） */
@page { size: A4; margin: 18mm 20mm; }
```

---

## 字體層級

| 層級 | 用途 | 字體大小 | 字重 |
|------|------|---------|------|
| 封面主標題 | 封面 | 26–30px | 800 |
| 頁面標題 | 每頁 sh-title | 16–18px | 800 |
| Insight box 文字 | 核心主張句 | 10–11px | 700 |
| Card body | 內文 | 8.5–10px | 400 |
| Eyebrow / Label | 小標籤 | 7.5–8px | 600–700 |
| Source note | 來源標注 | 7px | 400 |
| Metric 數字 | 關鍵數字 | 16–22px | 800 |

---

## 禁止

- CSS variable（WeasyPrint 不支援）
- 外部字體（使用 system font stack）
- `position: fixed`
- `min-height: 100vh`

跨頁風險元素加 `page-break-inside: avoid`：表格、圖表、steal card、metric row。

---

## 非案例頁規格（A4）

**封面**：左右雙欄，左側主標題 + 決策情境 + 說明文，右側案例列表。
必含：報告主題、決策情境、案例列表（品牌名 + 分析視角）、版本日期。

**方法頁**：說明三案如何對應決策問題的不同層次。可高密度文字 + 架構圖。

**案例總覽頁**：三個 card 縱向排列，每案說明：它解決的真實問題 + 對決策有用的理由 + 在三案視角結構中的角色。

**對照 Recap 頁**：六維度橫向比較表（真實問題 / 策略選擇 / 關鍵動作 / 借用點 / 前置條件 / 最快第一步）。白色背景完整表格。

**結論頁**：三階段行動框架（Phase 1 / 2 / 3）+ 核心主張句。phase stack 佈局 + 深色 final statement box。

**來源附錄**：所有引用來源 + 批判執行摘要 + 視覺決策記錄。單頁，雙欄。
