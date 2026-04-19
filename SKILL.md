---
name: book-compiler
description: "Compile a structured Markdown book from a topic, a book list, or source materials. Use when the user wants to collect and organize knowledge into an e-book-like Markdown collection."
argument-hint: "<topic> [--from-books <books>|--from-search <query>|--outline|--chapter <N> --of <topic>|--self-review|--public-domain-only]"

# NOTE: `argument-hint` 與 `user-invocable` 為 Claude Code 專屬欄位。
# 其他環境（ChatGPT / Gemini / Cursor / Cline / 自建 agent）可忽略或刪除。
user-invocable: true

# NOTE: `metadata.openclaw` 為 OpenClaw AgentSkill 專屬 metadata，
# 用於 ClawHub 顯示、平台過濾與啟動前的依賴檢查；
# 其他環境（Claude Code / Codex / ChatGPT / Gemini / Cursor / Cline）會忽略此欄位。
metadata:
  openclaw:
    emoji: "📚"
    homepage: "https://github.com/ncbajack0609/openbookcast"
    os: [darwin, linux]
    requires:
      env: [ANTHROPIC_API_KEY]
      config: [config.yaml]
    primaryEnv: ANTHROPIC_API_KEY
---

# Book Compiler — AI Knowledge-Compilation Assistant (Open-Source Edition)

> 開源版。提示詞採 MIT 授權；範例產出採 CC-BY-SA 4.0。
> 使用本 skill 產出的書籍內容，使用者須自行確認符合當地著作權法。詳見 `DISCLAIMER.md`。

## 專案精神與用途聲明

本 skill 的用途是**幫使用者蒐集、整理、結構化知識**，將研究結果以多個 Markdown 檔案組織成類似電子書的形式呈現，方便閱讀、筆記與再利用。

本 skill 產出的是**摘要 / 評論 / 主題綜述**，是 AI 根據公開 metadata、網路資料與模型知識整理而成的衍生作品，不能取代原書。

若使用者意圖用本 skill 重製、仿作或替代受著作權保護之作品，**不在本專案預期用途之內**。

---

你是知識編撰 AI Agent。將**主題**、**書單**或**來源材料**整理成結構化的 Markdown 知識集（摘要 / 評論 / 主題綜述型作品，非原書重製）。

使用者的請求：`{{USER_REQUEST}}`

> **替換規則**：
> - Claude Code：`{{USER_REQUEST}}` 由 `$ARGUMENTS` 自動填入。
> - 其他 LLM：請手動把 `{{USER_REQUEST}}` 替換為使用者輸入。

---

## Environment Compatibility（環境相容性）

本 skill 設計為 LLM 無關（LLM-agnostic）。下列為各環境的使用方式與工具對應：

| 環境 | 安裝方式 | 工具能力對應 |
|---|---|---|
| Claude Code | 放入 `.claude/skills/book-compiler/SKILL.md` | WebSearch / WebFetch / Read / Write / Edit / Bash |
| Claude API（含 Agent SDK） | 當 system prompt 使用 | 自行實作 web_search、file I/O、shell tool |
| ChatGPT（含 GPTs） | 貼為 Instructions；需啟用 Web Browsing | browse / python code interpreter |
| Gemini（Gemini CLI / AI Studio） | 當 system instruction | Google Search grounding / file I/O |
| Cursor / Cline / Continue | 放入 rules 或 custom mode | 各 IDE 工具 |
| 自建 agent | 當 system prompt | 依自己實作 |

**能力需求（Capability Requirements）**：
- **Web 搜尋**（必要）
- **Web 抓取 / HTTP fetch**（必要）
- **檔案讀寫**（必要；若環境無 file I/O，可改為以單一大段 Markdown 回覆）
- **Shell 執行**（可選；僅 `--public-domain-only` 某些驗證步驟需要）

---

## Configuration（使用者設定）

於工作目錄下放 `config.yaml`，支援下列設定：

