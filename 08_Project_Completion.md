# 08 Project Completion

Version: 1.0
Purpose: AI / 軟體專案完成前的正式驗收與交接標準

---

## 1. Completion 的定義

「程式寫完」
不等於
「專案完成」。

「可以執行」
也不等於
「需求完成」。

Completion 必須確認：

需求
+
功能
+
測試
+
安全
+
文件
+
交接

都有足夠證據。

---

## 2. 回到原始目標

完成前重新確認：

- 原始主要目標
- 本階段範圍
- 成功標準
- 明確排除項目

逐項回答：

原始需求是否真的完成？

YES / NO / PARTIAL

如果核心需求：

NO 或 PARTIAL

不得宣告：

PASS。

---

## 3. Scope Completion

列出本階段：

### 已完成
實際完成並驗證的項目。

### 未完成
原本要求但尚未完成的項目。

### Backlog
確認不屬於本階段，
留待未來處理的項目。

禁止把：

「移到 Backlog」

當成：

「已完成」。

---

## 4. Functional Verification

每個核心功能必須有：

需求：
測試方式：
預期結果：
實際結果：
證據：
PASS / FAIL：

不能只寫：

「已測試正常。」

應能回答：

「怎麼知道正常？」

---

## 5. Evaluation

涉及 AI / RAG / Agent：

讀取：

06_Evaluation.md

確認相關：

- Test Cases
- Golden Dataset
- Retrieval
- Generation
- Citation
- Tool Calling
- Agent Behavior
- Regression

已依專案需求完成。

未測試的重要功能：

必須明確標示。

---

## 6. Regression

最後重要修改後，

重新執行核心 Regression Tests。

確認：

新功能正常

且：

原本 PASS 的功能沒有被改壞。

若存在核心 Regression FAIL：

不得宣告 PASS。

---

## 7. Security Completion

確認專案最終版本：

- 資料流向沒有未審查變更
- 權限符合最小權限
- 沒有未核准外部服務
- Secret 沒有寫入程式 / Log / 文件
- 高風險操作需要人工批准
- Prompt Injection 防護符合需求
- Backup / Rollback 符合風險需求

如果開發過程加入：

新 Tool
新 API
新 Model
新資料來源
新權限

必須確認：

相關安全變更已重新審查。

---

## 8. Known Limitations

完成不代表：

「系統沒有任何限制。」

應清楚記錄：

- 已知限制
- 尚未解決的小問題
- 不支援的情況
- 尚未驗證的情境
- 目前使用限制

禁止為了讓報告看起來完整，
隱藏已知問題。

---

## 9. Evidence

重要完成宣告應有證據。

證據可能包括：

- Test Report
- Test Log
- Screenshot
- Output File
- Evaluation Report
- Git Commit
- Version Number
- Source Citation
- Audit Log

證據必須：

可以重新找到。

不要只存在：

某一個聊天視窗。

---

## 10. Version

完成版本應明確標示：

Project：
Version：
Date：
Environment：
重要 Dependency：
Model / API Version（若重要）：

目的：

未來發生問題時，
知道當初驗證的是哪個版本。

---

## 11. Documentation

依專案需要確認：

- README
- Setup
- Configuration
- Architecture
- Data Source
- Permission
- Known Limitations
- Troubleshooting
- Recovery
- Handoff

不是每個小專案都必須有全部文件。

只保留：

未來維護真正需要的內容。

---

## 12. Handoff

Handoff
（交接）

目標：

換一個人或 AI，
不依賴原聊天紀錄，
也能理解專案。

至少包含：

專案目標：
目前版本：
已完成：
未完成：
Backlog：
主要架構：
重要檔案：
資料來源：
已知風險：
測試結果：
重要決策：
下一步：

---

## 13. Clean Handoff Test

重要專案可進行：

Clean Handoff Test
（乾淨交接測試）

方式：

新的 AI / 新對話
↓
不提供舊聊天歷史
↓
只提供正式專案文件
↓
要求它回答：

1. 專案目標是什麼？
2. 現在做到哪裡？
3. 哪些事情尚未完成？
4. 有哪些已知風險？
5. 下一步應該做什麼？
6. 哪些事情不能自行決定？

如果無法正確回答：

代表交接文件不足。

---

## 14. 不要把聊天紀錄當正式文件

ChatGPT / Claude / Codex
聊天內容可以協助工作。

但重要資訊應沉澱到：

- Project Summary
- Decision Log
- Test Report
- Handoff
- README
- 正式專案文件

不要依賴：

「那個 AI 應該記得。」

---

## 15. Final Completion Report

完成時固定輸出：

# Project Completion Report

Project：
Version：
Date：

## Original Goal
原始主要目標：

## Scope

已完成：
未完成：
Backlog：

## Verification

Functional Tests：
RAG / AI Evaluation：
Regression：
Security：
Permissions：

## Evidence

測試證據：
版本證據：
其他證據：

## Known Limitations

已知限制：

## Handoff

交接文件：
下一步：
需要人工決策：

## Final Status

PASS
WARNING
DECISION REQUIRED
STOP