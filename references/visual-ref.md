# Visual Reference 系統

> 此文件由 `references/phases-4-7.md` Phase 6.5 引用。讀取時機：Visual Brief 視覺系統決策前。  
> 輸出形式：PDF（印刷版靜態 HTML 轉換）。  
> 目標：在 Phase 7 開始製作頁面前，鎖定整份報告的視覺系統，確保每頁風格一致、有主張、不飄移。  
> **本文件只處理視覺 ref（設計錨點用）。論證型配圖執行規則見 `references/writing-rules.md`。**

## 目錄
1. [執行時機](#執行時機)
2. [Phase 6.5 執行流程](#phase-65-visual-brief-執行流程)
   - Step 1：案例特性萃取
   - Step 2：視覺系統決策（色彩 / 字體 / 版型 / 動態）
   - Step 3：Visual Brief 內部記錄格式
3. [視覺 Ref 搜圖規則](#視覺-ref-搜圖規則)
4. [視覺 Ref 與論證型配圖的分工邊界](#視覺-ref-與論證型配圖的分工邊界)

---

## 執行時機

**Phase 6 批判完成後、Phase 7 開始前，插入 Phase 6.5：Visual Brief。**

這不是可選步驟。視覺系統必須在內容進入 PDF 製作前鎖定。  
**AI 自主決定視覺系統，不輸出 Visual Brief，不等使用者確認，直接進 Phase 7。** 視覺系統決策記錄寫入來源附錄。

**Sprint Mode 的執行範圍：** 只需完成色彩系統與字體配對兩個維度，版型節奏和動態節奏在製作時即時決定。

---

## Phase 6.5｜Visual Brief 執行流程

### Step 1｜案例特性萃取（決策輸入）

從已完成的分析內容中，萃取以下四個維度的方向輸入。  
這些不是憑空選擇，而是從案例本身的特性推導出來的：

| 維度 | 要問的問題 | 輸出 |
|------|----------|------|
| 產業調性 | 這個案例的品牌屬於哪個象限？（奢華 / 大眾 / 專業 / 親民） | 一個形容詞 |
| 情緒基調 | 這份報告讀完應該讓人感覺到什麼？（銳利 / 沉穩 / 張力 / 優雅） | 一個形容詞 |
| 讀者身份 | 這份報告的主要讀者是誰？（策略長 / 客戶高層 / 內部團隊） | 一個身份標籤 |
| 報告主張 | 這份報告整體想說的核心判斷是什麼？ | 一句話 |

這四個維度的組合，決定後續所有視覺選擇的方向。

**從四個維度合成視覺主張句（用於 Visual Brief 的「情緒基調」欄）：**
格式：「這是一份給 [讀者身份] 看的報告，視覺語言要傳遞 [情緒基調形容詞] 的感受，對應 [產業調性形容詞] 的品牌調性，核心主張是 [報告主張]。」

例如：「這是一份給策略長看的報告，視覺語言要傳遞銳利的感受，對應專業的品牌調性，核心主張是品牌在做的事比它說的話更值得被看見。」

這句話是後續色彩、字體、版型決策的統一錨點，每次做視覺決策時都對照這句話。

---

### Step 2｜視覺系統決策（四個維度）

#### 維度 A｜色彩系統

**決策邏輯：**

先確定主色調的「溫度」與「重量」：
- 案例品牌屬奢華 / 專業類 → 偏深色背景（深炭 / 深藍 / 深綠），亮色文字
- 案例品牌屬大眾 / 親民類 → 偏淺色背景，有重量感的深色文字
- 報告情緒偏銳利張力 → 高對比配色，accent 色要刺眼
- 報告情緒偏沉穩優雅 → 低對比主色，accent 色克制但精準

**色彩結構（每份報告必須定義以下五個）：**

```css
:root {
  --bg-primary: #______;      /* 主背景色，覆蓋 60-70% 視覺面積 */
  --bg-secondary: #______;    /* 次要背景，用於區塊分隔或卡片底色 */
  --text-primary: #______;    /* 主文字色 */
  --text-secondary: #______;  /* 次要文字色，用於說明文字、標注 */
  --accent: #______;          /* 強調色，用於關鍵判斷句、圖表亮點、頁碼 */
}
```

**禁止：**
- 不用紫色漸層配白底（過度使用的 AI 美學）
- 不用 5 個以上的顏色
- accent 色不能超過總視覺面積的 10%
- 不在同一份報告裡混用暖色調和冷色調主色

**搜圖方向（用於確認色彩方向感）：**
搜尋 `editorial magazine color palette [產業形容詞]` 或 `strategy deck dark minimal [情緒形容詞]`，找 2–3 張視覺參考確認色彩方向，搜到後通過證據檢查（見下方），才作為參考錨點。

---

#### 維度 B｜字體系統

**決策邏輯：**

PDF 路徑使用 system font stack，不使用 Google Fonts CDN（weasyprint 無法載入外部字體）。字體選擇遵循以下原則：

- **Display 字體**（用於頁面大標題、statement page）：選有個性的系統襯線體或幾何無襯線體
- **Body 字體**（用於內文、說明、標注）：選可讀性強的系統無襯線體，與 Display 字體形成對比

**推薦字體配對（依情緒基調，PDF / system font 版）：**

| 情緒基調 | Display | Body | 特質 |
|---------|---------|------|------|
| 銳利張力 | Impact / 'Arial Narrow' | Arial | 壓縮感、力道強 |
| 沉穩優雅 | Georgia / Palatino | 'Helvetica Neue', Arial | 古典感、有重量 |
| 專業精準 | Trebuchet MS | 'Helvetica Neue', Arial | 現代感、乾淨 |
| 高冷奢華 | 'Palatino Linotype', Palatino | Georgia | 時尚感、留白多 |

**PPTX 字體對照表（HTML Google Fonts 無法在 PPTX 使用，改用以下系統字體）：**

| 情緒基調 | PPTX Header 字體 | PPTX Body 字體 | 說明 |
|---------|----------------|--------------|------|
| 銳利張力 | Impact | Calibri | 壓縮感保留，力道最強 |
| 沉穩優雅 | Georgia | Calibri Light | 最接近 Playfair 的古典感 |
| 專業精準 | Trebuchet MS | Calibri | 現代感、乾淨 |
| 高冷奢華 | Palatino Linotype | Calibri | 最接近 Bodoni 的優雅感 |

**PDF 字體層級結構（固定 px，不用 clamp / vw，weasyprint 不支援）：**

```css
/* Display — 頁面主標題，statement page 大字 */
font-family: '[Display字體]', serif;
font-size: 36px;
font-weight: 700;
letter-spacing: -0.02em;

/* Heading — 分析頁小標、模型標題 */
font-family: '[Body字體]', sans-serif;
font-size: 13px;
font-weight: 600;
letter-spacing: 0.08em;
text-transform: uppercase;

/* Body — 內文段落 */
font-family: '[Body字體]', sans-serif;
font-size: 11px;
line-height: 1.7;

/* Caption — 來源標注、圖說、頁碼 */
font-family: '[Body字體]', sans-serif;
font-size: 9px;
letter-spacing: 0.12em;
opacity: 0.5;
```

**PPTX 字體大小規範（pptxGenJS 使用 pt 單位）：**

| 層級 | 用途 | 大小 | 樣式 |
|------|------|------|------|
| Display | 投影片主標題、statement slide 大字 | 40pt | bold |
| Heading | 分析頁小標、模型標題 | 20pt | bold、uppercase、charSpacing: 4 |
| Body | 內文段落 | 14pt | regular、lineSpacingMultiple: 1.4 |
| Caption | 來源標注、頁碼、eyebrow | 10pt | regular、charSpacing: 6 |
| Insight | Tension 頁的 insight 句 | 22pt | bold italic |
| Footer | 頁尾收束句 | 11pt | italic、accent 色 |

**PPTX 版型尺寸規範：**
- 投影片尺寸：`LAYOUT_WIDE`（13.3" × 7.5"），比 16:9 更寬，適合策略報告
- 邊距：左右 0.6"，上下 0.5"，導航列高度 0.4"
- 內容區域：從 y=0.9" 開始（導航列下方）

---

#### 維度 C｜版型節奏

**決策邏輯：**

整份 deck 至少要有三種版型節奏，不能所有頁面長得一樣。

**五種標準版型（每份報告選定每種版型的具體呈現方式）：**

| 版型類型 | 適用頁面 | 核心視覺邏輯 | PDF 佈局方向 |
|---------|---------|------------|--------------|
| Statement page | Case in one line、封面、章節分頁 | 大字佔滿版面，留白為主，圖片為背景層 | `height: 247mm; display: flex; align-items: center` + 背景色塊（不用背景圖，weasyprint 不穩定） |
| Analysis page | Real problem、Tension、Reframe | 左側文字 + 右側模型圖表，或上下分區 | CSS Grid `grid-template-columns: 1fr 1fr`；`page-break-inside: avoid` |
| System page | The system | 水平流程圖，靜態 SVG 呈現接觸點 | Flex row + 靜態 SVG；不用圖片嵌入節點 |
| Proof page | Why it lands | 數據大字突出，佐證文字為輔 | 數字用 Display 字體放大，說明文字縮小對比 |
| Takeaway page | What to steal | 條列清晰，每條方法有獨立視覺區塊 | 卡片式佈局，每張卡片有 accent 色左邊框；`page-break-inside: avoid` |

**排版禁止事項：**
- 不用置中對齊做主要版型（過於對稱，缺乏張力）
- 不讓所有頁面都是「左文右圖」的同一模式
- Statement page 的文字不能超過版面的 60%，留白是設計的一部分

---

#### 維度 D｜動態節奏

**PDF 路徑不適用動態節奏。** CSS 動畫在靜態 PDF 中無效，weasyprint 會忽略所有 `@keyframes` 和 `animation` 屬性。

PDF 的「節奏感」改由以下靜態手段建立：
- **色塊層次**：用 `--bg-primary` / `--bg-secondary` 交替區塊製造視覺分隔
- **字級對比**：Display 大字 vs. Caption 小字的落差創造張力
- **留白節奏**：每個 `.page` 內元素之間的 margin 一致，封面頁留白更多
- **accent 線條**：用 `border-left: 3px solid [accent色]` 取代動畫強調重點句

---

### Step 3｜Visual Brief 內部記錄格式

完成四個維度決策後，AI 將以下格式記錄於來源附錄，不輸出給使用者，直接進 Phase 7：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VISUAL BRIEF｜[報告名稱]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

【情緒基調】[一句話描述這份報告的視覺主張]

【色彩系統】
  主背景：#______ （[顏色名稱]，[視覺感受]）
  次背景：#______
  主文字：#______
  次文字：#______
  強調色：#______ （用於[具體用途]）

【字體配對｜PDF / system font】
  Display：[系統字體名稱]（用於[具體用途]）
  Body：[系統字體名稱]（用於[具體用途]）

【版型節奏】
  Statement page → [具體描述]
  Analysis page → [具體描述]
  System page → [具體描述]
  Proof page → [具體描述]
  Takeaway page → [具體描述]

【靜態節奏手段】
  色塊交替：[說明哪些頁面用主背景 / 次背景]
  accent 強調：[說明 accent 色用在哪些元素]

【視覺禁止事項（本報告特定）】
  - [依案例特性列出 2–3 條這份報告不能做的設計選擇]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━
視覺系統已鎖定，直接進入 Phase 7 製作。
```

---

## 視覺 Ref 搜圖規則

視覺 ref 的搜圖邏輯與論證型配圖完全不同。

**目的收窄：只用於確認色彩組合的視覺感受。**

字體選擇和版型決策不需要搜圖——AI 可以直接依情緒基調決策。搜圖只在「選定色彩系統後，需要確認這個色彩組合在實際視覺上是否符合情緒基調」時才執行。

**PPTX 路徑不需要執行此步驟。** 系統字體（Georgia、Impact、Palatino 等）的視覺感受 AI 本身已知，色彩確認邏輯與 PDF 路徑相同，搜圖規則一致。

---

**搜尋關鍵字模式（色彩確認專用）：**

搜尋對象是能呈現「色調氛圍」的空間、物件、或抽象色彩圖，不搜版面設計或字體海報。

| 目的 | 關鍵字範例 |
|------|----------|
| 深色背景色調確認 | `dark charcoal interior minimal`、`deep navy texture abstract` |
| 淺色背景色調確認 | `warm cream linen texture`、`soft white minimal space` |
| Accent 色感受確認 | `gold accent detail`、`teal accent minimal`、`rust orange accent` |
| 整體色彩組合確認 | `color palette [主色形容詞] [accent色形容詞]` |

**禁止搜尋（觸發內容限制或搜不到有效結果）：**
- `editorial magazine layout`、`magazine spread`（版權內容）
- `typography poster [字體名稱]`（對設計決策無幫助）
- `strategy presentation layout`（搜到的是 stock mockup，無參考價值）
- `annual report design`（版權內容）

---

**搜圖執行流程：**

**Step 1｜先用文字描述色彩方向**
在搜圖之前，先寫出色彩組合的文字判斷，例如：
> 「主背景 #1a1a1a（深炭黑）配 accent #e8c547（暖金），視覺感受應該是沉穩但不沉悶，金色給予呼吸感。」

若文字判斷已足夠確定，可以直接跳過搜圖，進入 Visual Brief 輸出。

**Step 2｜只有不確定色彩感受時才搜圖**
選定搜尋關鍵字（參考上方表格），執行 image search。搜到後用以下問題判斷：

1. 這張圖的色調，和剛才文字判斷描述的感受吻合嗎？
2. 這張圖是否確認了 accent 色在深色背景上的實際視覺比例感？
3. 這張圖不涉及任何品牌版權或可識別的設計作品？

三個問題全部通過才作為參考錨點。

**Step 3｜失敗處理**
搜不到合適的圖，或搜到的圖無法通過問題 1–3，最多再試一輪不同關鍵字。第二輪仍無結果，直接回到文字判斷，不強迫配視覺 ref。搜圖失敗不阻塞流程。

---

**視覺 ref 圖片的用途限制：**
- 只作為色彩決策的確認錨點，看完即棄
- 不嵌入報告頁面
- 不作為論證配圖使用
- 不出現在 Image Source Appendix

---

## 視覺 Ref 與論證型配圖的分工邊界

| 類型 | 目的 | 是否進入最終頁面 | 來源記錄 |
|------|------|----------------|---------|
| 視覺 ref | 確立設計語言，供 AI 製作時參考 | 否 | 不記錄 |
| 論證型配圖 | 佐證分析論點，讓讀者更相信某個判斷 | 是 | 必須記錄於 Image Source Appendix |

兩種圖的搜圖在不同步驟執行：
- 視覺 ref → Phase 6.5，在內容確認後、PDF 製作前
- 論證型配圖 → Phase 7，在頁面製作過程中逐頁處理

不能把視覺 ref 的圖直接拿來當論證配圖用。如果視覺 ref 搜到的圖恰好也能作為論證使用，必須重新走論證配圖的證據檢查流程，確認後才能嵌入。
