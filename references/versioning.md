# Versioning & Compatibility

> 用途：說明 report-renderer repo 內各文件的版本角色與優先序，避免跨 repo / 跨版本混淆。

## 版本角色

| 文件 | 角色 | 版本意義 |
|------|------|---------|
| `SKILL.md` | 主流程入口 | repo 的操作流程版本 |
| `references/report-data-schema.md` | 資料結構定義 | `report_data.json` schema 版本 |
| `references/render-mapping.md` | 欄位到頁面映射 | 頁面語義規則版本 |
| `references/output-slide.md` | Slide 版型 / 技術規格 | Slide 輸出規格版本 |
| `references/output-a4.md` | A4 版型 / 技術規格 | A4 輸出規格版本 |
| `references/visual-ref.md` | 視覺決策規則 | 視覺系統決策版本 |

## 優先序

1. **資料誠實性優先**：`report-data-schema.md` + `render-mapping.md`
2. **輸出媒介規格次之**：`output-slide.md` / `output-a4.md`
3. **流程控制最後**：`SKILL.md`

若不同文件出現衝突：
- 先保留 `confirmed / estimated / unknown` 的正確露出
- 再調整版型與視覺細節
- 不可為了版面整潔改寫資料語義

## 配套關係

- `report-renderer v1.1` 預期搭配目前 repo 內的 schema / mapping / output specs 一起使用。
- 若未來單獨升級 `report-data-schema.md` 或 `output-slide.md`，應同步更新本文件與 `README.md` 的版本記錄。
