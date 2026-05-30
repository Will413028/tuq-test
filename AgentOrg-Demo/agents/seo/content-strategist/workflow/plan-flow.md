# Content Strategist — Plan Flow

R-D-V 第二段：Plan。將 research_summary 轉化為主題叢集、內容日曆與 on-page 規格藍圖。由 workflow.yaml route 到此。

## 流程步驟

### 1. 建立主題叢集架構
依 keyword_groups 與 intent_map 規劃 pillar + cluster 網狀結構。
```
topic_clusters:
  - pillar:
      title: [pillar 標題]
      target_keyword: [head term]
      intent: [意圖]
      url_slug: [slug]
    clusters:
      - title: [cluster 標題]
        target_keyword: [long-tail]
        intent: [意圖]
        internal_link_to: [pillar / 其他 cluster]
```
- 每個 cluster 必須標注內鏈方向（指向 pillar 並彼此串連）。

### 2. 規劃內容日曆
依叢集優先級、意圖（高商業意圖優先）、季節性排定發佈順序。
```
content_calendar:
  - publish_date: [日期]
    title: [標題]
    cluster: [所屬 pillar]
    target_keyword: [關鍵字]
    intent: [意圖]
    owner_role: [寫手/角色]
    priority: [P0/P1/P2]
```
- 優先發佈 pillar 與高商業意圖內容，cluster 隨後填充強化叢集權威。

### 3. 撰寫 on-page 規格
為每篇內容產出可執行規格（非建議）。
```
onpage_spec:
  - page: [標題]
    title_tag: [≤60 字元，含主關鍵字]
    meta_description: [≤155 字元，含意圖誘因]
    h_structure: [H1（唯一）/ H2 / H3 語意層級]
    internal_links: [{ anchor_text, target_url }]
    url_slug: [slug]
```

### 4. 設計 E-E-A-T 信任訊號
為每篇標注如何展現 Experience / Expertise / Authoritativeness / Trustworthiness。
- YMYL（健康/金融/法律）主題強制標記額外信任要求（作者 bio、引用來源、更新日期、署名）。

## 輸出（傳遞給 Execute 段）
完整策略藍圖物件：`topic_clusters` + `content_calendar` + `onpage_spec` + `eeat_signals`。

## 注意事項
- title/meta 字元上限與內鏈數量等精確統計請求 `shared/calculator`，不自行心算。
- 標題與 meta 避免 keyword stuffing，兼顧點擊誘因與可讀性。