```yaml
# 輸出目錄（相對或絕對路徑）
output_dir: ./books

# 撰寫語言（ISO 639-1 + 地區碼）
language: zh-TW   # 其他可選：zh-CN, zh-HK, en, ja, ...

# 在地化 profile（讀取 profiles/<locale>.yaml；留空表示不在地化）
localization_profile: zh-TW

# 章節字數範圍
chapter_length:
  min: 2000
  max: 3000

# 合理使用引用限制（可選）
quote_limits:
  enabled: true
  max_quote_chars: 200         # 單次 blockquote 上限
  max_quote_ratio: 0.10        # 全書引用佔比上限

# 是否啟用公版書守門（建議新手打開）
public_domain_only: false

# 敘事視角（narrator / first-person）
narrative_style: narrator
```

若環境無法讀取 `config.yaml`（如純 ChatGPT prompt），則採內建預設值，並在執行前把採用的設定輸出給使用者確認。

**不要將路徑寫死在提示詞或產出檔案中**。任何需要寫入檔案的地方一律使用 `{{output_dir}}` 變數。

---

## 可用指令

- `<topic>` — 從主題建立知識集（例：`投資心理學入門`）
- `--from-books <book1>, <book2>, ...` — 從多本書綜合整理
- `--from-search <query>` — 先搜尋書籍再編撰
- `--outline <topic>` — 僅產生大綱（供審閱）
- `--chapter <N> --of <topic>` — 僅產生特定章節
- `--self-review <path>` — 對既有草稿執行 Phase 4 審稿
- `--public-domain-only` — 僅處理公版書（預設美國 1928 年以前出版；或作者過世 ≥ 70 年）

---

## 著作權與合理使用建議

本 skill 以摘要 / 評論型作品為目的。以下為**建議性原則**，使用者可依自身情境調整：

1. **建議採旁白敘事**（「書中主張…」「作者認為…」「本書指出…」）。此為預設風格。若使用者透過 `narrative_style: first-person` 改用第一人稱模擬敘事，應自行評估著作人格權與誤認風險。
2. **建議不宣稱是原書的授權改編、續作或官方版本**。
3. **引用建議上限**：單次 blockquote ≤ 150 字，全書佔比 ≤ 10%。可由 `quote_limits` 調整或關閉。
4. **建議引用附來源**：書名、作者、出版年、章節或頁碼。
5. **避免從已知盜版來源抓取內容**（如 Library Genesis 的全文頁面等）。
6. **建議於產出書籍開頭插入免責聲明**（使用者可自行移除或改寫）：

   > **Disclaimer**：本書為 AI 輔助編撰之**摘要與評論**，非原書之重製或授權改編。書名與作者名僅為引述目的使用。本書內容不代表被引用作者之立場，且可能含有 AI 生成之解讀與推論。**商業散布前請自行評估當地著作權與合理使用風險。**

7. **公版書模式**：啟用 `--public-domain-only` 時，僅處理可確認為公版之書籍。對新使用者或不確定風險的情境是安全入門路徑。

## Acceptable Use Policy（開源專案層級）

作為工具提供者，本專案明確聲明：

- 本 skill 僅提供提示詞與流程，**不對使用者產出的書籍內容負責**。
- 使用者應自行確保產出符合當地著作權法規。
- **不鼓勵**以本 skill 重製、仿作或冒名散布受著作權保護之作品。
- 若收到 DMCA 或其他正式法律通知，專案維護者會配合處理。

此聲明之完整版請見 `DISCLAIMER.md`。

---

## 編撰工作流程

### Phase 0：載入設定（必要，第一步）

在進入任何後續 Phase 之前，**必須**先執行下列步驟：

1. **嘗試讀取工作目錄下的 `config.yaml`**。
   - 若存在：解析其內容，作為本次執行的設定來源。
2. **若 `config.yaml` 不存在**：
   - 採用 SKILL.md 內文所述之內建預設值。
3. **將最終採用的設定輸出給使用者確認**，格式如下：

   ```
   📋 本次執行採用的設定：
   - 設定來源：config.yaml | 內建預設
   - output_dir: <value>
   - language: <value>
   - localization_profile: <value>
   - chapter_length: <min>-<max>
   - narrative_style: <value>
   - public_domain_only: <value>
   - quote_limits.enabled: <value>
   ```

4. 後續所有 Phase 中的 `{{output_dir}}`、`{{chapter_length.*}}`、`language`、`narrative_style` 等變數，**一律**以 Phase 0 載入的值為準，不得寫死或猜測。

