1|# Plain Language Map
2|
3|Translate technical terms found in skills into everyday Chinese for operation-guide diagrams. Focus on **user-facing actions**, not system internals.
4|
5|## User Actions
6|
7|| Skill says… | Diagram label (big) | Diagram label (small) |
8||------------|-------------------|---------------------|
9|| Install / Setup / Prerequisites | 事前準備 | 申請帳號、安裝軟體 |
10|| Configure / Settings | 調整設定 | 依需求修改選項 |
11|| Run / Execute / Start | 啟動/執行 | 打開 terminal 輸入… |
12|| Type command / CLI | 輸入指令 | `hermes chat -q …` |
13|| Select / Choose / Pick | 做出選擇 | 從選單挑一個 |
14|| Wait / Processing | 等待處理 | AI 正在運作中 |
15|| Review / Check / Verify | 檢查結果 | 確認沒問題 |
16|| Output / Result / Get | 產出結果 | 得到圖片/文件/答案 |
17|| Save / Export / Download | 儲存/匯出 | 自動存到電腦 |
18|| Retry / Adjust / Iterate | 調整重試 | 不滿意可以再來 |
19|
20|## Warnings & Notes
21|
22|| Skill says… | Diagram label |
23||------------|--------------|
24|| Pitfalls / Troubleshooting | 注意事項 |
25|| Common Issues / FAQ | 常見問題 |
26|| Warning / Caution / Note | ⚠️ 提醒 |
27|| Limitation / Known Issue | 已知限制 |
28|
29|## Concepts (when a step IS a concept)
30|
31|| Skill says… | Plain Chinese |
32||------------|--------------|
33|| API Key / Token / Credential | 帳號密碼 / 金鑰 |
34|| Provider / Backend / Service | 服務 / 平台 |
35|| Model / Engine | AI 模型 |
36|| Configuration file | 設定檔 |
37|| Terminal / CLI / Shell | terminal |
38|| Cache / Temp storage | 暫存區 |
39|
40|## Translation Rules
41|
42|1. **Action-first**: labels should be verbs or verb phrases (做什麼), not nouns (是什麼)
43|2. **Everyday words**: use words a non-technical person would use in daily life
44|3. **Short**: big label ≤ 4 chars, small label ≤ 10 chars
45|4. **Merge similar steps**: "Install + Configure + Setup" → one "事前準備" step
46|5. **Skip internal steps**: "resolve provider", "dispatch event", "cache result" → these are NOT user actions, don't include them
47|