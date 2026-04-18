# Book Compiler

> AI-powered knowledge compilation assistant — turn a topic, a reading list, or source materials into a structured Markdown e-book.

**Book Compiler** 是一個 LLM 無關（LLM-agnostic）的 skill / prompt，幫你把研究結果整理成類似電子書的 Markdown 集合。適用於個人知識管理、主題綜述、讀書筆記、研究整理等場景。

## ⚠️ 重要聲明

本工具產出的是摘要 / 評論 / 主題綜述，是 AI 根據公開 metadata、網路資料與模型知識整理而成的衍生作品，**不能取代原書**。

詳見 [DISCLAIMER.md](./DISCLAIMER.md)。

## 特色

- **LLM 無關**：Claude Code、Claude API、ChatGPT、Gemini、Cursor、Cline 等皆可使用
- **結構化流程**：研究 → 大綱 → 撰寫 → 審稿 → 組裝，五階段可中斷可恢復
- **來源透明**：每章明確標示 📖 書籍摘要 / 🌐 網路資料 / 🤖 AI 生成
- **在地化支援**：透過 `profiles/<locale>.yaml` 定義語系用語，預設提供 `zh-TW`
- **著作權友善**：預設旁白敘事避免仿作、強制重新命名書名、合理使用引用上限、公版書模式
- **多模式**：`--outline` 僅大綱 / `--chapter` 單章 / `--self-review` 草稿自審 / `--public-domain-only` 公版書安全模式

## 快速開始

### Claude Code
1. 把 `SKILL.md` 放到專案的 `.claude/skills/book-compiler/SKILL.md`
2. 執行：`/book-compiler 你的主題`

### ChatGPT / GPTs
1. 建立新 GPT，把 `SKILL.md` 內容貼到 Instructions
2. 啟用 Web Browsing
3. 直接輸入主題即可

### Gemini（AI Studio / Gemini CLI）
1. 把 `SKILL.md` 當 system instruction
2. 啟用 Google Search grounding
3. 輸入主題

### Cursor / Cline / Continue
1. 將 `SKILL.md` 放入 rules 或 custom mode
2. 呼叫時觸發

### Claude API（含 Agent SDK）
1. 當 system prompt
2. 自行實作 `web_search`、`web_fetch`、檔案 I/O、shell 等 tool

## 設定

複製 `config.example.yaml` 為 `config.yaml`，依需求調整：

```yaml
output_dir: ./books
language: zh-TW
localization_profile: zh-TW
chapter_length:
  min: 1500
  max: 3000
quote_limits:
  enabled: true
  max_quote_chars: 150
  max_quote_ratio: 0.10
public_domain_only: false
narrative_style: narrator
```

## 使用範例

```bash
# 主題型
/book-compiler 投資心理學入門

# 多書綜合
/book-compiler --from-books "Poor Charlie's Almanack, Thinking Fast and Slow"

# 僅產出大綱
/book-compiler --outline AI 時代的軟體工程

# 公版書安全模式（推薦新手）
/book-compiler --public-domain-only Henry David Thoreau nature writing

# 對既有草稿自審
/book-compiler --self-review ./books/my-draft/
```

## 目錄結構

```
book-compiler/
├── SKILL.md                 # 主提示詞（此 skill 的核心）
├── README.md                # 本檔
├── LICENSE                  # MIT
├── DISCLAIMER.md            # 著作權 / 合理使用聲明
├── config.example.yaml      # 設定範本
└── profiles/
    └── zh-TW.yaml           # 台灣在地化 profile
```

## Contributing

歡迎貢獻：

- 新語系的 `profiles/<locale>.yaml`（zh-CN、zh-HK、en、ja、ko ……）
- 公版書範例產出（`examples/`）
- 多 LLM 環境的整合說明與調整建議
- 品質檢查規則與 heuristics

## License

- 提示詞與流程：[MIT](./LICENSE)
- 範例產出：CC-BY-SA 4.0
- 使用者自行產出的書籍：使用者自行決定授權

## Acknowledgements

Inspired by the original private-use `book-compiler` skill that served personal knowledge compilation. Open-sourced with legal-safety hardening and LLM-agnostic design.
