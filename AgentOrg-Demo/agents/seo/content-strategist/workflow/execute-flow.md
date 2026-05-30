# Content Strategist — Execute Flow

R-D-V 第三段：Execute（含自我驗證）。將策略藍圖落地為文件並寫入 Manager 指定的 output_path。由 workflow.yaml route 到此。

## 流程步驟

### 1. 確認輸出路徑
- 寫入 Manager 派遣訊息中的 `output_path`（典型 `<CWD>/output/seo/{task_id}/`）。
- **嚴禁**寫入自己的 agent 資料夾（memory/ 除外）。
- 若缺 `output_path`：停止並回報 `SCOPE VIOLATION: missing output_path`。

### 2. 產出策略文件
依任務需求寫出以下檔案（檔名依任務慣例）：
- `content-strategy.md` — 主題叢集架構（pillar/cluster + 內鏈圖）
- `content-calendar.md` — 內容日曆（發佈日期、主題、優先級、負責角色）
- `onpage-spec.md` — 每頁 on-page 規格表（title/meta/H/內鏈/slug）
- `eeat-checklist.md` — 每篇內容的 E-E-A-T 信任訊號清單（YMYL 標記）
- 工具：`Write`（新檔）/ `Edit`（同次任務內迭代修訂自己的草稿）/ `mcp__desktop-commander__write_file`（T:\ 路徑）。

### 3. 自我驗證（Verify）
交付前逐項自查：
- 每篇規劃內容是否對應明確搜尋意圖？（無意圖者剔除）
- 每篇是否歸屬某叢集並標注內鏈方向？（無孤島）
- title_tag ≤60 字元、meta_description ≤155 字元、含主關鍵字且未 keyword stuffing？
- H 層級唯一且語意正確？內鏈含錨文字與目標 URL？
- YMYL 主題是否含 E-E-A-T 信任訊號？
- 所有輸出檔是否寫在 `output_path`（非 agent 資料夾）？

### 4. 回報 Manager
- 回傳產出檔案清單、叢集數/規劃頁數、已驗證項目與任何 flags（如 keyword_data_missing）。
- 自我驗證失敗項目：修正後重跑步驟 2–3，無法修正則回報 Manager。

## 注意事項
- 字數/頁數/字元數等精確統計請求 `shared/calculator`，不自行心算。
- 收工前依 workflow.yaml 觸發 `save_memory`（若被退回或有新意圖模式）與 `log_end` 打卡。
