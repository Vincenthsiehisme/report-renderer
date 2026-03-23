# Output Slide Spec · 16:9 Slide PDF
version: "2.0"
updated: "2026-03-20"
paired-with: "strategy-case-report SKILL v2.3"

---

## 文件定位

本文件是 `strategy-case-report` SKILL v2.3 的配套規格文件。v2.0 為重大改版，引入**案例識別色帶系統**、**五種主從版型**、**深色 Steal 頁**，取代 v1.x 的頂部色條 + 等寬欄位設計。

**當本文件與 SKILL.md 說法有衝突時，以本文件為準。**

---

## 版本變更記錄

| 版本 | 日期 | 主要變更 |
|------|------|---------|
| v1.0 | 2026-03-20 | 初始版本 |
| v1.2 | 2026-03-20 | 中文硬規則、密度單位制、版型庫重構 |
| v2.0 | 2026-03-20 | **重大改版**：識別色帶取代頂部色條、五種主從版型取代等寬模式、深色 Steal 頁、Quote box 識別色化、Reframe 非對稱佈局 |

---

## 1. 頁面規格

```css
@page {
  size: 338mm 190mm;   /* 16:9，等比於 1920×1080 */
  margin: 0;
}

.slide {
  width: 338mm;
  height: 190mm;
  position: relative;
  overflow: hidden;          /* 硬規則：不可移除 */
  page-break-after: always;
  display: flex;
  flex-direction: row;       /* v2.0：改為 row，左側色帶為第一子元素 */
}
```

**`overflow: hidden` 是不可妥協的硬規則。** 內容塞不進去的唯一解法是拆頁。

---

## 2. 識別色帶系統（v2.0 核心變更）

### 2.1 設計原則

v1.x 使用頂部 4px 橫條作為案例識別，力道太弱——翻頁時讀者必須看文字才能確認案例。

v2.0 改為**左側縱向色帶**：每個內容頁的最左側有一條 `8mm` 寬的色帶，從頁頂延伸到頁底，顏色為該案例的識別色。視線掃到頁面左側即可定位案例，不需要讀文字。

### 2.2 色帶 HTML 結構

每個 Content 頁的 `.slide` 改為 `flex-direction: row`，第一個子元素是色帶，第二個子元素是內容區：

```html
<div class="slide" style="flex-direction:row;">
  <!-- 左側色帶（8mm，案例識別色） -->
  <div style="width:8mm; background:#3D7EC7; flex-shrink:0;"></div>
  <!-- 內容區（其餘寬度） -->
  <div style="flex:1; padding:7mm 12mm 6mm; display:flex; flex-direction:column; overflow:hidden;">
    <!-- 頁面內容 -->
  </div>
</div>
```

### 2.3 五案識別色定義

| 案例 | 識別色（亮） | 深色版（Steal 頁背景） | 用途 |
|------|------------|---------------------|------|
| Case 01 | `#3D7EC7`（鋼藍） | `#0D2A4A` | 色帶、Quote box 背景、Steal 頁底色 |
| Case 02 | `#2A9D6F`（翠綠） | `#0D3326` | 同上 |
| Case 03 | `#7B5EA7`（深紫） | `#2A1F42` | 同上 |
| Case 04 | `#D4602A`（燒橘） | `#4A1E0A` | 同上 |
| Case 05 | `#1E7E8C`（深青） | `#0A2E33` | 同上 |

**深色版 = 識別色加深約 60%，確保白色文字可讀（對比比率 ≥ 4.5:1）。**

### 2.4 深色頁不使用色帶

Cover、Section Divider、DarkFull-Recap、DarkFull-Insight、Steal 頁（深色底）均為**全版深色背景**，不加左側色帶。色帶只出現在白底內容頁。

---

## 3. 字體規格

Slide 讀者距離遠，閱讀時間短，字體規格與 A4 印刷版完全不同，**不可混用**。

### 3.1 字體層級表

| 層級 | 用途 | 字體大小 | 字重 | 備註 |
|------|------|---------|------|------|
| Display | 封面主標題、Section Divider 主標 | 26–34px | 800 | 最多 3 行；行距 1.15 |
| Title | 每頁標題 | 16–20px | 800 | 中文最多 16 字；超過拆副標 |
| Subtitle | Quote box / Insight box 主張句 | 11–13px | 700 | 中文最多 44 字 |
| Body | Card / Steal card 內文 | 10.5px | 500 | 見 3.2 節硬規則 |
| Label | Eyebrow、tag、欄位標籤 | 8–9px | 700 | 全大寫 + letter-spacing 1.5–3px |
| Source | 來源標注、初步判讀 | 8px | 500 | 色彩減淡，不使用 italic |
| Metric | 關鍵數字 | 18–26px | 800 | 必須搭配 Label 層級 |

### 3.2 中文硬規則（優先於上方層級表）

**字體大小下限：**
- 白底頁 body ≥ **10.5px**
- 深色背景頁 body ≥ **11px**
- Source 層級 ≥ **8px**
- 絕對最小值：8px。低於此值在投影場景不可辨。

