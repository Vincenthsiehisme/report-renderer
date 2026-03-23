# Report Data Schema

> **讀取時機**：Phase 7 路徑 B（HTML artifact）執行前，由 `references/phases-4-7.md` 引用。  
> **用途**：定義截點 JSON（`report_data.json`）的完整資料結構。第一段分析 context 結束後填入，第二段輸出 context 讀入並依此製作報告。  
> **字體說明**：`visual.fonts` 在 PDF 路徑使用 system font（Georgia、Arial、Palatino 等），HTML 路徑可使用 Google Fonts。  
> **核心變更（v2.6）**：每個分析單元從「結論填空」改為三欄結構（confirmed / estimated / unknown）；新增 `root_evidence` 頂層區塊承載扎根搜尋結果。

---

## 頂層結構

```
report_data.json
├── meta              報告基本資訊
├── visual            視覺系統（色彩 + 字體）
├── cases[]           案例陣列（1–5 案）
│   ├── root_evidence     扎根搜尋結果（三層）
│   └── pages{}           各分析頁內容（三欄結構）
└── conclusion        跨案結論（選填）
```

---

## `meta`

| 欄位 | 型別 | 必填 | 說明 |
|------|------|------|------|
| `title` | string | ✅ | 報告標題 |
| `subtitle` | string | — | 一句話說明本期聚焦的策略問題 |
| `date` | string | ✅ | 格式 `YYYY-MM-DD` |
| `mode` | string | ✅ | `"full"` 或 `"sprint"` |

---

## `visual`

### `visual.colors`

| 欄位 | 說明 |
|------|------|
| `bg_primary` | 主背景色 |
| `bg_secondary` | 次要背景，用於區塊分隔或卡片底色 |
| `text_primary` | 主文字色 |
| `text_secondary` | 次要文字色 |
| `accent` | 強調色 |

所有顏色填硬編碼 hex，不使用 CSS variable。

### `visual.fonts`

| 欄位 | 說明 |
|------|------|
| `display` | Display 層級字體 |
| `body` | Body 層級字體 |

### `visual.mood`

從以下四選一：`銳利張力` / `沉穩優雅` / `專業精準` / `高冷奢華`

---

## `cases[]`

每個 case 物件結構：

| 欄位 | 型別 | 必填 | 說明 |
|------|------|------|------|
| `id` | number | ✅ | 案例序號，從 1 起算 |
| `brand` | string | ✅ | 品牌名稱 |
| `industry` | string | ✅ | 產業類別 |
| `year` | string | ✅ | 執行年份 |
| `case_in_one_line` | string | ✅ | 格式：「[品牌] 面對 [真實問題]，選擇 [策略動作]，而不是 [原本的做法]。」 |
| `root_evidence` | object | ✅ | 扎根搜尋結果，見下方 |
| `pages` | object | ✅ | 各分析頁內容，見下方 |
| `model_configs` | array | — | 使用的分析模型配置 |
| `sources` | array | ✅ | 來源列表 |

---

## `cases[].root_evidence`

扎根搜尋（Phase 3.5）的結果。三層各自有 `confirmed`（直接資料）和 `estimated`（代理變數）兩個欄位。

```json
"root_evidence": {
  "pressure": {
    "status": "rooted | estimated | unknown",
    "confirmed": [
      {
        "fact": "具體事實陳述（資料裡說的，不是推論）",
        "source_ref": "對應的來源 label 或 URL"
      }
    ],
    "estimated": [
      {
        "claim": "推論內容",
        "proxy": "使用的代理變數",
        "assumption": "這個推論成立的假設條件",
        "source_ref": "來源"
      }
    ]
  },
  "tension": {
    "status": "rooted | estimated | unknown",
    "confirmed": [],
    "estimated": []
  },
  "boundary": {
    "status": "rooted | estimated | unknown",
    "confirmed": [],
    "estimated": []
  }
}
```

**status 規則**：
- `rooted`：confirmed 至少有 1 條
- `estimated`：confirmed 為空，estimated 至少有 1 條
- `unknown`：兩者都為空

**第二段輸出映射**：
- `rooted` → Why it lands 的「已驗證事實」欄，正文引用
- `estimated` → Why it lands 的「初步判讀」欄，附代理變數說明
- `unknown` → 來源附錄的「待補資料」區塊，標注「此層缺乏佐證」

---

## `cases[].pages` 三欄結構

**v2.6 核心變更**：每個分析單元從「結論性填空」改為三欄結構。

三欄定義：

| 欄位 | 說明 | 輸出映射 |
|------|------|---------|
| `confirmed` | 直接資料支撐的判斷，每條附 source_ref | 正文，無標注 |
| `estimated` | 代理變數推論，每條附 proxy + assumption | 正文 + 「初步判讀」標注 |
| `unknown` | 目前無法判斷，每條附 needed + impact | 來源附錄「待補資料」區塊 |

**Full Mode 必填頁面**：`real_problem` / `tension` / `strategic_reframe` / `system` / `why_it_lands` / `what_to_steal`
**Sprint Mode 必填頁面**：`real_problem` / `tension` / `strategic_reframe` / `what_to_steal`

---

### `real_problem`

