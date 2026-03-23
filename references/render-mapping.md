# Render Mapping｜JSON → 頁面元素映射規則

> **讀取時機**：Phase 3 渲染執行前，與對應版型規格文件（output-slide.md 或 output-a4.md）一起讀取。  
> **用途**：定義 `report_data.json` 每個欄位如何對應到頁面的視覺元素，以及三欄結構（confirmed / estimated / unknown）的具體渲染規則。  
> **原則**：渲染器只負責映射，不補分析、不推論、不修改 JSON 內容。

---

## 核心映射原則

### 三欄狀態的渲染規則

這是整份報告的信心層級系統，每種狀態有對應的視覺處理：

| JSON 狀態 | 渲染處理 | 視覺標注 |
|----------|---------|---------|
| `confirmed` | 正文直接呈現 | 無標注 |
| `estimated` | 正文呈現 + 標注 | 「初步判讀」badge（識別色底色，白字）|
| `unknown` | **不進正文** | 移至來源附錄「待補資料」區塊 |

**`estimated` 的完整渲染格式**：
```
[判斷內容]
┌─────────────────────────────┐
│ 初步判讀                     │
│ 代理變數：[proxy]            │
│ 成立假設：[assumption]       │
└─────────────────────────────┘
```

在 Slide 版中，代理變數說明縮至 Source 層級字體（8px），不佔主要版面。
在 A4 版中，以腳注形式呈現，不中斷主文閱讀流。

**`unknown` 的附錄格式**：
```
【待補資料｜[案例名] - [頁面名稱]】
問題：[unknown.question]
需要：[unknown.needed]
影響：[unknown.impact]
```

---

## 頁面級別映射

### Cover（封面）

| 視覺元素 | JSON 來源 |
|---------|---------|
| 報告主標題 | `meta.title` |
| 副標題 | `meta.subtitle` |
| 日期 | `meta.date` |
| 案例列表（品牌名 + 視角） | `cases[].brand` + `cases[].case_in_one_line`（截取前半句）|
| 決策情境（meta 欄） | `meta.subtitle` |

---

### Section Divider（案例分隔頁）

| 視覺元素 | JSON 來源 |
|---------|---------|
| 案例序號 | `cases[].id`（補零格式：01 / 02 / 03）|
| 品牌名稱 | `cases[].brand` |
| 產業 + 年份 badge | `cases[].industry` + `cases[].year` |
| 說明文（反直覺之處） | `cases[].case_in_one_line`（完整句）|

說明文使用 `case_in_one_line` 的完整句，不額外推論「反直覺之處」。

---

### Case in one line

| 視覺元素 | JSON 來源 |
|---------|---------|
| Quote box 主張句 | `cases[].case_in_one_line` |
| 產業 / 年份 eyebrow | `cases[].industry` + `cases[].year` |

---

### The real problem

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.real_problem.headline` | — |
| 主張區內容 | `pages.real_problem.confirmed[]` | 正文，無標注 |
| 佐證區內容 | `pages.real_problem.estimated[]` | 正文 + 初步判讀標注 |
| unknown | `pages.real_problem.unknown[]` | 移附錄 |
| 頁尾收束句 | `pages.real_problem.footer` | — |
| 壓力層扎根狀態警示 | `root_evidence.pressure.status` | 若為 `unknown`，在頁面底部加警示條：「壓力層缺乏直接佐證，以下分析基於代理推論」|

---

### The tension

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.tension.headline` | — |
| Quote box insight 句 | `pages.tension.insight` | 依 `insight_confidence` 決定是否加標注 |
| insight 信心標注 | `pages.tension.insight_confidence` | `estimated` → Quote box 右下角加「初步判讀」小 badge |
| 矛盾來源說明 | `pages.tension.confirmed[]` + `pages.tension.estimated[]` | 各自對應渲染規則 |
| unknown | `pages.tension.unknown[]` | 移附錄 |
| 頁尾收束句 | `pages.tension.footer` | — |

