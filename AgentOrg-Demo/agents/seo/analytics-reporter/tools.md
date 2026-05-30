# Analytics Reporter — Tools

## Primary Tools

| Tool | Purpose |
|------|---------|
| `Read` | 讀取排名/流量原始資料匯出（GSC/GA4 CSV、JSON）、KPI 目標設定、關鍵字清單、內容上線時間線、既有分析記憶 |
| `Write` | 將成效報告寫入 Manager 派遣訊息中的 `output_path` 指定路徑（典型為 `<CWD>/output/seo/{task_id}/`，詳見 `agents/protocols/rules/output-placement.md`）；memory 更新寫入自身 `agents/seo/analytics-reporter/memory/`；**不得**把報告寫入自己的 agent 資料夾 |

## MCP Tools (Authorized)

| MCP Tool | 授權操作 | 用途 |
|----------|---------|------|
| `mcp__desktop-commander__read_multiple_files` | read_multiple_files | 讀取 T:\ 路徑的 GSC/GA4 匯出資料、KPI 設定、時間線檔（Google Drive 限制須用此工具，禁用 read_file）|
| `mcp__desktop-commander__list_directory` | list_directory | 掃描 T:\ 目錄結構，定位原始資料與輸出路徑 |
| `mcp__workspace__bash` | worklog | 呼叫 `scripts/worklog.sh` 打卡記錄工時 |

> 注意：T:\ 路徑的檔案**必須**用 `read_multiple_files`，禁止使用 `read_file`（Google Drive 限制，見 `agents/protocols/rules/google-drive-read.md`）。

## Do NOT Use

- `Agent` — 不直接呼叫其他 agent（含 shared/calculator 的派遣由 Manager 協調）
- `Edit` — 不修改既有內容、不改原始資料；只新增報告與 memory
- `Bash` — 不執行任意 shell 指令（打卡用 `mcp__workspace__bash` 呼叫 worklog.sh，不在此執行其他腳本）
- `WebSearch` / `WebFetch` — 外部搜尋/抓取不在成效分析職責內；資料來自 GSC/GA4 既有匯出
- 自行心算精確數值 — 所有成長率、百分比、KPI 達成率必須委派 `shared/calculator`（由 Manager 派遣）
- 寫入自己的 agent 資料夾作為任務產物存放處（違反 `agents/protocols/rules/output-placement.md`）— 報告一律寫 Manager 傳來的 `output_path`（memory/ 除外，那是 agent 內部快取）

## Tool Usage Guidelines

- **Read 來源順序**：先讀 KPI 目標設定與報告期間 → 再讀原始資料匯出（GSC/GA4/排名）→ 再讀內容上線時間線（供歸因）→ 才開始分析
- **計算委派流程**：彙整待算清單（指標 + 原始數值 + 公式）→ 回報 Manager 請派 `shared/calculator` → 收到結果後填入報告，未驗證者標「待驗證」
- **Write 目標路徑**：
  - 成效報告一律寫入 Manager 派遣訊息中的 `output_path`（典型為 `<CWD>/output/seo/{task_id}/`）
  - **若 Manager 未傳 `output_path`，停止執行並回報 `SCOPE VIOLATION: missing output_path`**
  - Memory 寫入 `agents/seo/analytics-reporter/memory/`（agent 內部快取，不是任務產物）
  - 詳見 `agents/protocols/rules/output-placement.md`
- **報告格式**：每個數字標來源 + 期間 + 比較基準；KPI 達成度回扣半年目標；歸因區分「已證實 / 相關但未證實」；結尾附可行動訊號
