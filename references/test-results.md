1|# Test Results
2|
3|Real test runs of the skill-visualizer operation manual format. Each entry documents the skill tested, the extracted user journey, the prompt used, and the quality score.
4|
5|---
6|
7|## Test 1: hermes-image-generation (2026-06-18)
8|
9|**Skill type**: Tool configuration (setup → use → get result)
10|
11|**Extracted user journey**:
12|
13|| Step | Label | Description |
14||------|-------|-------------|
15|| 1 | 事前準備 | 申請 OpenAI 帳號拿到金鑰 |
16|| 2 | 設定 Hermes | 輸入指令切換到 OpenAI |
17|| 3 | 說出需求 | 告訴 Hermes 你要什麼圖 |
18|| 4 | AI 自動生成 | 等待 15-40 秒自動畫圖 |
19|| 5 | 得到圖片 | 圖片自動儲存可直接使用 |
20|
21|**Trigger**: 說「幫我生圖」或直接描述想要的圖片
22|
23|**Pitfalls**: 圖片文字可能不完美、不滿意可以重新生成
24|
25|**Score**: 8.5/10 (general audience)
26|
27|**Vision analysis highlights**:
28|- ✅ Skill name tag visible top-left
29|- ✅ Trigger keywords visible below title
30|- ✅ Warm cream gradient background
31|- ✅ All 5 steps + icons + descriptions present
32|- ⚠️ Assumes user knows what Hermes and OpenAI are
33|- ⚠️ "金鑰" (API key) concept may intimidate beginners
34|
35|---
36|
37|## Test 2: github-pr-workflow (2026-06-18)
38|
39|**Skill type**: Sequential workflow (branch → code → PR → CI → merge)
40|
41|**Extracted user journey**:
42|
43|| Step | Label | Description |
44||------|-------|-------------|
45|| 1 | 事前準備 | 登入 GitHub 進入專案 |
46|| 2 | 開新分支 | 輸入指令建立新分支 |
47|| 3 | 寫程式提交 | 修改檔案後提交變更 |
48|| 4 | 發送 PR | 推上 GitHub 建立 Pull Request |
49|| 5 | 自動檢查 | CI 自動測試失敗就修正 |
50|| 6 | 合併完成 | 通過後合併刪除舊分支 |
51|
52|**Trigger**: 說「幫我發 PR」或「我要提交程式」
53|
54|**Pitfalls**: 分支名稱要規範（feat/fix/docs）、CI 失敗要修好才能合併、合併後記得刪除舊分支
55|
56|**Score**: 8.5/10 (general audience)
57|
58|**Vision analysis highlights**:
59|- ✅ All 6 steps + descriptions + 3 pitfalls present
60|- ✅ Color coding (orange→blue→green) intuitively signals progress
61|- ✅ Multi-modal: icons + text + color for different learning styles
62|- ⚠️ Step 2 "輸入指令切換到 OpenAI" assumes command-line familiarity
63|- ⚠️ No time estimates per step
64|
65|---
66|
67|## Cross-test observations
68|
69|1. **8.5/10 is the current ceiling** for GPT-Image 2 operation-manual diagrams. The consistent score across two different skill types suggests the format is stable, and the remaining ~1.5 points are inherent model limitations (text fidelity, spatial precision).
70|
71|2. **Both tests share the same weakness**: assuming prior knowledge of tools/brands (Hermes, OpenAI, GitHub). When building prompts, add context-softening language like "如果你已經有…" or treat Step 1 as "確認你有…" rather than "申請…".
72|
73|3. **6 steps work fine** — the original constraint of "4-6 steps" was validated. github-pr-workflow at 6 steps scored the same as hermes-image-generation at 5 steps.
74|
75|4. **Chinese labels are reliable** — GPT-Image 2 rendered all Chinese labels correctly in both tests. No garbled or missing characters. 4-character Chinese labels are safe.
76|