**字體大小全報告統一，不允許同類元素在不同頁面使用不同字號。**  
Body 全報告固定 10.5px（白底）/ 11px（深色底），不依頁面空間調整。內容放不下的解法是拆頁。

**卡片密度上限：**
- 一張 card 最多 3 個語意段
- 一張 card 中文字數 > 40 字，必須拆卡或移至下頁

### 3.3 字體系統與字重補償

```css
font-family: "Helvetica Neue", Arial, "PingFang TC", "Microsoft JhengHei", sans-serif;
```

system font stack 中文渲染往往比預期細，補償方式：

| 傳統 spec | 中文補償後 |
|----------|----------|
| 400 | → **500** |
| 600 | → **700** |
| 700 | → **800** |

Body 固定 500，Label/Source 固定 700，不使用 400。

**Source 標注不使用 italic（對中文無效）。** 改用色彩減淡 + 字重對比：

```css
/* 深色背景頁 Source */
font-size: 8px; font-weight: 500; color: rgba(255,255,255,0.45);

/* 白色背景頁 Source */
font-size: 8px; font-weight: 500; color: #94A3B8;
```

---

## 4. 版面設計語言

### 4.1 印刷版 vs. Slide 根本差異

| 維度 | A4 印刷報告 | 16:9 Slide |
|------|------------|-----------|
| 閱讀方式 | 線性、逐段讀 | 掃視、主張先行 |
| 每頁資訊量 | 高密度 | 單一核心主張 |
| 段落文字 | 允許 5–8 行 | **禁止**，改用 card |
| 欄寬 | 可等寬 | **主從關係**，核心主張欄 > 佐證欄 |
| 色塊 | 點綴性 | 結構性，深色頁是常態 |
| 視覺重心 | 可居中 | 每頁應有明確重心位置（不全在頂部） |
| 頁面高度 | 297mm | 190mm，零 margin |
| overflow | visible | **hidden**（硬規則） |

### 4.2 深淺節奏規則（v2.0 新增）

整份報告的深淺節奏必須有設計意圖，不能讓深色頁只出現在「過場」。

**標準節奏（每案）：**
```
Section Divider（深藍）
→ Case in one line（白底 + 色帶）
→ Real problem（白底 + 色帶）
→ Tension / Reframe（白底 + 色帶）
→ System（白底 + 色帶）
→ Why it lands（白底 + 色帶）
→ Steal（識別色深底）← 每案的視覺句號，不加色帶
```

**Steal 頁必須是深色底**，不允許用白底。深色 Steal 頁是每案的收尾視覺節點，功能是「讓讀者感受到這個案例已完整，準備進入下一案」。

### 4.3 每頁資訊密度原則

每頁只允許**一個核心主張**，其餘為支撐元素。

| 支撐元素 | 說明 | 密度單位 |
|---------|------|---------|
| Metric strip | 大數字 + 短說明，3–4 個並排 | 1 |
| Short card | 有標題，內文 ≤ 1 個語意段 | 1 |
| Card | 有標題，內文 2–3 個語意段 | 1 |
| Steal card（橫條） | 三欄橫條：識別號 / 說明 / 執行 | 2 |
| Flow（階層式） | 1 個核心 + 3 個結果，上下層結構 | 2 |
| Quote box | 核心主張句，識別色背景 | 1 |
| Reframe pair（非對稱） | 舊說法（35%）vs. 新說法（65%） | 2 |
| Recap matrix | 跨案比較表格 | 4 |

**硬規則：每頁密度總和 ≤ 4 單位。** 超過必須拆頁。

---

## 5. 版型庫（v2.0 重構）

每頁必須選用以下版型之一。**不允許自行發明未定義的版型。**

選版型的判斷順序：先看頁面的**主體資訊形狀**，再看語義。

---

### Cover（封面）

- 背景：深藍 `#0B2244`，`flex-direction: row`
- 結構：左欄（60%）eyebrow + 主標題 + 說明文 + meta；右欄（40%）案例摘要列表
- 特徵：主標題 ≥ 26px；右欄背景 `#0D2A52`（略淺）製造層次；**無左側色帶**

---

### Section Divider（案例分隔頁）

- 背景：深藍 `#0B2244`，`flex-direction: column; justify-content: center`
- 結構：大型案例序號底紋（透明度 5–6%）+ eyebrow + 主標題 + 說明文 + 分析視角 badge
- 特徵：主標題 ≥ 26px；badge 用識別色實心背景；**無左側色帶**；每案 Section Divider 的說明文必須有個性，不能五案格式完全相同

**Section Divider 說明文個性化原則：**  
說明文（`sec-desc`）不只是「說明案例做了什麼」，而是要說出「這個案例的最反直覺之處是什麼」。每案的說明文應有不同的切入角度：
- Marriott：強調「為什麼分階段是幻覺」
- KKday：強調「先解決別人的問題才能解決自己的問題」
- Hyatt：強調「資料越好，反而讓組織更封閉」
- Disney：強調「採集的主動性不應在用戶端」
- Club Med：強調「服務不被感知等於服務不存在」