---

### The strategic reframe

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.strategic_reframe.headline` | — |
| 舊框架（35% 欄） | `pages.strategic_reframe.old_frame` | 直接呈現 |
| 新框架（65% 欄） | `pages.strategic_reframe.new_frame` | 直接呈現 |
| 換框依據 | `pages.strategic_reframe.rationale` | 新框架欄下方，Source 層級 |
| confirmed / estimated / unknown | `pages.strategic_reframe.*` | 標準三欄規則 |
| 頁尾收束句 | `pages.strategic_reframe.footer` | — |

---

### The system

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.system.headline` | — |
| 接觸點節點 | `pages.system.touchpoints[].name` | — |
| 各節點角色說明 | `pages.system.touchpoints[].role` | — |
| 待補圖標注 | `pages.system.touchpoints[].image_status = "needed"` | 節點下方加「📋 待補圖」標注 |
| 配圖需求說明 | `pages.system.image_brief` | 若非 null，在頁面底部呈現需求描述 |
| confirmed / estimated / unknown | `pages.system.*` | 標準三欄規則 |
| 頁尾收束句 | `pages.system.footer` | — |

---

### Why it lands

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.why_it_lands.headline` | — |
| 已驗證事實欄（60%） | `pages.why_it_lands.confirmed[]` | 每條附 source_ref，正文無標注 |
| 初步判讀欄（40%） | `pages.why_it_lands.estimated[]` | 識別色左邊框 + 「初步判讀」標注 + proxy 說明 |
| unknown | `pages.why_it_lands.unknown[]` | 移附錄 |
| root_evidence 扎根狀態 | `root_evidence.*.status` | 各層 unknown → 對應段落加「此層缺乏直接佐證」警示 |
| 頁尾收束句 | `pages.why_it_lands.footer` | — |

`why_it_lands.confirmed` 的內容應直接來自 `root_evidence` 對應層的 confirmed 資料。若兩者不一致，以 `pages.why_it_lands.confirmed` 為準（分析者已做過整合）。

---

### What to steal

| 視覺元素 | JSON 來源 | 三欄處理 |
|---------|---------|---------|
| 頁面標題 | `pages.what_to_steal.headline` | — |
| Steal card 標題（左欄） | `methods[].title` | — |
| Scenario 條件列表（中欄） | `methods[].scenarios[].condition` | — |
| Scenario 執行說明（中欄） | `methods[].scenarios[].first_step` 或 `outcome` | 依 `confidence` 決定是否加初步判讀標注 |
| 邊界狀態警示 | `methods[].boundary_status = "unknown"` | 右欄加「邊界條件待驗證」標注 |
| confirmed / estimated / unknown | `pages.what_to_steal.*` | 標準三欄規則 |
| 頁尾收束句 | `pages.what_to_steal.footer` | — |

**Scenario 渲染格式**（Slide 版 Steal card 中欄）：
```
在 [condition] 的條件下
→ [first_step]           ← confidence: rooted，無標注
→ [first_step] ⚠️        ← confidence: estimated，加初步判讀標注
→ 此方法不成立           ← outcome，用不同視覺處理（灰色底或刪除線）
```

---

### Cross-Case Recap（對照頁）

從三案 JSON 提取以下欄位組成比較表：

| 比較維度 | JSON 來源 |
|---------|---------|
| 品牌名 | `cases[].brand` |
| 真實問題 | `cases[].pages.real_problem.headline` |
| Tension 核心 | `cases[].pages.tension.insight` |
| 策略重框方向 | `cases[].pages.strategic_reframe.new_frame`（截取前 20 字）|
| 最快第一步 | `cases[].pages.what_to_steal.methods[0].scenarios[0].first_step` |
| 扎根強度 | `cases[].root_evidence.pressure.status` + `tension.status`（用 icon 表示）|

**扎根強度 icon 規則**：
- `rooted` → 🟢
- `estimated` → 🟡
- `unknown` → 🔴

讓讀者在 Recap 頁一眼看出哪個案例的分析基礎最紮實。

---

### Core Insight（結尾頁）

| 視覺元素 | JSON 來源 |
|---------|---------|
| 核心主張大字 | `conclusion.cross_case_insight` |
| 行動指向句 | `conclusion.action_prompt` |
| 知識邊界聲明 | `conclusion.key_unknowns[]`（若存在）|

**知識邊界聲明渲染格式**（結尾頁底部，Source 層級字體）：
```
目前最該補的資訊：
• [key_unknowns[0]]
• [key_unknowns[1]]
```

這讓讀者在報告結束時知道「接下來應該驗證什麼」，不是空白的感動結尾。

---

### 來源附錄

來源附錄由三個部分組成，依序排列：

**Part 1｜引用來源**
從所有 `cases[].sources[]` 彙整，依 `type` 分組：
- `official`：官方來源
- `major_media`：主流媒體
- `secondary_media`：次級媒體
- `proxy`：代理變數來源（獨立分組，標注「以下為代理變數，非直接引用」）

**Part 2｜待補資料**
從所有 `pages.*.unknown[]` 彙整：
```
【待補資料｜[案例名] - [頁面名稱]】
問題：[question]
需要：[needed]
影響：[impact]
```

**Part 3｜批判執行摘要**
若 JSON 中有 `meta.critique_summary`（選填），在此呈現。
若無，附錄只有 Part 1 和 Part 2。

---

## 驗證規則（Phase 1 執行）

渲染前 AI 對 JSON 執行以下檢查，有問題直接說明，不猜測補填：

**必填欄位檢查**：

| 欄位 | Full Mode | Sprint Mode |
|------|-----------|------------|
| `meta.title` / `date` / `mode` | ✅ | ✅ |
| `visual.colors.*`（5 個色彩） | ✅ | ✅ |
| `visual.fonts.display` / `body` | ✅ | ✅ |
| `cases[].brand` / `industry` / `year` | ✅ | ✅ |
| `cases[].case_in_one_line` | ✅ | ✅ |
| `cases[].root_evidence.*` | ✅ | ✅ |
| `cases[].pages.real_problem` / `tension` / `strategic_reframe` | ✅ | ✅ |
| `cases[].pages.system` / `why_it_lands` | ✅ | ❌（選填）|
| `cases[].pages.what_to_steal` | ✅ | ✅ |
| `conclusion` | ❌（選填）| ❌（選填）|

**三欄完整性檢查**：
- 每個 `estimated` 項目必須有 `proxy` 和 `assumption`——缺少則標記為驗證錯誤
- 每個 `unknown` 項目必須有 `needed` 和 `impact`——缺少則標記為驗證錯誤
- `insight_confidence` 必須存在（`tension` 頁面專用）

**輸出媒介確認**：
- 讀取 `meta` 或使用者指定
- 若 `meta` 中無輸出媒介欄位，且使用者未指定 → 提案：「未指定輸出媒介，預設為 16:9 Slide PDF（路徑 A2），這樣對嗎？」

---

## 錯誤處理規則

| 錯誤類型 | AI 行為 |
|---------|---------|
| 必填欄位缺失 | 列出所有缺失欄位，停止渲染，等使用者補充 |
| estimated 缺 proxy / assumption | 列出問題條目，停止渲染，等補充 |
| unknown 缺 needed / impact | 列出問題條目，停止渲染，等補充 |
| 輸出媒介未指定 | 提案預設值，等確認 |
| JSON 格式錯誤（無法解析）| 說明錯誤位置，提示使用者修正後重新貼入 |
| cases 為空陣列 | 停止，說明至少需要一個案例 |

渲染器不補填、不猜測、不降級執行。有問題就停，說清楚，等修正。
