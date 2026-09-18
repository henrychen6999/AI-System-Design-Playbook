# 06 Evaluation

Version: 1.0
Purpose: AI、RAG、Agent 與 AI 系統的品質驗證標準

---

## 1. 核心原則

Evaluation（評估）
不是確認：

「AI 有回答。」

而是確認：

「AI 是否完成原本要求，
而且有證據證明。」

任何重要 AI 功能，
都應先定義成功標準，
再決定測試方法。

---

## 2. 先確認要測什麼

不同系統不能使用同一種測試。

例如：

RAG
→ 測搜尋與回答

Agent
→ 測 Tool 選擇與執行

API
→ 測資料交換與錯誤處理

Dashboard
→ 測資料正確與來源一致

開始測試前先回答：

- 核心功能是什麼？
- 哪些錯誤不能接受？
- 怎樣算 PASS？
- 哪些項目需要人工判斷？

---

## 3. 建立 Test Cases

Test Case（測試案例）

至少包含：

輸入：
預期結果：
實際結果：
證據：
PASS / FAIL：

不要只測正常問題。

應依風險加入：

- 正常案例
- 邊界案例
- 錯誤案例
- 缺少資料
- 無權限資料
- 工具失敗
- 不存在答案

---

## 4. Golden Dataset

Golden Dataset
（標準答案測試集）

是一組事先確認過：

問題
+
正確答案 / 正確來源 / 預期行為

的測試資料。

用途：

每次修改系統後，
重新跑相同測試。

確認：

原本會的東西沒有被改壞。

Golden Dataset 不需要一開始很大。

先從最重要、
最容易出錯的案例建立。

---

## 5. RAG Evaluation

RAG 至少分兩層測試。

### A. Retrieval

先測：

「資料有沒有找對？」

例如：

問題：
RAG 與 MCP 有什麼差別？

預期來源：
指定 Knowledge 文件

實際搜尋：
是否找到正確文件 / Chunk？

### B. Generation

再測：

「找到正確資料後，
AI 有沒有回答對？」

確認：

- 是否忠於來源
- 是否漏掉重要內容
- 是否加入來源不存在的內容
- Citation 是否正確

不要把 Retrieval 與回答品質混在一起。

---

## 6. Retrieval 指標

需要量化時，可考慮：

Recall@K
= 正確資料有沒有出現在前 K 筆搜尋結果。

例如：

前 5 筆結果中，
有沒有找到應該找到的文件？

必要時再考慮：

Precision
MRR
NDCG

不要因為存在很多指標，
就全部使用。

只使用能回答實際問題的指標。

---

## 7. Hallucination / Grounding

測試：

- AI 的重要敘述是否有來源？
- 來源真的支持這個答案嗎？
- 找不到資料時是否承認不知道？
- 是否把推論說成事實？
- 是否捏造文件、數字或 Citation？

重要資料應區分：

Source Fact
AI Inference
Human Confirmed

---

## 8. Agent Evaluation

Agent 不只測最後答案。

還要測：

- 是否選對 Tool
- Tool 參數是否正確
- 是否取得真實結果
- 是否越權
- Tool 失敗是否正確處理
- 是否重複執行
- 是否正常停止
- 高風險操作是否要求批准

例如：

「查詢 ERP 訂單」

不能只確認：

Agent 回答了一個數字。

還要確認：

那個數字真的來自 ERP。

---

## 9. Security Evaluation

依風險測試：

- 未授權使用者能否看到資料？
- Prompt Injection 是否能改變安全規則？
- Secret 是否可能出現在輸出或 Log？
- Agent 是否能執行未授權 Tool？
- 公司資料是否被送往未核准服務？
- 高風險操作是否會要求人工批准？

安全測試失敗：

不得因主要功能正常就判定 PASS。

---

## 10. Failure Test

主動測試失敗情況。

例如：

API 掛掉
↓
Agent 怎麼處理？

RAG 找不到資料
↓
AI 是否亂回答？

文件格式錯誤
↓
Parsing 是否發現？

權限不足
↓
系統是否停止？

目的：

確認系統不只在
「一切正常」
時才能工作。

---

## 11. Regression Test

Regression Test
（回歸測試）

每次重要修改後，
重新執行原本通過的核心 Test Cases。

目的：

防止：

修好 A
↓
不小心弄壞 B。

尤其適用：

- Prompt 修改
- Chunking 修改
- Embedding 更換
- Retrieval 修改
- Tool 修改
- API 修改
- Agent Workflow 修改

---

## 12. AI-as-a-Judge

可以讓另一個 AI
協助評估大量結果。

但：

AI Judge
不能自動視為真實答案。

重要、高風險或爭議案例，
仍應：

使用已確認標準答案
或
人工抽查。

AI Judge 適合：

輔助評估

不是：

唯一裁判。

---

## 13. 人工抽查

Human Review
（人工審查）

適合：

- 策略分析
- 複雜推論
- 高風險結果
- AI Judge 無法可靠判斷
- 正式上線前抽查

人工抽查結果也可以逐步加入：

Golden Dataset。

---

## 14. 成本與速度

品質不是唯一指標。

正式系統必要時確認：

- Latency
- Token Usage
- API Cost
- Tool Calls
- Failure Rate

但不能為了降低成本，
讓核心準確度低於可接受標準。

先定義：

最低品質要求

再優化：

成本與速度。

---

## 15. 修改前後比較

重大改動前保存 Baseline。

Baseline
= 修改前的基準結果。

修改後比較：

Before
vs.
After

例如：

原本 Retrieval PASS：82%
修改後：91%

但同時：

Latency 增加 300%

這時不能只說：

「新版比較好。」

需要呈現實際取捨。

---

## 16. 不可只看平均值

平均結果可能隱藏重大錯誤。

例如：

100 題有 95 題正確。

看起來：

95%

但剩下 5 題如果全部是：

權限洩漏

仍然不能 PASS。

因此必須區分：

一般錯誤
與
Critical Failure（重大失敗）。

---

## 17. Evaluation Report

正式評估至少輸出：

測試目標：
版本：
測試日期：
Test Cases 數量：
PASS：
FAIL：
Critical Failure：

RAG Retrieval：
Generation：
Citation：
Agent / Tools：
Security：
Regression：
Latency / Cost：

已知限制：
未測試項目：
需要人工確認：

證據位置：

Final Status:
PASS / WARNING / DECISION REQUIRED / STOP