---

### Content-A｜Statement + Evidence（主從佈局）

- 資訊形狀：1 個主張區（55%寬）+ 最多 2 個佐證卡（45%寬）
- 適用頁面：Real problem、Tension、Case in one line
- 結構：

```
左側色帶（8mm）
┌──────────────────┬─────────────────────┐
│ 主張區（55%）     │ 佐證區（45%）        │
│                  │ ┌─────────────────┐  │
│ Quote box        │ │ 佐證卡 1        │  │
│ 識別色背景        │ └─────────────────┘  │
│ 主張句 11–13px   │ ┌─────────────────┐  │
│                  │ │ 佐證卡 2        │  │
│ 說明文 10.5px    │ └─────────────────┘  │
└──────────────────┴─────────────────────┘
```

- **Quote box 背景 = 案例識別色**（不再統一深藍）
- 左欄寬、右欄窄，視覺重心在左
- 密度上限：Quote box（1）+ 2 × Card（1+1）= 3 單位 ✓

---

### Content-B｜Metric Strip + Short Cards

- 資訊形狀：1 條 metric strip（3–4 個數字）+ 2–3 張短卡
- 適用頁面：Why it lands（數據佐證頁）
- 結構：metric strip 全寬在頂部，短卡橫排在下方
- 規則：metric 數字用識別色，短卡只放單一語意段
- 密度上限：Metric strip（1）+ 3 × Short card（1+1+1）= 4 單位 ✓

---

### Content-C｜Reframe Pair（非對稱）

- 資訊形狀：Insight 句（全寬）+ 舊框架（35%）vs. 新框架（65%）
- 適用頁面：Strategic reframe、Tension+Reframe 合頁
- 結構：

```
左側色帶（8mm）
┌──────────────────────────────────────────┐
│ Insight 句（全寬，識別色背景）            │
└──────────┬───────────────────────────────┘
           │
┌──────────┴────────────────────────────────┐
│ 舊框架（35%）  │ 新框架（65%）              │
│ 淺灰底，小字   │ 識別色深底，白字大字        │
│               │ 字重 700，行距 1.4         │
└───────────────┴───────────────────────────┘
```

- **非對稱是設計意圖**：舊框架故意做小（35%），新框架做大（65%），視覺上傳遞「舊的讓位給新的」
- Insight 句全寬佔頂部，識別色背景，字重 700，最多 44 字
- 密度上限：Quote box（1）+ Reframe pair（2）= 3 單位 ✓

---

### Content-D｜Flow（階層式，非等分）

- 資訊形狀：1 個核心動作（全寬頂部）+ 3 個結果（橫排底部）
- 適用頁面：The system
- 結構：

```
左側色帶（8mm）
┌──────────────────────────────────────────┐
│ 核心動作（全寬，識別色背景）              │
└──────────┬───────────────────────────────┘
           ↓
┌──────────┬──────────────┬───────────────┐
│ 結果 1   │ 結果 2       │ 結果 3        │
│（33%）   │（33%）       │（33%）        │
└──────────┴──────────────┴───────────────┘
```

- **一個核心驅動三個結果**，不是四件等重要的事
- 若系統有 4 個步驟，改為「核心（全寬）+ 2 個主要結果 + 1 個說明卡」
- 箭頭用靜態 `↓` 或 SVG 線條，不用 CSS `::before` 偽元素（WeasyPrint 定位不穩）
- 密度上限：Flow（2）+ Card（1）= 3 單位 ✓

---

### Content-E｜Proof Split（事實 vs. 判讀）

- 資訊形狀：左欄事實（60%）+ 右欄判讀（40%）+ 頂部 metric strip
- 適用頁面：Why it lands
- 結構：

```
左側色帶（8mm）
┌──────────────────────────────────────────┐
│ Metric strip（全寬，識別色數字）          │
└──────────────┬───────────────────────────┘
┌──────────────┴──────────┬────────────────┐
│ 已驗證事實（60%）        │ 初步判讀（40%） │
│ 條列，每條有來源標注      │ 識別色左邊框    │
│                         │ 「初步判讀」標注│
└─────────────────────────┴────────────────┘
```

- 事實欄（60%）> 判讀欄（40%），視覺傳遞「事實比判讀更有份量」
- 判讀欄必須有識別色左邊框 + 「初步判讀」標注，不允許與事實欄同視覺處理
- 密度上限：Metric strip（1）+ 左欄 Card（1）+ 右欄 Card（1）= 3 單位 ✓

---

### Content-F｜Steal（深色底，橫條三欄）

- 適用頁面：What to steal
- 背景：**識別色深色版**（見 2.3 節色彩表），不是白底
- 結構：每個 steal 是一個橫條，上下排列，每頁最多 2 個 steal

