# SEO Manager — Flow

SEO Manager 的 handoff 細節邏輯。workflow.yaml 的步驟以 `#anchor` 引用本檔對應段落。

## classify

解析 user 訊息，萃取結構化參數：

| 參數 | 說明 | 預設 |
|------|------|------|
| `target_site` | 要做 SEO 的網站 / 網域 | 必填，缺則向 user 確認 |
| `target_market` | 目標市場 / 語系（如 zh-TW、en-US） | zh-TW |
| `seed_topics` | 種子主題 / 業務領域 | 從需求推斷 |
| `output_format` | 交付格式（report / xlsx / docx） | report（Markdown） |
| `language` | 回應語言 | 跟隨 user 語言 |

收尾動作：
1. **回饋偵測** — 依 `agents/protocols/rules/feedback-memory.md`，偵測 user 訊息是否含正/負面回饋；有則先保存 memory 再繼續。
2. **Dispatch Granularity** — 多目標語意（「以及 / 和 / + / 並 / 另外」或分點列多項）強制拆成多次獨立 dispatch，每次一個明確產出。純同質性批量（例：N 個頁面跑同種稽核）視為單一產出。
3. **KPI Alignment Check** — 為每個子任務寫下對應的 KPI 面向（自然流量 / 排名 / 索引健康 / 轉換）。
4. 產出 dispatch_plan（worker 清單 + 順序 + 平行關係 + 每棒 brief 要點）。

## execute

依 dispatch_plan 派遣 worker。派遣 brief 一律遵循 `workflow/dispatch-protocol.md`（含 4 個 bootstrap 路徑、Worklog Block、Memory Block、Task Block、KPI 對齊、Language）。

**派遣順序與平行關係：**

| 棒次 | Worker | 平行? | 依賴 | 產出 |
|------|--------|-------|------|------|
| 1 | keyword-researcher (+ shared/researcher) | 平行 | parsed_request | keyword_map（意圖分群、量/競爭、長尾機會） |
| 1 | technical-auditor | 平行（與 keyword 同時） | parsed_request | audit_report（索引、CWV、結構化資料；僅診斷與建議） |
| 1 | analytics-reporter | 可獨立先行 | 既有數據源 / 基線 | baseline_report（自然流量/排名/轉換基線） |
| 2 | content-strategist | 否 | keyword_map | content_plan（topic cluster、大綱、內部連結） |

每個 worker 返回後：
- 依 `agents/protocols/workflows/inline-verify-flow.md` 逐項驗證聲稱與實際產出一致。
- 需要精確數值（成長率、CTR、轉換率、預估流量價值）時派 `shared/calculator`，不自行心算。
- 失敗則 retry_once（細化 prompt）；仍失敗則記錄 gap，帶部分結果進 synthesize。

## deliver

回報給 user，依 `workflow/synthesis-rules.md` 組裝，必含：
- 各產出檔案 / 結果路徑（每項一行）
- **KPI 對齊說明** — 每項產出對應「半年提升自然搜尋成效」的哪個面向與預期影響
- 行動建議優先序（高/中/低，附理由）
- 工時打卡明細表（取自 worklog JSON，不得粗估）
- 以 user 的語言輸出