**若環境無 file I/O 能力**（如純 ChatGPT prompt），則直接採用內建預設值，並於執行前明示使用者。

---

### Phase 1：研究與規劃

1. **理解請求**：解析主題、來源書籍或搜尋查詢。
2. **蒐集來源材料**（能力：web 搜尋 / 抓取）：
   - 主題型：搜尋 5-10 本相關書籍。
   - 書單型：抓取 Open Library / Google Books 的 metadata 與描述。
   - URL / 檔案型：讀取並擷取關鍵內容。
3. **記錄每個來源的類型**（用於後續標示）：
   - `📖 [書籍摘要]` — 來自 Open Library / Google Books API 的 metadata + 描述
   - `🌐 [網路資料]` — 來自 web 搜尋 / 抓取的網路資源（書評、訪談、摘要）
   - `🤖 [AI 生成]` — 來自模型知識合成（無外部來源佐證）
4. **辨識關鍵主題**：從來源擷取 10-20 個主要概念。
5. **建立大綱**：組織成邏輯章節結構。
6. **評估來源豐富度**：每個主題評估為 **豐富 / 一般 / 稀疏**。

### Phase 2：大綱生成

產生大綱並呈現給使用者審核。

#### 📛 新書名生成規則

**產出書籍的標題（及主目錄名）一律由 LLM 重新命名，不可沿用使用者輸入的原書名或原主題字串**。理由：

- 降低與既有著作書名混淆、商標爭議、讀者誤認風險。
- 體現本 skill「整理 / 綜述」的定位，而非「仿作原書」。

命名原則：

- 以本次編撰所聚焦的**核心觀點或角度**命名，不得直接複製任何來源書名。
- 若使用者輸入的是主題（如「投資心理學」），生成的書名應為該主題下的具體切角（如「市場情緒與決策：一本給散戶的心智地圖」）。
- 若使用者輸入的是書單（`--from-books`），新書名應反映**綜合視角**，不得以其中任一本原書名為主標（如不可用「窮查理的普通常識進階版」）。
- 語言依 config 的 `language` 設定。
- 必須同時產出：
  - `title`（顯示用，可含中文 / 日文等）
  - `slug`（檔名與目錄名用，ASCII、短横線分隔、全小寫、不超過 60 字元）

使用者若對新書名不滿意，可於確認大綱階段要求 LLM 重新命名。

#### 多部分擴展判斷

同時滿足下列條件才可擴展：

1. 該章來源評估為「豐富」
2. 書籍總章節數 ≤ 5
3. 擴展後全書總部分數不超過 15

以 `🔀 [擴展]` 標示。若無章節需擴展，不提及此機制。

#### 大綱輸出模板

```markdown
# 📖 <新生成的書名>
> <One-line subtitle>
> **Slug（目錄/檔名用）**：<ascii-slug>
> **使用者原始輸入**：<original input>（僅供參考，不作為書名）

**基於來源**: <list of source books/materials>
**來源類型分佈**: 📖 / 🌐 / 🤖 （各章節主要來源）
**預估章節數**: N（含擴展共 M 個部分）
**預估字數**: ~X,000 字
**語言 / 在地化**: <locale>

## 目錄

### 前言
- 本書目的與讀者對象
- 核心問題意識

### 第一章：<Chapter Title>
- <Key point 1>
- <Key point 2>

### 第二章：<Chapter Title> 🔀 [擴展: 2 部分]
- **Part A: <Sub-topic>** — <Key points>
- **Part B: <Sub-topic>** — <Key points>

### 結語
- 核心摘要
- 行動建議
- 延伸閱讀

## 核心術語對照表

| 原文 | 本書統一用語 | 避免使用 |
|------|-------------|---------|
| <Term> | <統一譯法> | <易混淆詞> |
```

#### 術語對照表說明

列出全書 5-15 個關鍵術語的統一用法，存為 `_terminology.md`，供 Phase 3 撰寫與 Phase 4 審稿參照。

#### 來源稀疏閘門

若超過半數章節來源為「稀疏」，必須揭露並提供建議（縮減範圍 / 合併章節 / 標示 AI 生成）。

**等待使用者確認後才進入 Phase 3。**

### Phase 3：逐章撰寫

每章執行：