```
識別色深底（整頁）
┌──────────────────────────────────────────────────┐
│ STEAL 01 橫條                                    │
│ ┌────────┬──────────────────────┬──────────────┐ │
│ │識別號   │ 標題 + 說明文        │ 執行欄        │ │
│ │大字     │ 白色，10.5px × 4–5句 │ 具體第一步    │ │
│ │識別色   │                     │ 邊界 / 前置   │ │
│ └────────┴──────────────────────┴──────────────┘ │
│                                                  │
│ STEAL 02 橫條（同上結構）                        │
│ ┌────────┬──────────────────────┬──────────────┐ │
│ │ ...    │ ...                  │ ...          │ │
│ └────────┴──────────────────────┴──────────────┘ │
└──────────────────────────────────────────────────┘
```

**三欄比例：左（12%）中（55%）右（33%）**
- 左欄：大號序號（STEAL 01）+ 識別色背景，作視覺錨點
- 中欄：標題（font-weight 700）+ 說明文（10.5px，4–5 句，充分展開方法）
- 右欄：「具體第一步」段 + 「邊界條件」或「前置條件」段，用小標籤區分

**深色底讓頁面有重量，兩個橫條之間的間距是設計留白，不是失誤。**

**高度由內容決定，不強迫等高。** 若兩個 steal 的高度自然相差較大，允許下方 steal 較高，不需要人為填滿。

- 密度上限：2 × Steal card（2+2）= 4 單位 ✓（已滿，不得加任何元素）
- **不允許使用 `flex: 1` 強迫等高**——等高導致空白問題

---

### DarkFull-Recap（跨案比較頁）

- 背景：深藍 `#0B2244`
- 用途：Cross-Case Recap，五案橫向比較表
- 特徵：頂部金色色條 `#C9A84C`；表格白色文字；重點欄位用強調色；**無左側色帶**；**無大型數字底紋**

---

### DarkFull-Insight（核心主張頁）

- 背景：深藍 `#0B2244`
- 用途：Core Insight，整份報告最後的策略判斷
- 特徵：1 個大主張句佔版面主體，font-weight 800，留白是設計語言；**無左側色帶**；**無表格、無多卡**
- 密度上限：1 個 Quote box（1 單位）

---

### Appendix-2col（來源附錄）

- 背景：淺灰 `#F4F7FB`
- 結構：雙欄文字佈局
- 用途：來源列表 + 批判執行摘要
- **通常 2 頁，不硬壓 1 頁。** Source 字體保持 8px，不縮小。
- 頂部用漸層色條（三案識別色漸層），傳遞「這是所有案例的共同附錄」

---

## 6. 色彩規則

### 6.1 色彩角色

| 角色 | 用途 | 選色原則 |
|------|------|---------|
| 主色 | Cover / Section Divider / DarkFull 背景 | 深藍 / 深灰，傳達策略嚴肅性 |
| 識別色（每案） | 色帶、Quote box 背景、Steal 頁底色、metric 數字 | 五案各一色，明顯可區分 |
| 強調色 | badge、Section Divider 點綴 | 暖色，傳達行動感 |
| 金色 | Recap 頁頂條、Steal 頁小標籤 | 選配 |

### 6.2 Quote box 識別色化（v2.0 新增）

v1.x 所有案例的 Quote box 統一深藍，造成視覺單調且無法區分案例。

v2.0 規則：**Quote box 背景 = 該案例識別色**。

```html
<!-- Case 01 (Marriott) 的 Quote box -->
<div style="background:#3D7EC7; padding:5mm 6mm; border-radius:2px;">
  <p style="font-size:11.5px; font-weight:700; color:#fff; line-height:1.55;">主張句</p>
</div>

<!-- Case 02 (KKday) 的 Quote box -->
<div style="background:#2A9D6F; padding:5mm 6mm; border-radius:2px;">
  <p style="...">主張句</p>
</div>
```

### 6.3 顏色編碼硬規則

所有顏色必須用**硬編碼 hex**，不允許 `var(--xxx)`。  
WeasyPrint 不支援 CSS variable，使用後顏色全部失效。

### 6.4 對比度規則

- 白底頁文字與背景對比 ≥ 4.5:1
- 深色背景頁主要文字用白色，輔助文字用 `rgba(255,255,255,0.55–0.70)`
- 識別色做為 Quote box 背景時，確認白色文字對比 ≥ 4.5:1

---

## 7. 寫作規則（Slide 專用）

### 7.1 標題規則

- `sh-title`（頁面標題）：導覽用，名詞句，不是主張，中文最多 16 字
- Quote box 主張句：完整策略主張，中文最多 44 字，可引發討論

```
錯誤（描述）：「Marriott 導入了 Oracle OPERA Cloud PMS。」
正確（主張）：「分階段換系統不是穩健，是把風險在三個系統之間搬移位置。」
```

### 7.2 Section Divider 說明文

說明文必須說出「這個案例的反直覺之處」，五案說法不能格式相同。

### 7.3 Steal card 標題

