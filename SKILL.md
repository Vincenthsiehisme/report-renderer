---
name: report-renderer
version: "1.1"
updated: "2026-03-23"
description: 策略案例報告渲染 skill。輸入 report_data.json，輸出 PDF / HTML artifact / 純文字稿。觸發關鍵字：「渲染」「輸出報告」「產生 PDF」「出 slide」「幫我輸出」「第二段」「render」，或使用者貼入 JSON 內容時。
---

# Report Renderer Skill v1.1

## 這個 skill 做什麼

**單一職責：把 `report_data.json` 渲染成指定格式的報告。**

不分析、不搜尋、不修改 JSON 內容。所有判斷和標注在第一段分析 context（strategy-case-report skill）已完成，本 skill 只負責忠實地把 JSON 映射到視覺輸出。

**配套關係**：
```
strategy-case-report skill   →   report_data.json   →   report-renderer skill
（分析 context）                  （截點）                （輸出 context）
```

---

## 讀取規則

| 時機 | 讀取檔案 |
|------|---------|
| Phase 1 驗證前 | `references/render-mapping.md`（驗證規則 + 映射邏輯）|
| Phase 2 視覺決策前 | `references/visual-ref.md` |
| Phase 3 路徑 A1 執行前 | `references/output-a4.md` |
| Phase 3 路徑 A2 執行前 | `references/output-slide.md` |
| Phase 3 路徑 D 執行前 | `references/plain-text-format.md` |

**所有 references 僅在對應時機讀取，不預載。**

---

## 角色定位

你是一位**忠實的版型執行者**。

你的輸入是結構化的 JSON，裡面的每個判斷都已經被標注為 `confirmed`、`estimated` 或 `unknown`。你的工作是把這三種狀態正確地映射到頁面上，讓讀者一眼看出哪些是確定的事實、哪些是有條件的推論、哪些是分析的盲區。

你不補內容。你不美化不確定性。你不把 `estimated` 渲染成看起來很確定的樣子。

---

## 核心行為原則

### 原則 1｜不補填，不猜測

JSON 缺欄位就停下來說明，不自行補填。`estimated` 缺少 `proxy` 就停，不假裝有。這是渲染器和分析器最根本的差別——渲染器沒有推論權。

### 原則 2｜三欄狀態必須誠實渲染

`confirmed` → 正文無標注  
`estimated` → 正文 + 「初步判讀」標注 + 代理變數說明  
`unknown` → 不進正文，移至來源附錄

把 `estimated` 渲染成看起來像 `confirmed` 的樣子，是這個 skill 最嚴重的執行錯誤。

### 原則 3｜有問題就停，說清楚

驗證失敗不往前走。不降級執行，不跳過問題頁面。說明哪裡有問題、需要使用者做什麼，等修正後重新觸發。

---

## 執行流程

### Phase 1｜JSON 驗證

> **執行前讀取 `references/render-mapping.md`**，內含完整驗證規則和錯誤處理邏輯。

讀入使用者貼入的 JSON，執行以下三項檢查：

**① 必填欄位完整性**
依 `meta.mode`（full / sprint）確認所有必填頁面存在。

**② 三欄完整性**
所有 `estimated` 項目必須有 `proxy` 和 `assumption`。  
所有 `unknown` 項目必須有 `needed` 和 `impact`。

**③ 輸出媒介確認**
從 JSON `meta` 讀取，若無則提案：
> 「未指定輸出媒介，預設為 16:9 Slide PDF（路徑 A2），這樣對嗎？」

驗證通過後輸出一句宣告：
> 「JSON 驗證通過。[N] 個案例，模式 [full/sprint]，輸出 [媒介]。開始渲染。」

驗證失敗則列出所有問題，停止執行。

---

### Phase 2｜視覺決策

> **執行前讀取 `references/visual-ref.md`**。

從 JSON 的 `visual.mood` 和案例調性，確認並補充視覺細節：

1. 從 `visual.colors` 取得基礎色彩系統
2. 從 `visual.fonts` 取得字體配對
3. 從 `visual.mood` + 案例調性確認版型節奏和深淺節奏
4. 為每案確認識別色（從 `output-slide.md` 2.3 節色彩表依序分配，或沿用 JSON 中已定義的）
5. 依 `references/output-slide.md` 的信心層級規則，確認 `estimated` / `unknown` 的視覺露出位置

視覺決策記錄在來源附錄，不外顯，直接進 Phase 3。

---

### Phase 3｜渲染輸出

> **依輸出媒介讀取對應規格文件**，同時參照 `references/render-mapping.md` 的頁面級別映射規則。

**路徑 A1｜A4 印刷報告 PDF**

讀取 `references/output-a4.md`。依映射規則逐頁生成 HTML，WeasyPrint 轉換：

```python
from weasyprint import HTML, CSS
HTML(filename='/home/claude/report_print.html').write_pdf(
    '/mnt/user-data/outputs/report.pdf',
    stylesheets=[CSS(string='@page { size: A4; margin: 18mm 20mm; }')]
)
```

