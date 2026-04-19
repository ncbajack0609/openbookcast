# 範例：《從自卑到自由：阿德勒心理學給現代人的勇氣地圖》

> 這是用 [openbookcast / book-compiler](../../SKILL.md) 產出的 **精選樣本**（showcase sample）。
> 授權：**CC-BY-SA 4.0**。

---

## 這份樣本展示了什麼

這是一本用 `book-compiler` skill 從三本 Alfred Adler（1870–1937）的公版著作綜合編撰出來的知識書。它示範了四件事：

1. **多書合成**（`--from-books`）：綜合三本原典而非單一來源。
2. **公版書安全模式**（`--public-domain-only`）：取材完全落在公有領域。
3. **Phase 2 重新命名**：書名不沿用任何原書名或既有熱門譯本書名。
4. **來源透明標示**：每章開頭與結尾皆註明 📖 / 🌐 / 🤖 的來源類型。

---

## 生成命令（示意）

```bash
/book-compiler --from-books "Understanding Human Nature, The Science of Living, What Life Could Mean to You" --public-domain-only
```

Phase 0 採用的設定（基於根目錄 `config.yaml`，但 `public_domain_only` 被本次執行覆寫為 `true`）：

```yaml
output_dir: ./examples
language: zh-TW
localization_profile: zh-TW
chapter_length: 2000-3000
narrative_style: narrator
public_domain_only: true      # 本次覆寫
quote_limits:
  enabled: true
  max_quote_chars: 200
  max_quote_ratio: 0.10
```

---

## 為什麼選阿德勒

- **公版狀態清晰**：阿德勒 1937 年過世，三本主要英譯本皆於 1927–1931 出版；在台灣（過世後 50 年）與歐盟（70 年）皆已完全進入公有領域。
- **市場熱度高**：岸見一郎《被討厭的勇氣》帶動的閱讀浪潮仍在，台灣讀者對阿德勒的名字不陌生，但對原典內容相對模糊——正是摘要與評論型作品有價值的位置。
- **多書可合成**：三本書的概念互相補充，非常適合示範 `--from-books`。
- **風險極低**：不碰任何在世學者、不依賴任何受著作權保護的二手解讀書籍。

---

## 為什麼只寫兩章

完整版預計 6 章 + 前後言，約 18,000–22,000 字。作為 repo 的 showcase，做完整版的成本（生成 + 審稿）過高，也會稀釋樣本的訊號。

本範例選擇**豐富度最高**的第 1 章（自卑感與補償）與第 4 章（社會興趣與三大人生任務）作為深度樣本，其餘章節在 [`_outline.md`](_outline.md) 中列出完整大綱，明確標示 "Sample only – full chapters not generated"。

這是想試讀的人判斷「整本書會是什麼品質」的最小足跡。

---

## 檔案結構

| 檔案 | 內容 |
|------|------|
| [`00-front-matter.md`](00-front-matter.md) | 扉頁、免責聲明、讀者輪廓、核心地圖一覽 |
| [`_outline.md`](_outline.md) | 完整 6 章大綱、章節豐富度評估 |
| [`_terminology.md`](_terminology.md) | 阿德勒核心術語（德/英/中）對照表 |
| [`01-chapter-01.md`](01-chapter-01.md) | **完整樣本**：自卑不是病 |
| [`04-chapter-04.md`](04-chapter-04.md) | **完整樣本**：社會興趣是總開關 |
| [`99-back-matter.md`](99-back-matter.md) | 參考書目、詞彙表、授權聲明 |

---

## 如何用這份樣本判斷要不要 clone

讀這兩章，問自己：

- 這樣的**深度**與**在地化語感**，符合你心目中「值得留下來的讀書筆記 / 電子書」嗎？
- 你想讀的主題，來源類型分佈會是怎樣？（比對這裡的 📖 / 🌐 / 🤖 比例）
- 你的 LLM 環境是否支援 Web 搜尋與檔案 I/O？（Phase 1 的來源蒐集需要）

如果這些都對得上，clone [openbookcast](../../README.md)，把 `SKILL.md` 放到你的 LLM 工具裡，換一個你真正想寫的主題跑一遍。

---

## 授權

- **本範例產出**：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- **原典**：Alfred Adler 三本著作皆為公有領域（public domain）
- **book-compiler skill 提示詞**：MIT（見 [LICENSE](../../LICENSE)）

詳細授權與免責聲明見 [`99-back-matter.md`](99-back-matter.md) 與 repo 根目錄 [`DISCLAIMER.md`](../../DISCLAIMER.md)。
