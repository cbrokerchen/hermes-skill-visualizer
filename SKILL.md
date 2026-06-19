---
name: skill-visualizer
description: "Use when the user asks to visualize or explain a Hermes Skill. Reads the skill, extracts the user-facing workflow, and generates an operation-guide-style diagram via GPT-Image 2 — showing what you need, what you do step by step, and what you get."
version: 2.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [skills, visualization, diagram, gpt-image-2, user-guide, how-to]
    related_skills: [hermes-image-generation]
---

# Skill Visualizer

把任何一個 Hermes Skill 變成一張**操作手冊圖**。

不用讀長篇文件，一張圖告訴你：這個 skill 怎麼用、要準備什麼、步驟是什麼、最後得到什麼、要注意什麼。

---

## What to Expect

一張橫向步驟圖，從左到右，像 Ikea 說明書那樣：

```
事前準備 → 第一步 → 第二步 → 第三步 → 產出結果
                              ↓
                          注意事項
```

每張圖約 15-40 秒，成本約 NT$1-2。

---

## When to Use

- 「幫我畫 skill X 的使用說明」
- 「這個 skill 怎麼用？畫圖給我看」
- 「幫我把 skill X 畫成操作手冊」

**不適合：**
- Skill 內容少於 3 個段落（太簡單，沒必要畫）
- 你要的是技術架構圖，不是操作說明（用 `architecture-diagram` 或 `excalidraw`）

---

## Prerequisites

```bash
hermes config get image_gen.provider           # 應該顯示 "openai"
hermes config get image_gen.openai.model        # 應該顯示 "gpt-image-2-low"
```

如果不是：
```bash
hermes config set image_gen.provider openai
hermes config set image_gen.openai.model gpt-image-2-low
```

需要有效的 `OPENAI_API_KEY`。

---

## How It Works

一句話觸發，背後三個階段：

### ① 讀取
讀取 skill 的完整內容。

### ② 提取「使用者操作流程」

從 skill 內容中找出答案：

| 問題 | 從 skill 的哪裡找 |
|------|-----------------|
| 需要準備什麼？ | Prerequisites / Requirements / Setup 段落 |
| 怎麼啟動？ | Quick Start / Trigger / Usage 段落 |
| 步驟是什麼？ | 編號清單、Workflow 段落、Step-by-step 說明 |
| 最後得到什麼？ | Output / Results / 「你會得到…」的描述 |
| 要注意什麼？ | Pitfalls / Troubleshooting / 「注意」段落 |

每個答案濃縮成一句 ≤ 6 字的白話標籤，整張圖 5-8 個步驟。

### ③ 畫成步驟圖

把提取出的步驟轉成 prompt，告訴 GPT-Image 2：
- 從左到右排列
- 每一步一個盒子（附編號）
- 箭頭串接
- 下方放注意事項
- 全部用白話中文

---

## Prompt Template

```
A clean, easy-to-read step-by-step guide diagram on a soft warm gradient background (light cream to pale beige, not stark white). Like an IKEA instruction manual but more modern and polished.

Top-left corner: a small rounded tag labeled "[skill-name]" (dark blue #2C3E80, white text) — this is the skill's technical name.
Title centered below the tag in large friendly text: "【技能名稱】使用說明"
Below the title in medium gray text: "🔑 啟動方式：[一句話，例如「說『幫我生圖』」或「輸入 /skill xxx」]"

Horizontal layout, left to right, numbered steps connected by arrows:

Step 1: "事前準備" (orange rounded box #E67E22)
  small text below: [一句話說明需要什麼]

Step 2: "[第一步動作]" (blue rounded box #4A90D9)
  small text below: [一句話說明做什麼]

Step 3: "[第二步動作]" (blue rounded box #4A90D9)
  small text below: [一句話說明做什麼]

Step 4: "[第三步動作]" (blue rounded box #4A90D9)
  small text below: [一句話說明做什麼]

Step 5: "產出結果" (green rounded box #27AE60)
  small text below: [一句話說明得到什麼]

Below the steps: a light red section (no box needed, just text) labeled "⚠️ 注意事項" with 2-3 bullet points in small text.

Style: friendly and approachable, clean sans-serif Chinese typography, consistent spacing, no overlapping elements. The soft cream gradient background gives warmth without distraction. Overall feeling: "anyone can follow this."
```

**自訂規則**：
- 步驟數 4-6 個（太少沒資訊、太多眼花）
- 每個步驟盒上方大字（步驟名，≤ 4 字），下方小字（一句話說明，≤ 10 字）
- Step 1 永遠是「事前準備」（橙色）
- 最後一步永遠是「產出結果」（綠色）
- 中間步驟用藍色
- 注意事項用紅色文字，不用盒子包

---

## Extracting the User Journey（agent reference）

When reading a skill, look for these patterns and translate them into user-facing steps:

### Pattern 1: Prerequisites / Setup
Look for: "Prerequisites", "Requirements", "Setup", "Installation", sections listing API keys or config commands.

→ Step 1: "事前準備" — list 1-2 key requirements in plain language

### Pattern 2: Quick Start / Entry Point
Look for: "Quick Start", "Usage", "Getting Started", first code block or command.

→ Step 2: the first action the user takes

### Pattern 3: Workflow / Steps
Look for: numbered lists (1. 2. 3.), "Step 1/2/3", "Workflow" sections, sequential instructions.

→ Steps 3, 4, … — each major action as one step

### Pattern 4: Output / Result
Look for: "Output", "Results", "You will get", "Returns", final state descriptions.