1. **前置準備**：讀取 `_progress.md` 與大綱；參照 `_terminology.md`。
2. **研究**：從來源蒐集該章資訊。
3. **撰寫**：每章 `{{chapter_length.min}}`-`{{chapter_length.max}}` 字（依 config）：
   - 清楚的章節標題（H3/H4）
   - 以範例解釋關鍵概念
   - 引用來源（附完整 citation）
   - 實用重點或行動建議
   - 銜接下一章
   - 使用 config 指定的語言與在地化 profile 撰寫
   - 敘事視角依 config 的 `narrative_style`（預設 `narrator`，採旁白敘事）
4. **儲存**：寫入 `{{output_dir}}/<slug>/<NN>-chapter-<NN>.md`。
5. **更新進度追蹤**：追加到 `_progress.md`：

   ```
   ## 第N章：<Title>
   - **核心論點**：<1-2 句>
   - **關鍵術語**：<本章使用的重要術語>
   - **銜接方向**：<結尾引導，下一章如何承接>
   ```

6. **多部分章節**：每部分獨立檔案，標題 `## 第N章 Part A: <Sub-topic>`；Part A 開頭加整體概述，各部分結尾加銜接。

### Phase 4：總編輯審稿

所有章節完成後，以總編輯角度審查：

#### 4.1 結構與邏輯
- 章節銜接、論述一致性、篇幅均衡、大綱符合度、術語一致性。

#### 4.2 內容品質
- 深度、範例品質、引用可信度、冗餘刪除。
- **敘事視角一致性**：若採預設旁白敘事，改寫偏離的段落；若使用者選 `first-person`，檢查全書一致性。

#### 4.3 合理使用檢查（可選，依 config）
若 `quote_limits.enabled: true`：
- 計算全書 blockquote 字數總和 / 全書字數比例，超過 `max_quote_ratio` 時提醒使用者。
- 單次 blockquote 超過 `max_quote_chars` 建議縮短或改寫。
- 建議所有 blockquote 下方附 citation。
若未啟用則略過此步驟。

#### 4.4 在地化校準（若啟用 localization profile）
- 依 `profiles/<locale>.yaml` 執行用語、標點、文化語境的在地化。
- 預設提供 `zh-TW`；其他語系歡迎貢獻。

#### 4.5 可讀性
- 段落長度、標題層級、粗體與強調、閱讀節奏。

#### 4.6 執行方式
- 先讀取 `_progress.md` 建立鳥瞰 → 逐章 Read + Edit → 全書通讀。

### Phase 5：組裝與輸出

1. **合併**所有章節成單一 Markdown 文件（若使用者要求單檔輸出）。
2. **加入前言**：書名頁、目錄、序言、建議的免責聲明。
3. **加入後記**：參考書目（完整 citation）、詞彙表、索引。
4. **儲存** 至 `{{output_dir}}/<slug>.md` 或 `{{output_dir}}/<slug>/`。
5. **Slug 規則**：ASCII、短横線分隔、全小寫、不超過 60 字元。顯示用的書名（含中文）保留在 front-matter 的 `title` 欄位。
6. **檢查點**：組裝前再次確認產出書名與目錄名皆為 Phase 2 重新命名的結果，非使用者原始輸入的沿用。

---

## 寫作指南

### 風格
- 使用 config 指定的撰寫語言（預設 `zh-TW`）。
- 清楚易讀，避免未解釋的術語。
- 每章獨立但連貫。
- 實際範例、案例研究或思想實驗。
- 以類比解釋複雜概念。

### 每章結構

```markdown
## 第N章：<Title>
> <來源標示：📖 [書籍摘要] | 🌐 [網路資料] | 🤖 [AI 生成]>

> <Opening quote or key insight>

### <Section 1>
<Content>

### 本章重點
- Point 1
- Point 2

### 思考問題
1. <Question>

### 本章來源
- 書籍：《Book Title》, Author Name, Year, ISBN: xxx
- 網路：[Title](url) — 簡短說明

---
```

### 來源標示規則

| 標示 | 使用時機 |
|------|----------|
| 📖 [書籍摘要] | 內容主要來自 Open Library / Google Books API 的 metadata 與描述（**非原書全文**） |
| 🌐 [網路資料] | 內容主要來自 web 搜尋 / 抓取的網路資源 |
| 🤖 [AI 生成] | 無外部來源佐證，由模型知識合成 |

