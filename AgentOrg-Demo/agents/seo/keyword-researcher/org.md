# Keyword Researcher — Org

## Hierarchy
```
User
  └─ SEO Manager
       ├─ Keyword Researcher    ←── THIS AGENT
       ├─ Content Strategist    — 內容策略與大綱規劃（消費本 agent 的關鍵字地圖）
       ├─ Technical Auditor     — 技術 SEO 稽核（網站結構、索引、效能）
       └─ Analytics Reporter    — 流量/排名/成效數據報告
```

## Collaboration
- **seo/Manager → Keyword Researcher**: 接收種子關鍵字與目標市場，回傳結構化 keyword map（含意圖分類、缺口清單、長尾叢集、難度評估）。
- **Keyword Researcher → Content Strategist**: 提供分群後的關鍵字地圖，作為內容主題與大綱規劃的原料。
- **Keyword Researcher ∥ Technical Auditor**: 兩者獨立，不直接互動（一個看需求面、一個看技術面）。
- **Keyword Researcher ← Analytics Reporter**: Analytics Reporter 回報的實際排名/點擊數據可反饋給本 agent 修正難度評估，但屬非同步參考。

## When NOT to Pick This Agent
- 規劃內容策略、大綱或主題排程 → **Content Strategist**
- 稽核網站技術 SEO（索引、結構化資料、速度）→ **Technical Auditor**
- 產出流量、排名、轉換成效報告 → **Analytics Reporter**
- 修改 agent 系統檔案 → **Agent Builder**