```json
"real_problem": {
  "headline": "頁面標題，10–20 字",
  "footer": "頁尾收束句",
  "confirmed": [
    {
      "claim": "直接資料支撐的問題定義句",
      "source_ref": "來源 label"
    }
  ],
  "estimated": [
    {
      "claim": "推論的問題描述",
      "proxy": "使用的代理變數",
      "assumption": "成立假設"
    }
  ],
  "unknown": [
    {
      "question": "目前無法判斷的問題面向",
      "needed": "需要什麼資料才能判斷",
      "impact": "補齊後結論會怎麼改變"
    }
  ]
}
```

---

### `tension`

```json
"tension": {
  "headline": "頁面標題",
  "insight": "Insight 句（從 confirmed 或 estimated 提煉，需標注來源層級）",
  "insight_confidence": "rooted | estimated",
  "footer": "頁尾收束句",
  "confirmed": [],
  "estimated": [],
  "unknown": []
}
```

`insight_confidence` 標注這個 insight 句是基於直接資料還是代理推論——第二段輸出時決定是否加「初步判讀」標注。

---

### `strategic_reframe`

```json
"strategic_reframe": {
  "headline": "頁面標題",
  "old_frame": "品牌原本的說法",
  "new_frame": "品牌選擇的新說法",
  "rationale": "換框依據，一句話",
  "footer": "頁尾收束句",
  "confirmed": [],
  "estimated": [],
  "unknown": []
}
```

---

### `system`

```json
"system": {
  "headline": "頁面標題",
  "touchpoints": [
    {
      "name": "接觸點名稱",
      "role": "這個點在消費者路徑中的角色",
      "image_status": "found | needed"
    }
  ],
  "image_brief": null,
  "footer": "頁尾收束句",
  "confirmed": [],
  "estimated": [],
  "unknown": []
}
```

---

### `why_it_lands`

```json
"why_it_lands": {
  "headline": "頁面標題",
  "footer": "頁尾收束句",
  "confirmed": [
    {
      "claim": "已驗證事實",
      "source_ref": "來源 label"
    }
  ],
  "estimated": [
    {
      "claim": "初步判讀",
      "proxy": "代理變數",
      "assumption": "成立假設"
    }
  ],
  "unknown": [
    {
      "question": "待補問題",
      "needed": "需要什麼資料",
      "impact": "補齊後的影響"
    }
  ]
}
```

`why_it_lands` 的 `confirmed` 直接從 `root_evidence` 對應層的 confirmed 資料填入，不重新生成。

---

### `what_to_steal`

```json
"what_to_steal": {
  "headline": "頁面標題",
  "footer": "頁尾收束句",
  "methods": [
    {
      "title": "動詞開頭的方法標題",
      "scenarios": [
        {
          "condition": "在 A 條件下（品牌有直客基礎）",
          "first_step": "第一步是 X",
          "confidence": "rooted | estimated"
        },
        {
          "condition": "在 B 條件下（純 OTA 依賴）",
          "first_step": "需要先做 Y",
          "confidence": "estimated"
        },
        {
          "condition": "在 C 條件下（組織沒有數據能力）",
          "outcome": "此方法不成立",
          "confidence": "rooted | estimated"
        }
      ],
      "boundary_status": "rooted | estimated | unknown"
    }
  ],
  "confirmed": [],
  "estimated": [],
  "unknown": []
}
```

`scenarios` 取代原本的單一 `condition` 字串，改為「條件 → 結果」的區間結構。`boundary_status` 來自 `root_evidence.boundary.status`。

---

## `cases[].model_configs`

選填。每項：

| 欄位 | 說明 |
|------|------|
| `model` | 模型名稱，參照 `references/model-library.md` |
| `page` | 放在哪一頁 |
| `purpose` | 本案應用結論，非模型名稱 |

---

## `cases[].sources`

每項：

| 欄位 | 說明 |
|------|------|
| `label` | 資訊標注 |
| `source` | 來源名稱或完整 URL |
| `type` | `"official"` / `"major_media"` / `"secondary_media"` / `"proxy"` |

`type: "proxy"` 為 v2.6 新增，標注代理變數來源，在來源附錄中與直接引用分開列示。

---

## `conclusion`

選填。Sprint Mode 可設為 `null`。

| 欄位 | 說明 |
|------|------|
| `cross_case_insight` | 跨案洞察，不是摘要，是更高一層的判斷 |
| `action_prompt` | 對讀者的行動建議或思考提問 |
| `key_unknowns` | 選填，string[]，三案中最關鍵的 1–3 個待補資料 |

`key_unknowns` 是整份報告的知識邊界聲明，寫在結尾頁，讓讀者知道「目前最該補哪個資訊，補完後會最影響判斷」。

---

## 截點使用說明

**第一段結束時**，AI 輸出填完的 JSON 並說明：

> 「第一段分析完成。請確認 JSON 內容後，開啟新對話，貼入以下內容啟動輸出：
>
> 系統提示：你是 Strategy Case Report Skill 的輸出執行器。讀取以下 JSON 和 references/output-slide.md（或 output-a4.md），執行 Phase 6.5 和 Phase 7。
>
> [JSON 內容]」

**第二段讀入內容**：
- `report_data.json`
- `references/output-slide.md` 或 `references/output-a4.md`
- 不讀入任何分析 references

**輸出映射規則**：

| JSON 欄位狀態 | 輸出處理 |
|-------------|---------|
| `confirmed` | 正文直接呈現，無額外標注 |
| `estimated` | 正文呈現 + 「初步判讀」標注 + 代理變數說明 |
| `unknown` | 不進正文，寫入來源附錄「待補資料」區塊 |
| `root_evidence.*.status = unknown` | 對應分析單元加「此層缺乏直接佐證」警示 |
