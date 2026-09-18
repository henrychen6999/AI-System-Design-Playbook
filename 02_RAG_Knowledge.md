# 02 RAG & Knowledge

Version: 1.0
Purpose: RAG / 知識庫專案的設計與檢查標準

---

## 1. 先確認是否真的需要 RAG

先確認需求：

- 是否需要根據指定文件回答？
- 是否需要搜尋大量知識？
- 是否需要引用來源？
- 知識內容是否會持續更新？

如果一般 Prompt / Context 已足夠，
不要因為可以使用 RAG 就建立 RAG。

判斷：

必要 / 建議 / 暫不需要 / 待確認

---

## 2. 資料來源

確認：

- 文件來自哪裡？
- PDF / Word / Excel / Markdown / Web / Database？
- 是否存在掃描文件、圖片或表格？
- 資料是否完整？
- 是否存在重複版本？
- 哪個版本才是正式來源？
- 誰負責更新？

禁止：

- 未經授權讀取資料
- 把推論當成原始資料
- 無法確認來源時自行補內容

---

## 3. Parsing

Parsing（解析）
= 把原始文件轉成 AI 可以處理的內容。

確認：

- 標題是否正確保留？
- 段落是否被破壞？
- 表格是否正確解析？
- 頁碼 / 來源資訊是否保留？
- OCR 是否真的必要？

解析品質不好時：

先修 Parsing。

不要直接靠更強模型補救。

---

## 4. Chunking

Chunking（切塊）
= 把長文件切成適合搜尋的小段。

確認：

- Chunk 是否太大？
- 是否太小？
- 是否切斷完整語意？
- 標題與正文是否分離？
- 是否需要保留前後文？

搜尋錯誤時，
先檢查 Chunking 是否為原因。

不要直接增加新技術。

---

## 5. Metadata

Metadata（中繼資料）
= 描述資料的附加資訊。

依專案需求考慮：

- source_type
- source_title
- source_url
- author
- created_date
- topic
- content_type
- status
- version
- permission

Metadata 必須有實際用途。

例如：

- 搜尋過濾
- 權限控制
- 版本管理
- Citation
- 資料追溯

禁止為了「欄位看起來完整」
加入沒有用途的 Metadata。

---

## 6. Embedding

Embedding（向量嵌入）
= 把文字意思轉成可比較的數字表示。

確認：

- 使用哪個 Embedding Model？
- 語言是否適合？
- 中文搜尋效果是否足夠？
- 模型更換後是否需要重新建立索引？

不要只因為有更新的 Embedding Model
就直接更換。

必須先證明現有搜尋品質不足。

---

## 7. Retrieval

Retrieval（檢索）
= 從知識庫找出可能相關的內容。

基本方式：

Vector Search
→ 根據語意相似度搜尋。

如果品質不足，再評估：

Keyword Search / BM25
Hybrid Search
Metadata Filtering

不要預設全部啟用。

---

## 8. Hybrid Search

Hybrid Search（混合搜尋）
= 語意搜尋 + 關鍵字搜尋。

適合：

- 型號
- 人名
- 專案代號
- 特定術語
- 精確關鍵字

只有 Vector Search 實際出現不足時，
才評估導入。

---

## 9. Reranking

Reranking（重新排序）
= 搜尋後，再重新判斷哪些結果最相關。

使用前確認：

- 現有 Retrieval 是否真的排序不佳？
- 是否有測試證據？
- 增加的延遲與成本是否值得？

不要把 Reranking 當成預設功能。

---

## 10. Context Assembly

Context Assembly（上下文組裝）
= 把找到的資料整理後交給 LLM。

確認：

- 是否只放真正相關內容？
- 是否放入太多重複 Chunk？
- 是否超出必要 Context？
- 是否保留來源資訊？
- 是否混入不可信內容？

原則：

不是「找到越多越好」。

而是：

「足夠且相關」。

---

## 11. Grounding

Grounding（有依據回答）

要求 AI：

- 優先根據找到的來源回答
- 清楚區分原始資料與 AI 推論
- 找不到證據時明確表示不足
- 不得自行補造公司資料

若推論是允許的：

必須清楚標示為推論。

---

## 12. Citation

Citation（來源引用）

重要回答應盡量能追溯：

回答
↓
Chunk
↓
原始文件
↓
來源位置

至少確認：

- 文件名稱
- 可定位的來源資訊

若業務需要，再增加：

- 頁碼
- URL
- 日期
- 版本
- 段落

---

## 13. 更新與刪除

知識庫必須考慮：

新增資料
更新資料
舊版本取代
重複資料
錯誤資料
刪除資料
重新建立索引

刪除原始文件時，
確認相關索引 / Chunk 是否同步移除。

避免 AI 搜尋到已失效內容。

---

## 14. 權限

若不同使用者具有不同資料權限：

Retrieval 階段就必須考慮權限。

原則：

使用者沒有權限的資料，
不應先搜尋出來再期待 LLM 不顯示。

涉及公司資料：

→ 讀取 05_Security_Governance.md

---

## 15. RAG 品質驗證

不能只測：

「AI 有回答。」

必須測：

- 找到的文件對不對？
- 找到的 Chunk 對不對？
- 回答是否符合來源？
- Citation 是否正確？
- 找不到答案時是否會亂回答？
- 更新 / 刪除後是否仍搜尋到舊資料？

需要正式測試：

→ 讀取 06_Evaluation.md

---

## 16. 問題診斷順序

RAG 回答錯誤時：

先判斷問題在哪一層。

資料錯？
↓
Parsing 錯？
↓
Chunking 錯？
↓
Metadata 錯？
↓
Retrieval 錯？
↓
Ranking 錯？
↓
Context 錯？
↓
LLM 回答錯？

先修真正原因。

禁止看到結果不好，
就直接增加：

Hybrid Search
Reranking
更大的模型
更多 Agent

---

## 17. 完成前輸出

### RAG Design Summary

資料來源：
Parsing：
Chunking：
Metadata：
Embedding：
Retrieval：
Hybrid Search：
Reranking：
Context：
Grounding：
Citation：
權限：
更新 / 刪除：
Evaluation：

每項標示：

必要 / 建議 / 暫不需要 / 待確認

並列出：

目前已知風險：
目前缺少的證據：
需要人工決策事項：

Status:
PASS / WARNING / DECISION REQUIRED / STOP