必須以動詞開頭：

```
錯誤：「互依性地圖」
正確：「先畫互依性地圖，再決定哪個系統可以單獨替換」
```

### 7.4 禁止的寫作模式

- 「因為 AI 的崛起」「在數位化浪潮下」——背景描述，不是結構性力量
- 把品牌自述當事實（必須標注「品牌自述，未經獨立驗證」）
- 把推論寫成已驗證結論
- Slide 上連續文字超過 3 行未換 card

---

## 8. WeasyPrint 技術規格

### 8.1 轉換代碼

```python
from weasyprint import HTML, CSS

css = CSS(string='''
@page {
  size: 338mm 190mm;
  margin: 0;
}
''')

HTML(filename='/home/claude/report_slides.html').write_pdf(
    '/mnt/user-data/outputs/report_slides.pdf',
    stylesheets=[css]
)

print('PDF generated successfully')
```

### 8.2 WeasyPrint 已知限制（v2.0 補充）

| 限制 | 症狀 | 解法 |
|------|------|------|
| 不支援 CSS variable | 顏色全部失效（變黑或透明） | 全部改硬編碼 hex |
| flex + position:absolute 不穩定 | 偽元素箭頭位置飄移 | 改用靜態字符或 inline SVG |
| 不執行 JavaScript | 動態內容不渲染 | 全部改靜態 HTML |
| 不支援 CSS animation | 動畫無效 | 移除所有 animation / transition |
| 不支援外部圖片 URL | 圖片不顯示 | 改用 base64 或 SVG inline |
| flex align-items:stretch 強制等高 | Steal card 空白問題 | 改用 align-items:flex-start |

### 8.3 轉換前檢查清單

**頁面結構：**
- [ ] 所有投影片用 `<div class="slide">` 包裹
- [ ] 每個 `.slide` 有 `overflow: hidden`
- [ ] 每個 `.slide` 有 `page-break-after: always`
- [ ] Content 頁 `.slide` 改為 `flex-direction: row`（色帶 + 內容區）
- [ ] 無 `position: fixed`，無 `min-height: 100vh`，無 `height: 297mm`

**字體與顏色：**
- [ ] 所有顏色硬編碼 hex，無 `var(--xxx)`
- [ ] 所有字體使用 system font stack
- [ ] 白底頁 body ≥ 10.5px，深色頁 body ≥ 11px
- [ ] Source ≥ 8px，無 `font-style: italic`
- [ ] 所有 body / card 字重 ≥ 500（無 400）
- [ ] 報告中同類元素字號統一，不因頁面空間調整

**版型完整性：**
- [ ] 封面（Cover）存在
- [ ] 每案有 Section Divider，說明文有個性差異
- [ ] Content 頁有左側色帶（8mm，識別色）
- [ ] Quote box 背景使用該案識別色（非統一深藍）
- [ ] Reframe 頁為非對稱（35% vs 65%）
- [ ] Flow 頁為階層式（核心全寬 + 結果橫排）
- [ ] Steal 頁為識別色深底，橫條三欄，`align-items: flex-start`
- [ ] Steal card 不使用 `flex: 1` 強迫等高
- [ ] Cross-Case Recap 使用 DarkFull-Recap
- [ ] Core Insight 使用 DarkFull-Insight
- [ ] 每頁密度 ≤ 4 單位

**版型最低內容密度（Rule 9 自查，Phase 6 批判時對照執行）：**
- [ ] 章節封面頁（Section Divider）有情境說明文，不只是標題
- [ ] 橫向比較 / Summary 頁表格或視覺結構佔頁面 70% 以上，不留大片空白區
- [ ] 結尾頁（DarkFull-Insight）有行動指向句（≤ 30 字），不在金句後直接結束
- [ ] 高密度內文頁（Steal 等）body 字體維持硬規則（白底 ≥ 10.5px，深色底 ≥ 11px），字數超限唯一解法是拆頁

**禁止項目：**
- [ ] 無 CSS animation / transition
- [ ] 無 JavaScript
- [ ] 無外部圖片 URL
- [ ] 無 `::before` / `::after` 偽元素用於定位（WeasyPrint 不穩定）

---

## 9. HTML 骨架範本

顏色值依 Phase 6.5 決定的色彩系統替換。識別色用 `[CASE_COLOR]` 標記，深色版用 `[CASE_DARK]` 標記。

### Cover 骨架

