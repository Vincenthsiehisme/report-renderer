# Visual Reference 系統

> **讀取時機**：Phase 2 視覺決策前。<br>
> **適用範圍**：`report-renderer` repo 的 PDF / HTML 視覺系統決策。<br>
> **目標**：在開始製作頁面前，鎖定整份報告的視覺系統，確保每頁風格一致、有主張、不飄移。<br>
> **邊界**：本文件只處理視覺系統決策，不處理分析方法、論證圖片蒐集或其他 repo 的 phase 流程。

## 目錄
1. [執行時機](#執行時機)
2. [Phase 2 視覺決策流程](#phase-2-視覺決策流程)
   - Step 1：案例特性萃取
   - Step 2：視覺系統決策（色彩 / 字體 / 版型 / 靜態節奏）
   - Step 3：Visual Brief 內部記錄格式
3. [視覺 Ref 搜圖規則](#視覺-ref-搜圖規則)
4. [視覺 Ref 的分工邊界](#視覺-ref-的分工邊界)

---

## 執行時機

**在 `SKILL.md` 的 Phase 1 驗證完成後、Phase 3 渲染開始前，執行本文件。**

這不是可選步驟。視覺系統必須在內容進入輸出製作前鎖定。<br>
**AI 自主決定視覺系統，不輸出 Visual Brief，不等使用者確認，直接進入渲染。** 視覺系統決策記錄寫入來源附錄。

**Sprint Mode 的執行範圍：** 只需完成色彩系統與字體配對兩個維度；版型節奏與靜態節奏在製作時依輸出規格即時決定。

---

## Phase 2 視覺決策流程

### Step 1｜案例特性萃取（決策輸入）

從已完成的分析內容中，萃取以下四個維度的方向輸入：

| 維度 | 要問的問題 | 輸出 |
|------|----------|------|
| 產業調性 | 這個案例的品牌屬於哪個象限？（奢華 / 大眾 / 專業 / 親民） | 一個形容詞 |
| 情緒基調 | 這份報告讀完應該讓人感覺到什麼？（銳利 / 沉穩 / 張力 / 優雅） | 一個形容詞 |
| 讀者身份 | 這份報告的主要讀者是誰？（策略長 / 客戶高層 / 內部團隊） | 一個身份標籤 |
| 報告主張 | 這份報告整體想說的核心判斷是什麼？ | 一句話 |

**視覺主張句格式：**
> 這是一份給 [讀者身份] 看的報告，視覺語言要傳遞 [情緒基調形容詞] 的感受，對應 [產業調性形容詞] 的品牌調性，核心主張是 [報告主張]。

這句話是後續色彩、字體、版型決策的統一錨點。

---

### Step 2｜視覺系統決策（四個維度）

#### 維度 A｜色彩系統

**決策邏輯：**
- 奢華 / 專業類 → 偏深色背景（深炭 / 深藍 / 深綠），亮色文字
- 大眾 / 親民類 → 偏淺色背景，有重量感的深色文字
- 情緒偏銳利張力 → 高對比配色，accent 色要刺點
- 情緒偏沉穩優雅 → 低對比主色，accent 色克制但精準

**每份報告必須定義五個顏色：**
- `bg_primary`
- `bg_secondary`
- `text_primary`
- `text_secondary`
- `accent`

**禁止：**
- 不用 5 個以上的顏色
- accent 色不能超過總視覺面積的 10%
- 不在同一份報告裡混用暖色調和冷色調主色
- 不為了「設計感」犧牲 `estimated` / `unknown` 的辨識度

#### 維度 B｜字體系統

PDF 路徑使用 system font stack，不使用外部字體 CDN。

**推薦字體配對（依情緒基調）：**

| 情緒基調 | Display | Body | 特質 |
|---------|---------|------|------|
| 銳利張力 | Impact / 'Arial Narrow' | Arial | 壓縮感、力道強 |
| 沉穩優雅 | Georgia / Palatino | 'Helvetica Neue', Arial | 古典感、有重量 |
| 專業精準 | Trebuchet MS | 'Helvetica Neue', Arial | 現代感、乾淨 |
| 高冷奢華 | 'Palatino Linotype', Palatino | Georgia | 時尚感、留白多 |

**PDF 字體層級結構：**
- Display：36px / 700
- Heading：13px / 600 / uppercase
- Body：11px / line-height 1.7
- Caption：9px / opacity 0.5

#### 維度 C｜版型節奏

整份 deck 至少要有三種版型節奏，不能每頁都是同一種配置。

| 版型類型 | 適用頁面 | 核心視覺邏輯 |
|---------|---------|------------|
| Statement page | Case in one line、封面、章節分頁 | 大字佔滿版面，留白為主 |
| Analysis page | Real problem、Tension、Reframe | 主張區與佐證區有明確主從 |
| System page | The system | 水平流程 / 節點結構清楚 |
| Proof page | Why it lands | 已驗證事實與初步判讀必須分欄 |
| Takeaway page | What to steal | 條列清晰，每條方法有獨立視覺區塊 |

#### 維度 D｜靜態節奏

PDF 與靜態 HTML 不使用動態效果，節奏感改由以下手段建立：
- 色塊層次
- 字級對比
- 留白節奏
- accent 線條
- `estimated` badge / `unknown` 警示條等信心層級標示

---

### Step 3｜Visual Brief 內部記錄格式

完成四個維度決策後，AI 將以下格式記錄於來源附錄，不輸出給使用者：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VISUAL BRIEF｜[報告名稱]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

【情緒基調】[一句話描述這份報告的視覺主張]

【色彩系統】
  主背景：#______
  次背景：#______
  主文字：#______
  次文字：#______
  強調色：#______

【字體配對｜PDF / system font】
  Display：[系統字體名稱]
  Body：[系統字體名稱]

【版型節奏】
  Statement page → [具體描述]
  Analysis page → [具體描述]
  System page → [具體描述]
  Proof page → [具體描述]
  Takeaway page → [具體描述]

【靜態節奏手段】
  色塊交替：[說明哪些頁面用主背景 / 次背景]
  accent 強調：[說明 accent 色用在哪些元素]
  信心標示：[說明 estimated / unknown 如何露出]

【視覺禁止事項（本報告特定）】
  - [依案例特性列出 2–3 條不能做的設計選擇]
```

---

## 視覺 Ref 搜圖規則

視覺 ref 搜圖只用於**確認色彩組合是否符合既定情緒基調**。

- 字體選擇與版型決策不需要搜圖
- 搜圖是可選步驟，只在色彩感受不確定時執行
- 搜圖結果只作為色彩方向確認，不進最終頁面

**搜尋關鍵字模式（色彩確認專用）：**
- 深色背景：`dark charcoal interior minimal`、`deep navy texture abstract`
- 淺色背景：`warm cream linen texture`、`soft white minimal space`
- Accent 色：`gold accent detail`、`teal accent minimal`、`rust orange accent`
- 組合確認：`color palette [主色形容詞] [accent色形容詞]`

**失敗處理：**
- 若第一輪找不到合適參考，最多再試一輪不同關鍵字
- 第二輪仍無結果，直接回到文字判斷，不阻塞流程

---

## 視覺 Ref 的分工邊界

| 類型 | 目的 | 是否進入最終頁面 | 來源記錄 |
|------|------|----------------|---------|
| 視覺 ref | 確立設計語言，供 AI 製作時參考 | 否 | 不記錄 |
| 報告正式內容 | `report_data.json` 映射出的文字、來源、附錄 | 是 | 依 `render-mapping.md` 規則記錄 |

視覺 ref 只是暫時的設計錨點，不是最終內容的一部分。不能把視覺 ref 圖直接嵌入報告，也不能讓視覺 ref 覆蓋資料可信度標示規則。
