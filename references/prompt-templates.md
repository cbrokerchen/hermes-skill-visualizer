1|# User Journey Prompt Templates
2|
3|All diagrams use a single format: **horizontal step-by-step operation guide** on a warm gradient background, with skill name tag and trigger keywords.
4|
5|---
6|
7|## Base Template
8|
9|```
10|A clean, easy-to-read step-by-step guide diagram on a soft warm gradient background (light cream to pale beige, not stark white). Like an IKEA instruction manual but more modern and polished.
11|
12|Top-left corner: a small rounded tag labeled "[skill-name]" (dark blue #2C3E80, white text) — the skill's technical identifier.
13|Title centered below the tag in large friendly text: "【顯示名稱】使用說明"
14|Below the title in medium gray text: "🔑 啟動方式：[一句話說明如何觸發]"
15|
16|Horizontal layout, left to right, numbered steps connected by arrows:
17|
18|Step 1: "事前準備" (orange rounded box #E67E22)
19|  small descriptive text below: [一句話，≤ 10 字]
20|
21|Step 2: "[動作]" (blue rounded box #4A90D9)
22|  small descriptive text below: [一句話]
23|
24|Step 3: "[動作]" (blue rounded box #4A90D9)
25|  small descriptive text below: [一句話]
26|
27|Step 4: "[動作]" (blue rounded box #4A90D9)
28|  small descriptive text below: [一句話]
29|
30|Step 5: "產出結果" (green rounded box #27AE60)
31|  small descriptive text below: [一句話]
32|
33|Below all the steps: "⚠️ 注意事項" in red text (#E74C3C) with 2-3 bullet points, no surrounding box.
34|
35|Style: friendly and approachable, clean sans-serif Chinese typography, consistent spacing, no overlapping elements. The soft cream gradient gives warmth without distraction.
36|```
37|
38|---
39|
40|## How to fill [skill-name]
41|
42|Use the skill's actual name from its frontmatter (e.g., `hermes-image-generation`, `github-pr-workflow`). This goes in the top-left tag.
43|
44|## How to fill 【顯示名稱】
45|
46|A human-friendly Chinese name for the skill. Not the technical name. Examples:
47|- `hermes-image-generation` → "AI 生圖"
48|- `github-pr-workflow` → "GitHub 發 PR"
49|- `obsidian` → "筆記管理"
50|
51|## How to fill 🔑 啟動方式
52|
53|Look in the skill for:
54|- Slash commands (`/image`, `/skill name`)
55|- Natural language triggers ("說「幫我生圖」")
56|- Tool names (`image_generate`)
57|- CLI commands (`hermes chat -q ...`)
58|
59|Format: short, actionable, ≤ 15 chars. Examples:
60|- "說「幫我生圖」"
61|- "輸入 /skill xxx"
62|- "直接描述想要的圖片"
63|- "執行 hermes chat"
64|
65|## Variations
66|
67|### Fewer steps (4 steps)
68|Remove one middle step. Keep: 事前準備 → 動作 → 動作 → 產出結果.
69|
70|### More steps (6 steps)
71|Add one more blue step. Max 6.
72|
73|### With decision branch
74|Replace one blue step:
75|
76|```
77|Step N: "[判斷問題]？" (orange diamond #E67E22)
78|  small text: "是→[A] / 否→[B]"
79|```
80|
81|---
82|
83|## Color Rules (fixed)
84|
85|| Element | Color | Hex |
86||---------|-------|-----|
87|| Skill name tag | Dark blue bg, white text | #2C3E80 |
88|| Title | Dark blue | #2C3E80 |
89|| 啟動方式 text | Medium gray | #666666 |
90|| Step 1 (事前準備) | Orange | #E67E22 |
91|| Middle steps | Blue | #4A90D9 |
92|| Decision diamond | Orange | #E67E22 |
93|| Last step (產出結果) | Green | #27AE60 |
94|| 注意事項 text | Red | #E74C3C |
95|| Background | Cream-to-beige gradient | — |
96|