```html
<div class="slide" style="background:#0B2244; flex-direction:row; width:338mm; height:190mm; overflow:hidden; page-break-after:always;">
  <div style="width:60%; padding:14mm 12mm 10mm 14mm; display:flex; flex-direction:column; justify-content:space-between;">
    <div style="font-size:7px; letter-spacing:3px; text-transform:uppercase; color:#C9A84C; font-weight:700;">Strategy Case Report · 2026</div>
    <div>
      <div style="font-size:9px; color:#8BAFD4; margin-bottom:2.5mm;">報告副標題</div>
      <div style="width:18mm; height:3px; background:#E8522A; margin:2.5mm 0;"></div>
      <div style="font-size:28px; font-weight:800; color:#fff; line-height:1.15;">主標題<br><span style="color:#E8522A;">重點詞</span></div>
      <div style="font-size:9px; color:#8BAFD4; line-height:1.7; margin-top:4mm;">說明文，最多 3 行。</div>
    </div>
    <div style="font-size:7px; color:rgba(255,255,255,0.2); border-top:1px solid rgba(255,255,255,0.08); padding-top:2.5mm; display:flex; justify-content:space-between;">
      <span>決策情境</span><span>Month 2026</span>
    </div>
  </div>
  <div style="width:40%; background:#0D2A52; padding:10mm; border-left:1px solid rgba(255,255,255,0.08); display:flex; flex-direction:column; justify-content:center;">
    <div style="font-size:7.5px; color:#C9A84C; letter-spacing:2px; text-transform:uppercase; margin-bottom:5mm; font-weight:700;">五個案例</div>
    <!-- 案例項目 × N，每項：大號數字底紋 + 品牌名 + 視角標籤 -->
  </div>
</div>
```

### Section Divider 骨架

```html
<div class="slide" style="background:#0B2244; flex-direction:column; justify-content:center; padding:14mm 16mm; width:338mm; height:190mm; overflow:hidden; page-break-after:always;">
  <div style="font-size:64px; font-weight:800; color:rgba(255,255,255,0.05); line-height:1; margin-bottom:-3mm;">01</div>
  <div style="font-size:7px; letter-spacing:3px; color:#C9A84C; text-transform:uppercase; font-weight:700; margin-bottom:2mm;">Case 01 · 分析視角標籤</div>
  <div style="font-size:26px; font-weight:800; color:#fff; line-height:1.2; margin-bottom:3mm;">品牌名稱<br><span style="color:#E8522A;">× 反直覺的核心動作</span></div>
  <div style="font-size:9.5px; color:#8BAFD4; max-width:165mm; line-height:1.7;">這個案例的反直覺之處是什麼——不只是「做了什麼」，而是「為什麼這件事出乎意料」。</div>
  <div style="display:inline-block; background:[CASE_COLOR]; color:#fff; font-size:7.5px; font-weight:700; padding:1.5mm 4mm; border-radius:1px; letter-spacing:1px; text-transform:uppercase; margin-top:4mm;">分析視角 · 年份</div>
</div>
```

### Content 頁骨架（含左側色帶）

```html
<div class="slide" style="flex-direction:row; background:#fff; width:338mm; height:190mm; overflow:hidden; page-break-after:always;">
  <!-- 左側色帶 -->
  <div style="width:8mm; background:[CASE_COLOR]; flex-shrink:0;"></div>
  <!-- 內容區 -->
  <div style="flex:1; padding:7mm 12mm 6mm; display:flex; flex-direction:column; overflow:hidden;">
    <!-- Slide Header -->
    <div style="display:flex; align-items:flex-start; justify-content:space-between; margin-bottom:4mm; flex-shrink:0;">
      <div>
        <div style="font-size:7px; color:[CASE_COLOR]; font-weight:700; letter-spacing:2px; text-transform:uppercase;">Case 0X · 品牌名稱</div>
        <div style="font-size:17px; font-weight:800; color:#0B2244; line-height:1.2; margin-top:0.5mm;">頁面標題（最多 16 字）</div>
      </div>
      <div style="text-align:right; flex-shrink:0; margin-left:4mm;">
        <div style="font-size:13px; font-weight:800; color:#E8EEF6;">2023–25</div>
        <div style="font-size:7px; color:#94A3B8;">分析視角</div>
      </div>
    </div>
    <!-- 主體內容（依版型選擇組合元素） -->
  </div>
</div>
```

**注意：`sh-right` 需要 `flex-shrink:0; margin-left:4mm;` 防止標題文字重疊。**

### Content-A（Statement + Evidence）骨架

```html
<!-- 在 Content 頁骨架的主體內容區插入 -->
<div style="display:flex; gap:4mm; flex:1; overflow:hidden;">
  <!-- 主張區（55%） -->
  <div style="width:55%; display:flex; flex-direction:column; gap:3mm; overflow:hidden; flex-shrink:0;">
    <!-- Quote box，識別色背景 -->
    <div style="background:[CASE_COLOR]; padding:5mm 6mm; border-radius:2px;">
      <p style="font-size:11.5px; font-weight:700; color:#fff; line-height:1.55;">核心主張句，最多 44 字。</p>
      <div style="font-size:8px; font-weight:500; color:rgba(255,255,255,0.5); margin-top:2mm;">來源標注</div>
    </div>
    <!-- 說明文 -->
    <p style="font-size:10.5px; font-weight:500; color:#334155; line-height:1.6;">補充說明，2–3 句。</p>
  </div>
  <!-- 佐證區（45%） -->
  <div style="flex:1; display:flex; flex-direction:column; gap:3mm; overflow:hidden;">
    <!-- 佐證卡 1 -->
    <div style="border:1px solid #E8EEF6; border-radius:2px; padding:4mm; flex:1; overflow:hidden;">
      <div style="font-size:7px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:#94A3B8; margin-bottom:2mm;">佐證標籤</div>
      <div style="font-size:10.5px; font-weight:700; color:#0B2244; margin-bottom:2mm; line-height:1.4;">卡片標題</div>
      <p style="font-size:10.5px; font-weight:500; color:#334155; line-height:1.6;">說明文字。</p>
    </div>
    <!-- 佐證卡 2（同上） -->
  </div>
</div>
```