→ Last step: "產出結果" — what the user ends up with

### Pattern 5: Pitfalls / Warnings
Look for: "Pitfalls", "Troubleshooting", "Common Issues", "⚠️", "Note:", "Warning:"

→ Below the steps: "⚠️ 注意事項" — 2-3 most important gotchas

### If the skill has a decision branch
If there's a clear "if X, do Y; otherwise do Z" in the user flow, insert a diamond step:
- Step N: "[判斷問題]？" (orange diamond #E67E22)
  - 是 → [Action]
  - 否 → [Alternative]

### Plain Language Translation

Always translate technical terms into everyday Chinese. Reference for common mappings:

| 技術原文 | 白話 |
|---------|------|
| Install / Setup | 安裝設定 |
| Configure | 調整選項 |
| Run / Execute | 執行 |
| API Key / Token | 申請帳號 |
| Provider / Backend | 選擇服務 |
| Generate / Create | 生成/建立 |
| Output / Result | 得到結果 |
| Cache / Save | 自動儲存 |
| Pitfall / Warning | 注意事項 |
| CLI / Terminal | 打開 terminal |

---

## Common Pitfalls

### 給使用者看的

**1. 步驟是「翻譯過的」**
skill 原文可能寫「Execute the CLI command with --flag」，圖上會標「打開 terminal 輸入指令」。這是刻意簡化——操作手冊不需要技術精確，需要一看就懂。

**2. 太複雜的 skill 會省略細節**
一個有 20 個步驟的 skill 不可能全塞進一張圖。只保留 4-6 個主要步驟，細部操作省略。

**3. 第一張可能需要微調**
步驟圖比架構圖簡單，通常一次就 ok。不滿意就調。

### 給 agent 的技術提示

**4. 先找「使用者會做什麼」，再找「系統內部做什麼」**
讀 skill 時優先提取 user-facing actions（install, run, type, click），忽略 internal logic（resolve, dispatch, cache）。

**5. 不要假設使用者認識每個工具**
GPT-Image 2 的圖上無法附上下文解釋。如果 step 裡提到「OpenAI」「Hermes」「GitHub」等品牌/工具名，非技術人可能卡住。文案盡量用「申請帳號」「AI 助手」「程式平台」這類通用說法，把品牌名放在小字而非大字。

**6. 標籤分兩層：大字（≤ 4 字）+ 小字（≤ 10 字）**
- 大字：步驟名稱，一眼掃過去就能懂
- 小字：一句話補充，給想細讀的人

**7. 不要塞太多**
4-6 個步驟最佳。超過 6 個→合併相鄰步驟。少於 3 個→這個 skill 不適合畫操作手冊。

**8. 中文很精簡，善用**
「打開 terminal 輸入 hermes chat」10 字。可以縮成「輸入指令」4 字當大字，小字補「打開 terminal 執行 hermes」。

---

## Color Palette

| 顏色 | 色碼 | 固定用途 |
|------|------|---------|
| 橙 | `#E67E22` | Step 1：事前準備 |
| 藍 | `#4A90D9` | 中間操作步驟 |
| 綠 | `#27AE60` | 最後一步：產出結果 |
| 紅 | `#E74C3C` | 注意事項（文字，不用盒子） |
| 深藍 | `#2C3E80` | 標題 |

Full prompt templates → `references/prompt-templates.md`
Plain language translation map → `references/plain-language-map.md`
Test results (real runs) → `references/test-results.md`

---

## Verification Checklist

- [ ] 從 skill 中成功提取了使用者操作流程
- [ ] 步驟數 4-6 個
- [ ] Step 1 = 事前準備（橙色）
- [ ] 最後一步 = 產出結果（綠色）
- [ ] 每個步驟有：大字（≤ 4 字）+ 小字（≤ 10 字）
- [ ] 注意事項 2-3 點
- [ ] 全部標籤是白話中文
- [ ] prompt 使用操作手冊模板
- [ ] 圖片生成成功，可讀
- [ ] 回覆附上「圖上標籤 ↔ skill 原文」對照

---

## Example

### INPUT：hermes-image-generation skill

**觸發方式**：對 Hermes 說「幫我生圖」、或輸入 `/image`、或在 Chat 中直接描述想要的圖片

**從 skill 提取的使用者流程**：

| 步驟 | 大字 | 小字 |
|------|------|------|
| 1 | 事前準備 | 申請 OpenAI 帳號，拿到 API Key |
| 2 | 設定 Hermes | 輸入兩行指令，切換到 OpenAI |
| 3 | 說出需求 | 告訴 Hermes 你要什麼圖 |
| 4 | AI 自動生成 | 等待 15-40 秒，AI 幫你畫 |
| 5 | 得到圖片 | 圖片自動儲存，可以直接用 |
| ⚠️ | 注意事項 | 圖片文字可能不完美、不滿意可以再生成 |

### OUTPUT：操作手冊圖

```
image_generate(prompt="
A clean step-by-step guide on a soft cream gradient background.

Top-left tag: [hermes-image-generation]
Title: 【AI 生圖】使用說明
啟動方式：說「幫我生圖」或描述想要的圖片

Step 1: 事前準備 — 申請 OpenAI 帳號
Step 2: 設定 Hermes — 輸入指令切換服務
Step 3: 說出需求 — 告訴 AI 你要什麼
Step 4: AI 生成 — 等待自動畫圖
Step 5: 得到圖片 — 圖片儲存直接用
⚠️ 注意事項：文字可能不完美，可重試
", aspect_ratio="landscape")
```
