# Skill Visualizer — Hermes 技能圖解工具

把任何一個 [Hermes Agent](https://hermes-agent.nousresearch.com) Skill 變成一張**操作手冊圖**。

不用讀長篇文件，一張圖告訴你：這個 skill 怎麼用、要準備什麼、步驟是什麼、最後得到什麼、要注意什麼。

## 預覽

```
事前準備 → 第一步 → 第二步 → 第三步 → 產出結果
                              ↓
                          注意事項
```

每張圖約 15-40 秒生成，像 Ikea 說明書那樣，從左到右的步驟圖。

## 安裝

將本 repo 的 skill 檔案放到 Hermes 的 skills 目錄：

```bash
cp SKILL.md ~/.hermes/skills/creative/skill-visualizer/SKILL.md
cp -r references/ ~/.hermes/skills/creative/skill-visualizer/references/
```

或直接 clone：

```bash
mkdir -p ~/.hermes/skills/creative/skill-visualizer
git clone https://github.com/cbrokerchen/hermes-skill-visualizer.git ~/.hermes/skills/creative/skill-visualizer
```

## 使用方式

對 Hermes 說：

- 「幫我畫 skill X 的使用說明」
- 「這個 skill 怎麼用？畫圖給我看」
- 「幫我把 skill X 畫成操作手冊」

## 需求

- Hermes Agent（已安裝）
- OpenAI API Key
- GPT-Image 2 模型（`gpt-image-2-low`）

## 檔案結構

```
SKILL.md                          # 主 skill 檔案
references/
  prompt-templates.md             # 完整 prompt 模板
  plain-language-map.md           # 白話翻譯對照表
  test-results.md                 # 實測結果記錄
```

## License

MIT
