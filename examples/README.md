# Examples — 精選樣本

> 這個目錄收錄用 [openbookcast / book-compiler](../SKILL.md) 產出的**公版書精選樣本**，讓你在 clone 整個 repo 之前，就能實際感受 skill 的產出品質。

所有樣本均採 **[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)** 授權。

---

## 為什麼叫「精選樣本」

這些不是**完整的書**。每個樣本都包含：

- ✅ 完整的 front-matter（扉頁 + 免責聲明）
- ✅ 完整的大綱（6 章以上）
- ✅ 完整的術語對照表
- ✅ **2 章完整寫出的章節**（約 2,500 字 / 章，落在 [config.yaml](../config.yaml) 的 `chapter_length` 範圍內）
- ✅ 完整的後記、參考書目、授權聲明
- ❌ 其餘章節僅列出標題與摘要，明確標示 "Sample only"

目的是：**用最小足跡展現最大訊號**。足以判斷 skill 的品質、風格、與你的需求是否契合，但不需要審閱 20,000 字的 AI 生成內容。

---

## 目前的樣本

| 樣本 | 主題 | 來源 | 生成模式 |
|------|------|------|---------|
| [`from-inferiority-to-freedom/`](from-inferiority-to-freedom/) | 阿德勒個體心理學：自卑感、生活風格、社會興趣 | Alfred Adler 三本公版著作（1927、1929、1931） | `--from-books` + `--public-domain-only` |

### 細看某個樣本

從任一樣本目錄的 `README.md` 開始。例如：
- [`from-inferiority-to-freedom/README.md`](from-inferiority-to-freedom/README.md) 說明這份樣本用什麼命令生成、選哪兩章、為什麼是阿德勒。

### 快速品嘗

想在 30 秒內感受一下產出風格，直接點一章讀：
- [第 1 章：自卑不是病](from-inferiority-to-freedom/01-chapter-01.md)
- [第 4 章：社會興趣是歸屬感的總開關](from-inferiority-to-freedom/04-chapter-04.md)

---

## 選用公版書作為樣本的原因

- **風險最低**：所有樣本的原典作者皆已過世 70 年以上，著作在多數法域已進入公有領域。
- **示範 `--public-domain-only` 模式**：這是 skill 推薦給新使用者的安全入門路徑。
- **訊號乾淨**：讀者可以在不擔心侵權的前提下，專心評估產出品質。

---

## 想貢獻更多樣本？

歡迎 PR！建議的新樣本主題方向：

- **其他公版心理學家**：William James《心理學原理》、Freud《夢的解析》、Le Bon《烏合之眾》
- **東方古典**：《莊子》《論語》《孫子兵法》
- **公版哲學**：斯多噶學派（Marcus Aurelius、Epictetus、Seneca）
- **公版文學評論**：圍繞特定公版名著的主題綜述

貢獻指南見 repo 根目錄 [README.md](../README.md#contributing)。

---

## 授權

本目錄下所有 Markdown 檔案採 **CC-BY-SA 4.0** 授權。
skill 提示詞本身採 MIT（見 [LICENSE](../LICENSE)）。
使用者自行產出的書籍由使用者自行決定授權。

完整法律與合理使用聲明見 repo 根目錄 [`DISCLAIMER.md`](../DISCLAIMER.md)。
