# PROJECT AUTHORITY

Version: 1.0  
Last Updated: 2026-09-18

## 1. Purpose

本文件定義 AI 協作專案中的角色、權限、責任與決策邊界。

本文件回答的是：

- 誰負責決策？
- 誰負責規劃？
- 誰負責執行？
- 誰負責驗收？
- 什麼情況 AI 必須停止並交由人類決定？

技術設計、開發、Review 與驗收標準，依：

`PLAYBOOK_INDEX.md`

及其對應模組執行。

---

## 2. Authority Hierarchy

專案最高決策權：

**Owner → Henry**

AI 不得自行取代 Owner 做重大決策。

AI 可以：

- 分析
- 提出方案
- 提出風險
- 提出下一步
- 執行已授權工作
- 驗證結果

但重大方向與高風險決策必須由 Owner 確認。

---

## 3. AI Roles

### 3.1 主控 AI（Controller）

負責：

- 理解目前專案目標
- 讀取正式專案狀態
- 判斷目前階段
- 依 Playbook 進行必要檢查
- 拆解工作
- 建立執行任務
- 協調其他 AI
- 整理執行結果
- 提出下一步

主控 AI 不應在接手新專案時直接施工。

第一次接手應先：

1. 讀取正式文件
2. 重建目前專案狀態
3. 找出已完成、未完成、風險與待決策事項
4. 提出下一步
5. 等待需要的 Owner 決策

---

### 3.2 執行 AI（Executor）

例如：

- Codex
- Claude Code
- 其他程式或自動化 Agent

負責：

- 依已確認的任務執行
- 修改程式或文件
- 執行測試
- 除錯
- 驗證
- 回報結果

執行 AI 不得自行改變專案重大方向。

如果實作過程發現原方案不可行或風險明顯增加，應停止擴大修改並回報主控 AI / Owner。

---

### 3.3 審查 AI（Auditor）

負責：

- 獨立檢查執行結果
- 檢查是否符合需求
- 檢查是否偏離原目標
- 檢查風險
- 檢查測試結果
- 檢查是否符合 Playbook

重要功能原則上不由原施工 AI 單獨完成最終驗收。

Auditor 原則上只審查，不直接修改正式成果。

---

## 4. Owner Decision Required

遇到以下情況，AI 必須交由 Owner 決定：

- 專案目標不清楚
- 有多個差異明顯的方案需要選擇
- 重大架構改變
- 公司敏感資料
- 權限變更
- 資安或隱私風險
- 資料可能外流
- 不可逆操作
- 正式環境重大修改
- 高風險外部操作
- AI 無法安全判斷的重大事項

Owner 未確認前，不得把「待決策」當成「已核准」。

---

## 5. Autonomous Work

以下一般工作，在目標與授權範圍清楚時，AI 可以自主完成：

- 一般程式修改
- File Edit
- Refactoring
- Test
- Debugging
- Validation
- 文件更新
- 安全的本機設定修改

不需要每一個小步驟都要求 Owner 確認。

但如果工作途中觸發第 4 節條件，必須停止並升級為 Owner Decision Required。

---

## 6. Source of Truth

正式專案狀態應以 Repository 中的正式文件為準。

聊天紀錄不能取代正式專案文件。

AI 不應只依賴過去對話記憶判斷目前專案狀態。

AI System Design Playbook 的正式入口：

`PLAYBOOK_INDEX.md`

---

## 7. Handoff

AI 交接時至少應說明：

- 目前目標
- 已完成
- 未完成
- 已知風險
- 待決策事項
- 目前專案狀態
- 建議下一步

新主控 AI 應先完成接手與狀態重建，再開始新的施工工作。

---

## 8. Core Principle

AI 是執行與分析系統，不是專案最終決策者。

**Owner 決定方向。  
Controller 管理工作。  
Executor 執行工作。  
Auditor 獨立檢查。  
Playbook 定義檢查標準。**