**混合來源時**，標示所有適用類型，並說明比例：`🌐 [網路資料] + 🤖 [AI 生成]（AI 生成 ≈ 30%）`

**🤖 章節必附免責**：
> ⚠️ *本章內容為 AI 依據模型訓練知識生成，無獨立來源佐證，僅供參考。*

### 敘事視角

**預設採用旁白式摘要 / 評論敘事**（`narrative_style: narrator`）。把自己放在「書評人 / 摘要者」位置，降低誤認風險。

| 旁白敘事（預設） | 第一人稱模擬（使用者可選） |
|---|---|
| 書中把這個現象稱為「注意力赤字」 | 我們可以把這個現象稱為「注意力赤字」 |
| 作者指出，多數投資人忽略了風險 | 多數投資人忽略了風險 — 這正是問題所在 |
| 書中引用的研究顯示，70% 的人會…… | 研究顯示，70% 的人會…… |

**可直接敘事而不帶人名的情境**：
- 純屬通識性陳述、非特定書籍獨有的觀點。
- 多書合成（`--from-books`）時對共通觀點的綜合敘述。

使用者若選用 `first-person` 模式，應於事前了解相關著作人格權風險。

### 引用格式
- 直接引用原文：blockquote + `—— 《Book Title》, Author Name (Year), Ch.N / p.NN`
- 改寫摘要：旁白敘事 + 章末 citation
- 學術名詞首次出現：可加簡短源流說明

---

## 來源材料蒐集

優先序：

1. **Open Library**：metadata、主題、相關作品。
2. **Google Books 描述**：豐富摘要。
3. **Internet Archive**：公版書全文（`https://archive.org/details/<ia_id>`）。
4. **Web 搜尋**：書評、訪談、章節摘要（**注意網路內容本身也有著作權**）。
5. **Wikipedia**：主題與作者的背景。

**避免來源**：
- 已知盜版網站（如 Library Genesis 的全文頁面）。
- 未授權的書籍全文 PDF。

---

## Token 管理

1. **逐章作業**，不試圖保留整本書在 context。
2. **頻繁儲存**中間結果到檔案。
3. **以大綱為錨點**。
4. **多書合成**：先摘要每本書存檔，再從摘要合成。
5. **維護 `_progress.md`**。

### 中斷恢復

若作業中斷，要恢復時：
1. 讀取 `{{output_dir}}/<slug>/_progress.md` 確認已完成的章節。
2. 讀取 `_terminology.md` 載回術語。
3. 讀取大綱檔案。
4. 從 `_progress.md` 最後一章的「銜接方向」欄接續下一章。

---

## 輸出格式

### 格式一：單一檔案
`{{output_dir}}/<slug>.md`

### 格式二：分章（預設）

```
{{output_dir}}/<slug>/
├── 00-front-matter.md    # 含建議的免責聲明
├── 01-chapter-01.md
├── 02-chapter-02a.md     # 🔀 Part A
├── 02-chapter-02b.md     # 🔀 Part B
├── 99-back-matter.md     # 參考書目、citation
├── README.md             # 含連結的目錄
├── _terminology.md
└── _progress.md
```

---

## 範例

### 主題型
```
投資心理學入門
```
（LLM 會重新命名為如 `market-emotion-and-decision` 之類的 slug，並產生新書名）

### 多書合成
```
--from-books Poor Charlie's Almanack, Thinking Fast and Slow, Antifragile
```

### 僅大綱
```
--outline AI 時代的軟體工程
```

### 公版書安全模式（推薦新手）
```
--public-domain-only Henry David Thoreau nature writing
```

### 對既有草稿自審
```
--self-review ./books/my-draft/
```

---

## Contributing

歡迎貢獻：
- 新的 localization profile（`profiles/<locale>.yaml`）
- 範例產出（`examples/`）
- 多 LLM 環境的調整建議
- 品質檢查規則

詳見 `CONTRIBUTING.md`。

## License

- 本 skill 提示詞：MIT
- 範例產出：CC-BY-SA 4.0
- 使用者產出：使用者自行決定授權

見 `LICENSE` 與 `DISCLAIMER.md`。
