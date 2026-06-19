1|---
2|name: skill-visualizer
3|description: "Use when the user asks to visualize or explain a Hermes Skill. Reads the skill, extracts the user-facing workflow, and generates an operation-guide-style diagram via GPT-Image 2 — showing what you need, what you do step by step, and what you get."
4|version: 2.1.0
5|author: Hermes Agent
6|license: MIT
7|metadata:
8|  hermes:
9|    tags: [skills, visualization, diagram, gpt-image-2, user-guide, how-to]
10|    related_skills: [hermes-image-generation]
11|---
12|
13|# Skill Visualizer
14|
15|把任何一個 Hermes Skill 變成一張**操作手冊圖**。
16|
17|不用讀長篇文件，一張圖告訴你：這個 skill 怎麼用、要準備什麼、步驟是什麼、最後得到什麼、要注意什麼。
18|
19|---
20|
21|## What to Expect
22|
23|一張橫向步驟圖，從左到右，像 Ikea 說明書那樣：
24|
25|```
26|事前準備 → 第一步 → 第二步 → 第三步 → 產出結果
27|                              ↓
28|                          注意事項
29|```
30|
31|每張圖約 15-40 秒，成本約 NT$1-2。
32|
33|---
34|
35|## When to Use
36|
37|- 「幫我畫 skill X 的使用說明」
38|- 「這個 skill 怎麼用？畫圖給我看」
39|- 「幫我把 skill X 畫成操作手冊」
40|
41|**不適合：**
42|- Skill 內容少於 3 個段落（太簡單，沒必要畫）
43|- 你要的是技術架構圖，不是操作說明（用 `architecture-diagram` 或 `excalidraw`）
44|
45|---
46|
47|## Prerequisites
48|
49|```bash
50|hermes config get image_gen.provider           # 應該顯示 "openai"
51|hermes config get image_gen.openai.model        # 應該顯示 "gpt-image-2-medium"
52|```
53|
54|如果不是：
55|```bash
56|hermes config set image_gen.provider openai
57|hermes config set image_gen.openai.model gpt-image-2-medium
58|```
59|
60|需要有效的 `OPENAI_API_KEY`。
61|
62|---
63|
64|## How It Works
65|
66|一句話觸發，背後三個階段：
67|
68|### ① 讀取
69|讀取 skill 的完整內容。
70|
71|### ② 提取「使用者操作流程」
72|
73|從 skill 內容中找出答案：
74|
75|| 問題 | 從 skill 的哪裡找 |
76||------|-----------------|
77|| 需要準備什麼？ | Prerequisites / Requirements / Setup 段落 |
78|| 怎麼啟動？ | Quick Start / Trigger / Usage 段落 |
79|| 步驟是什麼？ | 編號清單、Workflow 段落、Step-by-step 說明 |
80|| 最後得到什麼？ | Output / Results / 「你會得到…」的描述 |
81|| 要注意什麼？ | Pitfalls / Troubleshooting / 「注意」段落 |
82|
83|每個答案濃縮成一句 ≤ 6 字的白話標籤，整張圖 5-8 個步驟。
84|
85|### ③ 畫成步驟圖
86|
87|把提取出的步驟轉成 prompt，告訴 GPT-Image 2：
88|- 從左到右排列
89|- 每一步一個盒子（附編號）
90|- 箭頭串接
91|- 下方放注意事項
92|- 全部用白話中文
93|
94|---
95|
96|## Prompt Template
97|
98|```
99|A clean, easy-to-read step-by-step guide diagram on a soft warm gradient background (light cream to pale beige, not stark white). Like an IKEA instruction manual but more modern and polished.
100|
101|Top-left corner: a small rounded tag labeled "[skill-name]" (dark blue #2C3E80, white text) — this is the skill's technical name.
102|Title centered below the tag in large friendly text: "【技能名稱】使用說明"
103|Below the title in medium gray text: "🔑 啟動方式：[一句話，例如「說『幫我生圖』」或「輸入 /skill xxx」]"
104|
105|Horizontal layout, left to right, numbered steps connected by arrows:
106|
107|Step 1: "事前準備" (orange rounded box #E67E22)
108|  small text below: [一句話說明需要什麼]
109|
110|Step 2: "[第一步動作]" (blue rounded box #4A90D9)
111|  small text below: [一句話說明做什麼]
112|
113|Step 3: "[第二步動作]" (blue rounded box #4A90D9)
114|  small text below: [一句話說明做什麼]
115|
116|Step 4: "[第三步動作]" (blue rounded box #4A90D9)
117|  small text below: [一句話說明做什麼]
118|
119|Step 5: "產出結果" (green rounded box #27AE60)
120|  small text below: [一句話說明得到什麼]
121|
122|Below the steps: a light red section (no box needed, just text) labeled "⚠️ 注意事項" with 2-3 bullet points in small text.
123|
124|Style: friendly and approachable, clean sans-serif Chinese typography, consistent spacing, no overlapping elements. The soft cream gradient background gives warmth without distraction. Overall feeling: "anyone can follow this."
125|```
126|
127|**自訂規則**：
128|- 步驟數 4-6 個（太少沒資訊、太多眼花）
129|- 每個步驟盒上方大字（步驟名，≤ 4 字），下方小字（一句話說明，≤ 10 字）
130|- Step 1 永遠是「事前準備」（橙色）
131|- 最後一步永遠是「產出結果」（綠色）
132|- 中間步驟用藍色
133|- 注意事項用紅色文字，不用盒子包
134|
135|---
136|
137|## Extracting the User Journey（agent reference）
138|
139|When reading a skill, look for these patterns and translate them into user-facing steps:
140|
141|### Pattern 1: Prerequisites / Setup
142|Look for: "Prerequisites", "Requirements", "Setup", "Installation", sections listing API keys or config commands.
143|
144|→ Step 1: "事前準備" — list 1-2 key requirements in plain language
145|
146|### Pattern 2: Quick Start / Entry Point
147|Look for: "Quick Start", "Usage", "Getting Started", first code block or command.
148|
149|→ Step 2: the first action the user takes
150|
151|### Pattern 3: Workflow / Steps
152|Look for: numbered lists (1. 2. 3.), "Step 1/2/3", "Workflow" sections, sequential instructions.
153|
154|→ Steps 3, 4, … — each major action as one step
155|
156|### Pattern 4: Output / Result
157|Look for: "Output", "Results", "You will get", "Returns", final state descriptions.
158|
159|→ Last step: "產出結果" — what the user ends up with
160|
161|### Pattern 5: Pitfalls / Warnings
162|Look for: "Pitfalls", "Troubleshooting", "Common Issues", "⚠️", "Note:", "Warning:"
163|
164|→ Below the steps: "⚠️ 注意事項" — 2-3 most important gotchas
165|
166|### If the skill has a decision branch
167|If there's a clear "if X, do Y; otherwise do Z" in the user flow, insert a diamond step:
168|- Step N: "[判斷問題]？" (orange diamond #E67E22)
169|  - 是 → [Action]
170|  - 否 → [Alternative]
171|
172|### Plain Language Translation
173|
174|Always translate technical terms into everyday Chinese. Reference for common mappings:
175|
176|| 技術原文 | 白話 |
177||---------|------|
178|| Install / Setup | 安裝設定 |
179|| Configure | 調整選項 |
180|| Run / Execute | 執行 |
181|| API Key / Token | 申請帳號 |
182|| Provider / Backend | 選擇服務 |
183|| Generate / Create | 生成/建立 |
184|| Output / Result | 得到結果 |
185|| Cache / Save | 自動儲存 |
186|| Pitfall / Warning | 注意事項 |
187|| CLI / Terminal | 打開 terminal |
188|
189|---
190|
191|## Common Pitfalls
192|
193|### 給使用者看的
194|
195|**1. 步驟是「翻譯過的」**
196|skill 原文可能寫「Execute the CLI command with --flag」，圖上會標「打開 terminal 輸入指令」。這是刻意簡化——操作手冊不需要技術精確，需要一看就懂。
197|
198|**2. 太複雜的 skill 會省略細節**
199|一個有 20 個步驟的 skill 不可能全塞進一張圖。只保留 4-6 個主要步驟，細部操作省略。
200|
201|**3. 第一張可能需要微調**
202|步驟圖比架構圖簡單，通常一次就 ok。不滿意就調。
203|
204|### 給 agent 的技術提示
205|
206|**4. 先找「使用者會做什麼」，再找「系統內部做什麼」**
207|讀 skill 時優先提取 user-facing actions（install, run, type, click），忽略 internal logic（resolve, dispatch, cache）。
208|
209|**5. 不要假設使用者認識每個工具**
210|GPT-Image 2 的圖上無法附上下文解釋。如果 step 裡提到「OpenAI」「Hermes」「GitHub」等品牌/工具名，非技術人可能卡住。文案盡量用「申請帳號」「AI 助手」「程式平台」這類通用說法，把品牌名放在小字而非大字。
211|
212|**6. 標籤分兩層：大字（≤ 4 字）+ 小字（≤ 10 字）**
213|- 大字：步驟名稱，一眼掃過去就能懂
214|- 小字：一句話補充，給想細讀的人
215|
216|**7. 不要塞太多**
217|4-6 個步驟最佳。超過 6 個→合併相鄰步驟。少於 3 個→這個 skill 不適合畫操作手冊。
218|
219|**8. 中文很精簡，善用**
220|「打開 terminal 輸入 hermes chat」10 字。可以縮成「輸入指令」4 字當大字，小字補「打開 terminal 執行 hermes」。
221|
222|---
223|
224|## Color Palette
225|
226|| 顏色 | 色碼 | 固定用途 |
227||------|------|---------|
228|| 橙 | `#E67E22` | Step 1：事前準備 |
229|| 藍 | `#4A90D9` | 中間操作步驟 |
230|| 綠 | `#27AE60` | 最後一步：產出結果 |
231|| 紅 | `#E74C3C` | 注意事項（文字，不用盒子） |
232|| 深藍 | `#2C3E80` | 標題 |
233|
234|Full prompt templates → `references/prompt-templates.md`
235|Plain language translation map → `references/plain-language-map.md`
236|Test results (real runs) → `references/test-results.md`
237|
238|---
239|
240|## Verification Checklist
241|
242|- [ ] 從 skill 中成功提取了使用者操作流程
243|- [ ] 步驟數 4-6 個
244|- [ ] Step 1 = 事前準備（橙色）
245|- [ ] 最後一步 = 產出結果（綠色）
246|- [ ] 每個步驟有：大字（≤ 4 字）+ 小字（≤ 10 字）
247|- [ ] 注意事項 2-3 點
248|- [ ] 全部標籤是白話中文
249|- [ ] prompt 使用操作手冊模板
250|- [ ] 圖片生成成功，可讀
251|- [ ] 回覆附上「圖上標籤 ↔ skill 原文」對照
252|
253|---
254|
255|## Example
256|
257|### INPUT：hermes-image-generation skill
258|
259|**觸發方式**：對 Hermes 說「幫我生圖」、或輸入 `/image`、或在 Chat 中直接描述想要的圖片
260|
261|**從 skill 提取的使用者流程**：
262|
263|| 步驟 | 大字 | 小字 |
264||------|------|------|
265|| 1 | 事前準備 | 申請 OpenAI 帳號，拿到 API Key |
266|| 2 | 設定 Hermes | 輸入兩行指令，切換到 OpenAI |
267|| 3 | 說出需求 | 告訴 Hermes 你要什麼圖 |
268|| 4 | AI 自動生成 | 等待 15-40 秒，AI 幫你畫 |
269|| 5 | 得到圖片 | 圖片自動儲存，可以直接用 |
270|| ⚠️ | 注意事項 | 圖片文字可能不完美、不滿意可以再生成 |
271|
272|### OUTPUT：操作手冊圖
273|
274|```
275|image_generate(prompt="
276|A clean step-by-step guide on a soft cream gradient background.
277|
278|Top-left tag: [hermes-image-generation]
279|Title: 【AI 生圖】使用說明
280|啟動方式：說「幫我生圖」或描述想要的圖片
281|
282|Step 1: 事前準備 — 申請 OpenAI 帳號
283|Step 2: 設定 Hermes — 輸入指令切換服務
284|Step 3: 說出需求 — 告訴 AI 你要什麼
285|Step 4: AI 生成 — 等待自動畫圖
286|Step 5: 得到圖片 — 圖片儲存直接用
287|⚠️ 注意事項：文字可能不完美，可重試
288|", aspect_ratio="landscape")
289|```
290|