# Analytics Reporter — Org

## Hierarchy

```
User
  └─ SEO Manager
       ├─ Keyword Researcher   — 關鍵字研究、搜尋意圖分析、機會評估
       ├─ Content Strategist   — 內容策略規劃、主題叢集設計
       ├─ Technical Auditor    — 技術 SEO 稽核、可索引性與效能檢查
       ├─ Analytics Reporter   ←── THIS AGENT
       └─ Shared: Calculator   — 精確數值計算，跨團隊使用
```

## Collaboration

- **SEO Manager → Analytics Reporter**: Manager 派遣成效分析任務（排名/流量/KPI 報表），傳入 `output_path` 與報告期間、KPI 目標。
- **Analytics Reporter → shared/calculator**: 任何精確數值（成長率、CTR、流量變化百分比、KPI 達成率）必須請 Manager 派遣 calculator 計算，不自行心算。
- **Analytics Reporter → SEO Manager**: 輸出成效報告（排名變化、自然流量趨勢、KPI 達成度、歸因與 A/B 觀察），供 Manager 整合策略決策。
- **Keyword Researcher / Content Strategist → Analytics Reporter**: 提供關鍵字清單與內容上線時間點，作為流量歸因與 A/B 觀察的基準輸入。

## Flow in SEO Pipeline

```
Research → Strategy → Audit → [Publish] → Analytics Reporter（追蹤成效 → 對齊 KPI → 歸因回饋）
```

成效報告回饋給 Manager，驅動下一輪研究與策略調整（閉環）。

## When to Pick This Agent

- 需要追蹤關鍵字排名變化與趨勢
- 需要分析自然流量（GA4）與搜尋表現（Google Search Console）
- 需要產出對齊半年成長目標的 KPI 報表
- 需要對流量/排名變化做成效歸因
- 需要觀察 A/B 測試或內容變更前後的成效差異

## When NOT to Pick This Agent

- 需要關鍵字研究或搜尋意圖分析 → **Keyword Researcher**
- 需要規劃內容策略或主題叢集 → **Content Strategist**
- 需要技術 SEO 稽核（爬取、索引、效能） → **Technical Auditor**
- 需要精確數值計算 → **Shared: Calculator**（由 Manager 派遣）
- 需要修改 agent 系統檔案 → **Agent Ops Team**
