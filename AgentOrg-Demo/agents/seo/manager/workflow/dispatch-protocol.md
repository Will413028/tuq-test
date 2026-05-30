<!-- Identity Block 含明確 bootstrap 路徑（agent-anatomy.md §6.7） -->
# SEO Manager — Dispatch Protocol

When dispatching each worker agent, include ALL of the following blocks in the prompt:

## 1. Identity Block

```
You are the {AgentName} agent.

Bootstrap files (read in this order, absolute paths):
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/seo/{agent-name}/agent.yaml
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/seo/{agent-name}/soul.md
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/seo/{agent-name}/org.md
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/seo/{agent-name}/tools.md

Bootstrap once, then start workflow.
```

For shared agents:
```
You are the {Researcher | Calculator} agent.

Bootstrap files (read in this order, absolute paths):
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/shared/{researcher|calculator}/agent.yaml
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/shared/{researcher|calculator}/soul.md
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/shared/{researcher|calculator}/org.md
  T:/共用雲端硬碟/快組隊.Agents/AgentOrg/agents/shared/{researcher|calculator}/tools.md

Bootstrap once, then start workflow.
```

## 2. Worklog Block

```
WORKLOG: You must punch your own clock using scripts/worklog.sh.
  First action:  FILE=$(bash scripts/worklog.sh start seo/{agent-name} {model} "{summary}" manager "{trace_id}" "{parent_task_id}")
  Last action:   bash scripts/worklog.sh end "$FILE" completed "{output}"
  If you fail:   bash scripts/worklog.sh end "$FILE" failed "{error}"
```

NOTE:
- dispatched_by 固定為 "manager"（Manager 自己打卡時傳 "user"，並省略 trace_id/parent_task_id，系統會自動生成 trace_id）。
- `{trace_id}` = Manager 自己的 trace_id（從 Manager 的 worklog JSON 讀取）。
- `{parent_task_id}` = Manager 自己的 task_id（從 Manager 的 worklog JSON 讀取）。
- **⚠️ trace_id 必帶（Governance 修補）**：派遣 worker 時，Manager 的 `trace_id` 與 `parent_task_id` 必須以**位置參數**實際帶入 `worklog.sh start`（欄位本就存在，過往 gap 在未帶入導致同一任務鏈的子 worklog 無法歸戶）。dispatch prompt 不可只留佔位字串而不替換成真值。

## 3. Memory Block

```
MEMORY: Before starting, read agents/seo/{agent-name}/memory/MEMORY.md.
Before finishing, save new learnings to agents/seo/{agent-name}/memory/ per agents/protocols/memory-protocol.md.
```

## 4. Task Block

| Field | Description |
|-------|-------------|
| `goal` | 任務目標 |
| `target_site` | 要做 SEO 的網站 / 網域 |
| `target_market` | 目標市場 / 語系（如 zh-TW、en-US） |
| `seed_topics` | 種子主題 / 業務領域 |
| `upstream_context` | 上一棒 worker 回傳的結果（非第一棒則必填，如 keyword_map 給 content-strategist） |
| `kpi_alignment` | **必填**。本任務對應「半年提升自然搜尋成效」的哪個面向（自然流量 / 排名 / 索引健康 / 轉換）及預期影響 |
| `output_format` | report \| xlsx \| docx |
| `language` | 使用者語言 |
| `output_path` | **必填**。Manager 在 session_init 建立的絕對路徑（典型 `$CLAUDE_PROJECT_DIR/output/seo/{task_id}/`）。Worker 所有產物寫入此路徑。詳見 `agents/protocols/rules/output-placement.md` |
| `task_id` | **必填**。格式 `{topic_short}-{YYYYMMDD}`，例：`keyword-map-20260530` |

> ⚠️ 若 Manager 漏傳 `output_path`，Worker 必須立即回報 `SCOPE VIOLATION: missing output_path` 並停工；不得自行寫入 agent 資料夾。

## 5. Language Block

```
LANGUAGE: Respond in {user's language}.
```

## Dispatch Order

| Step | Agents | Parallel? | Input | Output |
|------|--------|-----------|-------|--------|
| session_init | Manager 自執行 | 否（強制首步） | (無) | output_path + task_id |
| Keyword Research | keyword-researcher + shared/researcher | 平行 | parsed_request + output_path + task_id | keyword_map |
| Technical Audit | technical-auditor | 平行（與 keyword 同時） | parsed_request + output_path + task_id | audit_report |
| Analytics Baseline | analytics-reporter | 可獨立先行 | 既有數據源 + output_path + task_id | baseline_report |
| Content Strategy | content-strategist | 否（等 keyword 完成） | keyword_map + output_path + task_id | content_plan |
