# 07 Project Review

Version: 1.0
Purpose: 專案進行中的方向、範圍、架構、安全與品質審查

---

## 1. Review 的目的

Project Review 不是重新設計專案。

目的只有：

確認目前實作
是否仍然符合原始目標。

Review 不應因為發現新技術，
就主動擴大專案。

---

## 2. 什麼時候需要 Review

不要求每個小修改都 Review。

建議在以下情況觸發：

- 完成一個重要階段
- 準備加入大型新功能
- 準備修改核心架構
- 問題反覆修不好
- 修改次數明顯增加
- 專案範圍開始擴大
- 出現新的安全 / 權限問題
- 準備進入下一階段
- 人工主動要求 Review

目的：

在真正可能走偏時檢查，

而不是每一步都消耗 Context 與 Token。

---

## 3. 回到原始目標

Review 第一件事：

重新讀取：

- 原始主要目標
- 本階段範圍
- 明確排除項目
- 成功標準

然後回答：

目前實作仍然在解決
原始問題嗎？

YES
→ 繼續

NO / 不確定
→ DECISION REQUIRED

---

## 4. Scope Drift

Scope Drift
（範圍漂移）

檢查目前是否新增：

- 原本沒有要求的功能
- 不影響核心目標的功能
- 因 AI 建議而加入的功能
- 為未來可能需求提前建立的功能

每個新增項目問：

它是否真的需要現在做？

如果不是：

移回 Future / Backlog。

不要繼續擴大目前版本。

---

## 5. Architecture Drift

Architecture Drift
（架構漂移）

檢查：

- 是否加入越來越多框架？
- 是否增加不必要的服務？
- 是否重複建立相同功能？
- 是否存在大量臨時修補？
- 是否為了解決小問題修改整體架構？
- 是否比原始方案明顯複雜？

如果答案是 YES：

先找原因。

不要立即再增加一層架構。

---

## 6. Patch Accumulation

Patch Accumulation
（修補累積）

如果同一區域反覆：

修正
↓
再修正
↓
增加例外
↓
再增加 workaround

應停止繼續疊修補。

先確認：

- 原始設計是否錯誤？
- 問題是否出在資料？
- 是否有重複或廢棄程式？
- 是否應進行小範圍重構？

優先：

移除不必要複雜度

而不是：

再加一個 Patch。

---

## 7. 新技術檢查

若準備加入：

- Hybrid Search
- Reranking
- Agent
- Multi-Agent
- MCP
- 新 Database
- 新 Framework
- 新 Model
- 新 API
- 新服務

先回答：

1. 現在具體問題是什麼？
2. 有什麼證據？
3. 現有方案為什麼無法解決？
4. 新技術解決哪個問題？
5. 增加什麼成本與風險？
6. 如何驗證加入後真的改善？

回答不完整：

→ 不應直接導入。

---

## 8. Security Drift

檢查專案進行後是否新增：

- 新資料來源
- 新使用者
- 新 Tool
- 新 API
- 新外部服務
- 新雲端模型
- 新寫入權限
- 新跨境資料流

若有：

重新讀取：

05_Security_Governance.md

原本的安全審查
不能自動涵蓋新的資料流或權限。

---

## 9. Quality Drift

確認：

為了讓功能更多或速度更快，

是否造成：

- 準確度下降
- Citation 消失
- 測試被跳過
- Error 被忽略
- 權限控制降低
- AI 推論被當成事實
- 原本 PASS 的功能被改壞

需要時執行：

06_Evaluation.md

---

## 10. Regression

重要修改後：

重新執行相關 Regression Tests。

確認：

修好新的問題

沒有造成：

舊功能失效。

若核心測試從：

PASS → FAIL

不得直接宣布新版更好。

---

## 11. Backlog

Review 發現：

「這功能很好，
但現在不需要。」

不要刪掉想法。

放入：

Backlog / Future

並記錄：

- 想法
- 原因
- 未來觸發條件

這樣既不忘記，

也不污染目前專案。

---

## 12. Decision Log

重大方向改變應記錄：

日期：
原始方案：
新方案：
改變原因：
證據：
影響：
批准者：

目的：

避免未來 AI 接手時不知道：

「為什麼當初這樣設計？」

---

## 13. Review 不重新讀完整專案

為節省 Context：

優先讀：

- Project Summary
- 原始目標
- 最新狀態
- 最近重大變更
- 相關測試結果
- 必要 Playbook 模組

只有證據不足時，
才讀更多原始內容。

禁止為了 Review
預設重新讀完整專案歷史。

---

## 14. Project Review Report

Review 完成後固定輸出：

### Original Goal
原始主要目標：

### Current State
目前完成：
目前進行：
目前未完成：

### Review

Goal Alignment：
PASS / WARNING

Scope：
PASS / WARNING

Architecture：
PASS / WARNING

Security：
PASS / WARNING

Quality：
PASS / WARNING

Regression：
PASS / FAIL / NOT TESTED

### Findings

發現的問題：

證據：

是否影響原始目標：

### Recommendation

繼續：
修正：
移回 Backlog：
需要人工決策：

### Final Status

PASS
WARNING
DECISION REQUIRED
STOP