### Content-C（Reframe，非對稱）骨架

```html
<!-- 在 Content 頁骨架的主體內容區插入 -->
<!-- Insight 句全寬 -->
<div style="background:[CASE_COLOR]; padding:4mm 6mm; border-radius:2px; margin-bottom:3mm; flex-shrink:0;">
  <p style="font-size:11.5px; font-weight:700; color:#fff; line-height:1.55;">Insight 主張句，最多 44 字。</p>
</div>
<!-- 非對稱雙欄 -->
<div style="display:flex; gap:3mm; flex:1; overflow:hidden;">
  <!-- 舊框架（35%） -->
  <div style="width:35%; background:#F4F7FB; padding:4mm 5mm; border-radius:2px; flex-shrink:0; overflow:hidden;">
    <div style="font-size:7px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:#94A3B8; margin-bottom:2mm;">舊框架</div>
    <div style="font-size:12px; font-weight:700; color:#64748B; line-height:1.4; margin-bottom:2mm;">舊說法（1 句）</div>
    <p style="font-size:10.5px; font-weight:500; color:#64748B; line-height:1.6;">說明為何這個框架被接受了多年。</p>
  </div>
  <!-- 新框架（65%） -->
  <div style="flex:1; background:[CASE_DARK]; padding:4mm 5mm; border-radius:2px; overflow:hidden;">
    <div style="font-size:7px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:[CASE_COLOR]; margin-bottom:2mm;">新框架</div>
    <div style="font-size:13px; font-weight:800; color:#fff; line-height:1.4; margin-bottom:2mm;">新說法（1 句，字體略大於舊框架）</div>
    <p style="font-size:10.5px; font-weight:500; color:rgba(255,255,255,0.8); line-height:1.6;">說明品牌主動放棄了什麼，選擇了什麼。</p>
  </div>
</div>
```

### Content-D（Flow，階層式）骨架

```html
<!-- 在 Content 頁骨架的主體內容區插入 -->
<!-- 核心動作（全寬） -->
<div style="background:[CASE_COLOR]; padding:4mm 6mm; border-radius:2px; margin-bottom:3mm; flex-shrink:0;">
  <div style="font-size:8px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:rgba(255,255,255,0.6); margin-bottom:1mm;">核心動作</div>
  <div style="font-size:12px; font-weight:800; color:#fff;">核心動作說明</div>
</div>
<!-- 靜態箭頭 -->
<div style="text-align:center; font-size:14px; color:#C9A84C; margin-bottom:2mm; flex-shrink:0;">↓</div>
<!-- 三個結果（等寬橫排） -->
<div style="display:flex; gap:3mm; flex:1; overflow:hidden;">
  <!-- 結果 1、2、3，各 flex:1 -->
  <div style="flex:1; border:1px solid #E8EEF6; border-radius:2px; padding:4mm; overflow:hidden;">
    <div style="font-size:8px; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; color:[CASE_COLOR]; margin-bottom:2mm;">結果標籤</div>
    <div style="font-size:10.5px; font-weight:700; color:#0B2244; margin-bottom:2mm; line-height:1.4;">結果標題</div>
    <p style="font-size:10.5px; font-weight:500; color:#334155; line-height:1.6;">說明文，2 句。</p>
  </div>
  <!-- × 3 -->
</div>
```

### Content-F（Steal，深色底）骨架