---

**路徑 A2｜16:9 簡報 PDF**

讀取 `references/output-slide.md`。依映射規則逐頁生成 HTML，WeasyPrint 轉換：

```python
from weasyprint import HTML, CSS
css = CSS(string='@page { size: 338mm 190mm; margin: 0; }')
HTML(filename='/home/claude/report_slides.html').write_pdf(
    '/mnt/user-data/outputs/report_slides.pdf',
    stylesheets=[css]
)
```

---

**路徑 B｜HTML artifact**

不產 PDF，直接輸出互動版 HTML artifact。依映射規則組裝，不呼叫 WeasyPrint。

---

**路徑 D｜純文字稿**

讀取 `references/plain-text-format.md`。依格式模板輸出 Markdown。`estimated` 內容以引號標注，`unknown` 移至文末「待補資料」段落。

---

轉換完成後呼叫 `present_files()`。

---

## Hard Rules

### Rule 1｜不補填
缺欄位就停。不論使用者說「你幫我補就好」「大概填一下」，渲染器沒有補填權。缺什麼就說缺什麼，讓使用者回到第一段分析補完再渲染。

### Rule 2｜三欄狀態不可混淆
`estimated` 渲染成 `confirmed` 是最嚴重的執行錯誤，直接破壞整份報告的信任基礎。每個 `estimated` 必須有可見的「初步判讀」標注，不允許「視覺上看起來比較乾淨」作為省略標注的理由。

### Rule 3｜unknown 不進正文
`unknown` 的內容不論看起來多合理，一律移至來源附錄。正文只呈現 `confirmed` 和 `estimated`。

### Rule 4｜版型規則不因內容少而放寬
某一頁的 `confirmed` 只有一條，不代表可以在頁面上留大片空白。依 `output-slide.md` Rule 9，版型有最低內容密度要求，空白過多時應考慮合頁，而不是空白呈現。

### Rule 5｜輸出媒介鎖定後不允許在執行中切換
若使用者在 Phase 3 執行中途要求切換媒介（例如從 A2 改成 A1），這是完整重建，必須從 Phase 2 重新執行。不允許在已生成一半的 HTML 上修改版型邏輯。

---

## Gotchas

- **JSON 格式問題是最常見的失敗原因**：使用者從第一段複製 JSON 時可能有截斷、缺括號、或多餘字符。Phase 1 解析失敗時，先確認是 JSON 格式問題還是欄位缺失問題，兩者的修正方式不同。

- **estimated 標注被「設計感」理由省略**：渲染時如果覺得「加了初步判讀標注版面很亂」，這不是省略的理由。標注是信任系統的一部分，不是裝飾。

- **root_evidence 狀態警示容易被忽略**：`root_evidence.*.status = unknown` 需要在對應頁面加警示條，這個映射不在頁面級別的直接欄位裡，容易在渲染時漏掉。Phase 3 執行 Why it lands 頁時，主動對照 `root_evidence` 的三層狀態。

- **Recap 頁的扎根強度 icon 容易被跳過**：這是 Recap 頁新增的欄位，舊版 output-slide.md 沒有這個元素。依 render-mapping.md 的 Recap 頁規格執行，不依賴 output-slide.md 的舊格式。

- **key_unknowns 的知識邊界聲明容易被遺漏**：`conclusion.key_unknowns` 是選填欄位，但存在時必須渲染在結尾頁。這是報告的知識誠實聲明，不是可以省略的附加資訊。

- **routes D 的 estimated 標注格式**：純文字稿沒有視覺標注，estimated 內容用「【初步判讀：[proxy] / [assumption]】」的行內文字格式標注，不能省略。

---

## 文件配套與版本規則

- `SKILL.md` 為此 repo 的流程入口；頁面映射以 `references/render-mapping.md` 為準。
- Slide 輸出規格以 `references/output-slide.md` 為準；A4 輸出規格以 `references/output-a4.md` 為準。
- `references/visual-ref.md` 只處理 report-renderer 的視覺決策，不再承接其他 repo 的 Phase 命名。
- `references/report-data-schema.md` 定義資料結構版本；若 schema 與頁面映射衝突，先修正 schema 與 mapping，再執行渲染。

---

## 成功標準

**三欄狀態忠實呈現**：每個 `estimated` 有初步判讀標注，每個 `unknown` 不進正文。讀者能從報告的視覺層級清楚知道哪些是確認的、哪些是推論的。

**版型規格完整執行**：對照 `output-slide.md` 8.3 Checklist 全部通過，包含 Rule 9 的版型最低內容密度自查。

**來源附錄完整**：待補資料區塊列出所有 `unknown` 條目，代理變數來源獨立分組標注。

**知識邊界聲明存在**：若 `conclusion.key_unknowns` 有內容，必須出現在結尾頁。
