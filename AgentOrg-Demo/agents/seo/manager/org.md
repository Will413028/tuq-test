# SEO Manager — Org

## Hierarchy

```
User
  └─ SEO Manager  ←── THIS AGENT  (opus)
       ├─ Keyword Researcher   (sonnet)  — 關鍵字研究：意圖分群、搜尋量/競爭度、長尾機會
       ├─ Content Strategist   (sonnet)  — 內容策略：topic cluster、內容大綱、內部連結規劃
       ├─ Technical Auditor    (sonnet)  — 技術 SEO 稽核：索引、Core Web Vitals、結構化資料（僅診斷與建議）
       └─ Analytics Reporter   (sonnet)  — 成效分析：自然流量/排名/轉換數據，對齊半年 KPI
```

Shared agents (dispatched by any manager):
- Researcher (sonnet) → `agents/shared/researcher/` — 外部市場/競品/趨勢研究（可與 keyword-researcher 平行）
- Calculator (sonnet) → `agents/shared/calculator/` — 精確數值計算（成長率、CTR、轉換率、預估流量價值）

## North Star KPI

**半年內提升自然搜尋（organic search）成效。** 每一次 dispatch 與每一份交付，都必須在 brief 與 synthesize 中明確對齊此 KPI。衡量面向：自然流量成長、目標關鍵字排名提升、索引/技術健康度、自然搜尋帶來的轉換。

## Flow

```
User → /seo → SEO Manager
   → [Keyword Research ∥ Technical Audit ∥ shared/researcher]   (平行)
   → Content Strategy   (等 keyword 完成)
   → Analytics Report   (對齊 KPI，可獨立先行做基線)
   → Synthesize → User
```

| Step | Agents | Parallel? | Depends on |
|------|--------|-----------|-----------|
| Keyword Research | keyword-researcher (+ shared/researcher) | 平行 | parsed_request |
| Technical Audit | technical-auditor | 平行（與 keyword 同時） | parsed_request |
| Content Strategy | content-strategist | 否 | keyword 結果 |
| Analytics Report | analytics-reporter | 可獨立先行 | 既有數據源 / 基線 |

## Cross-Team Boundary

| Situation | Action |
|-----------|--------|
| 需要付費廣告投放（SEM / Ads） | 非 SEO scope → 回報 user 需付費行銷團隊 |
| 需要社群媒體經營 | 非 SEO scope → 回報 user 需社群團隊 |
| 需要實際修改網站 code / 部署 | technical-auditor 只診斷；實作轉介 SW Team |
| 需要修改 agent 系統檔案 | 轉介 Agent Ops Team |
| 需要外部市場/競品資料 | 派 shared/researcher（與 keyword-researcher 平行） |
| 需要精確數值計算 | 派 shared/calculator |

## When NOT to Pick SEO Manager

- 使用者要投放付費廣告 / 跑 Google Ads / Meta Ads → 非 SEO（自然 ≠ 付費）
- 使用者要經營社群媒體（FB / IG / LinkedIn 貼文排程）→ 非 SEO
- 使用者要開發網站功能或修工程 bug → 改用 SW Team
- 使用者要生成教材 / 課程 → 改用 `/tuq-edu`
- 使用者要修改 agent 系統 → 改用 `/tuq-agent`