```html
<div class="slide" style="background:[CASE_DARK]; flex-direction:column; width:338mm; height:190mm; overflow:hidden; page-break-after:always;">
  <div style="flex:1; padding:10mm 12mm 10mm; display:flex; flex-direction:column; justify-content:center; gap:5mm; overflow:hidden;">
    <!-- Steal 01 橫條 -->
    <div style="display:flex; align-items:flex-start; gap:0; border-radius:2px; overflow:hidden;">
      <!-- 左：識別號（12%） -->
      <div style="width:12%; background:[CASE_COLOR]; padding:5mm 4mm; display:flex; flex-direction:column; align-items:center; justify-content:flex-start; flex-shrink:0;">
        <div style="font-size:7px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:rgba(255,255,255,0.7);">STEAL</div>
        <div style="font-size:28px; font-weight:800; color:#fff; line-height:1;">01</div>
      </div>
      <!-- 中：說明（55%） -->
      <div style="width:55%; background:rgba(255,255,255,0.05); padding:5mm; flex-shrink:0;">
        <div style="font-size:11px; font-weight:700; color:#fff; line-height:1.4; margin-bottom:3mm;">動詞開頭的方法標題</div>
        <p style="font-size:11px; font-weight:500; color:rgba(255,255,255,0.85); line-height:1.6;">說明文，4–5 句，充分展開這個方法的邏輯和適用場景。比 v1.x 更詳細，因為深色底給了更多視覺份量。</p>
      </div>
      <!-- 右：執行（33%） -->
      <div style="flex:1; background:rgba(0,0,0,0.15); padding:5mm;">
        <div style="font-size:7px; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; color:[CASE_COLOR]; margin-bottom:1.5mm;">具體第一步</div>
        <p style="font-size:11px; font-weight:500; color:rgba(255,255,255,0.85); line-height:1.6; margin-bottom:3mm;">召集哪些人、做什麼決定、產出什麼。</p>
        <div style="font-size:7px; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; color:[CASE_COLOR]; margin-bottom:1.5mm;">邊界條件</div>
        <p style="font-size:11px; font-weight:500; color:rgba(255,255,255,0.7); line-height:1.6;">此方法在什麼情境下不成立。</p>
      </div>
    </div>
    <!-- Steal 02（同上結構） -->
  </div>
</div>
```

---

## 10. 常見錯誤與修正

**錯誤 1｜識別色帶缺失**  
Content 頁沒有左側 8mm 色帶，讀者無法快速定位案例。  
→ 修正：`.slide` 改為 `flex-direction: row`，第一子元素固定 `width:8mm; background:[CASE_COLOR]; flex-shrink:0`。

**錯誤 2｜Quote box 統一深藍，五案無差異**  
所有案例的 Quote box 顏色相同，失去案例個性。  
→ 修正：Quote box 背景改為該案識別色。確認白色文字對比 ≥ 4.5:1。

**錯誤 3｜Reframe 頁左右等寬**  
舊框架和新框架等寬，視覺上傳遞「兩者同等重要」，失去「舊讓位給新」的意圖。  
→ 修正：舊框架 35%、新框架 65%，新框架字體略大（13px vs 12px）。

**錯誤 4｜Flow 頁四步驟等分橫排**  
四欄等分抹平了「核心動作驅動結果」的層次關係。  
→ 修正：改為階層式——核心動作全寬頂部，結果三欄底部。箭頭用靜態字符 `↓`。

**錯誤 5｜Steal 頁白底 + 深色卡片，空白問題嚴重**  
深色卡片在白色背景上視覺重心失衡，卡片下方大量空白。  
→ 修正：Steal 頁整頁改為識別色深底，卡片改為橫條三欄，`align-items: flex-start`。

**錯誤 6｜Steal card 使用 flex:1 強迫等高**  
兩個 steal card 等高撐滿頁面，內容少的那個下方出現大量空白。  
→ 修正：移除 `flex: 1`，改用 `align-items: flex-start` 讓高度由內容決定。兩條橫條之間的間距是設計留白。

**錯誤 7｜sh-right 年份字與 sh-title 標題重疊**  
`sh-title` 長標題與右側 `sh-right` 年份字在 WeasyPrint 渲染時 overlap。  
→ 修正：`sh-title` 加 `max-width`（約 55–60% 可用寬度）；`sh-right` 加 `flex-shrink:0; margin-left:4mm`。

**錯誤 8｜Flow 箭頭用 CSS ::before 偽元素**  
WeasyPrint 對 flex + `::before` 偽元素的定位支援不穩定，箭頭飄移。  
→ 修正：改用靜態字符 `↓` 或 inline SVG 直線，不使用 `::before / ::after`。

**錯誤 9｜字體在不同頁面大小不一致**  
body 字體在某些頁面 10.5px、某些頁面 9.5px，整體視覺層級混亂。  
→ 修正：全報告同類元素字號統一。body 固定 10.5px（白底）/ 11px（深色底），不因頁面空間調整。拆頁是唯一解法。

**錯誤 10｜使用 CSS variable**  
`var(--primary)` 在 WeasyPrint 中顏色全部失效。  
→ 修正：全部改硬編碼 hex。

**錯誤 11｜Section Divider 五案說明文格式相同**  
五案 Section Divider 的說明文都是「面對 X，選擇 Y」的格式，視覺節奏單調。  
→ 修正：每案說明文角度不同，強調該案最反直覺的那個面向。

**錯誤 12｜整份報告沒有深色內容頁**  
深色頁只出現在過場（Section Divider），內容全白。視覺節奏單調，steal 頁也是白底。  
→ 修正：Steal 頁強制深色底，確保每案結尾有一頁深色視覺句號。

**錯誤 13｜`overflow: hidden` 被移除**  
當內容超出時移除 `overflow: hidden` 讓內容溢出，破壞投影片邊界。  
→ 修正：`overflow: hidden` 不可移除，拆頁是唯一解法。
