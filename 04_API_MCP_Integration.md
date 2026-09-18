# 04 API, MCP & Integration

Version: 1.0
Purpose: AI 與外部系統、工具及資料來源的整合設計標準

---

## 1. 先確認為什麼需要串接

開始整合以前先回答：

- AI 要取得什麼資料？
- AI 要執行什麼操作？
- 只需要讀取，還是需要寫入？
- 資料需要即時嗎？
- 系統是否已提供正式介面？
- 是否涉及公司敏感資料？

如果直接讀取核准檔案就能完成，
不要為了技術完整而建立 API。

---

## 2. 優先使用正式介面

整合方式優先考慮：

1. 系統官方 API
2. 已核准的 MCP / Connector
3. Database / File Interface
4. 其他經核准方式

避免：

- 模擬人工操作，卻有正式 API 可用
- 直接碰正式資料庫底層結構
- 使用未授權第三方服務
- 為了方便繞過既有權限

---

## 3. REST API

REST API 適合：

「送出一個要求 → 收到一個結果」

例如：

Agent
↓
查詢客戶資料
↓
ERP API
↓
回傳結果

常見用途：

- 查詢資料
- 新增資料
- 修改資料
- 呼叫外部服務

確認：

- Authentication
- Permission
- Timeout
- Error Handling
- Rate Limit
- Data Validation

不需要即時持續連線時，
REST 通常是優先考慮方式之一。

---

## 4. Webhook

Webhook 適合：

「有事情發生時，主動通知另一個系統。」

例如：

新文件進入指定位置
↓
Webhook
↓
通知處理系統
↓
開始工作

適合：

- 新資料到達
- 訂單狀態改變
- 工作完成通知
- 外部事件觸發

確認：

- 來源是否可信
- 是否驗證簽章
- 重複事件怎麼處理
- 失敗是否重送
- 是否可能被偽造

---

## 5. SSE / WebSocket

### SSE

Server-Sent Events

適合：

伺服器持續把更新送給使用者。

例如：

AI 回答逐步顯示。

### WebSocket

適合：

雙方需要持續、即時互相傳資料。

例如：

即時互動系統。

只有真的需要即時通訊時才導入。

不要因為技術較新就使用。

---

## 6. GraphQL / gRPC / SOAP / Long Polling

這些方式不是禁止使用。

只有當：

- 現有系統本身要求
- 有明確技術需求
- 比較後確實更適合

才採用。

Playbook 不要求專案支援所有 API Style。

---

## 7. MCP

MCP
Model Context Protocol
（模型上下文協定）

用途：

讓 AI 用較標準化的方式發現與使用工具或資源。

可以把它理解成：

AI
↓
MCP
↓
Tools / Resources
↓
外部系統

MCP 不等於 API 的替代品。

例如：

AI
↓
MCP Server
↓
API
↓
ERP

兩者可以同時存在。

---

## 8. API 與 MCP 怎麼選

先問：

「是系統跟系統溝通，
還是要讓 AI 使用工具？」

一般系統整合：

→ 優先評估 API

AI 需要發現、理解、呼叫工具：

→ 評估 MCP

已經有穩定 API：

→ 不要只為了 MCP 重寫 API

需要讓多種 AI 共用同一套工具：

→ MCP 可能具有價值

必須依實際需求判斷。

---

## 9. Authentication & Authorization

Authentication
= 確認「你是誰」。

Authorization
= 確認「你可以做什麼」。

兩者不可混為一談。

例如：

Agent 已成功登入 ERP

不代表：

Agent 可以修改所有 ERP 資料。

每個整合都應確認：

- 身分
- 權限
- 可讀範圍
- 可寫範圍
- 管理權限

遵守最小權限原則。

---

## 10. Secret 管理

整合過程中會接觸到 API Key、Password、Access Token、Secret 等敏感憑證，串接時至少要記得：

- 不得直接寫入 Prompt、Source Code、Repository、Log 或 Markdown 文件
- 應使用 Environment Variable、Secret Manager 或系統核准的安全儲存方式
- 若已意外曝光，不能只刪文字，必須立即撤銷並更換

完整規則見 05_Security_Governance.md。

---

## 11. 資料流向

建立整合前必須知道：

資料從哪裡來
↓
經過哪個系統
↓
送到哪裡
↓
是否離開公司環境
↓
是否被保存
↓
誰可以存取

涉及公司資料：

→ 讀取 05_Security_Governance.md

不得因為 API 可以呼叫，
就代表資料可以傳送。

---

## 12. Input / Output Validation

外部系統回傳資料不能直接假設正確。

確認：

- 欄位是否存在
- 型別是否正確
- 日期格式
- 數值範圍
- 空值
- 異常內容
- 非預期指令或文字

Agent 產生的資料送入正式系統前，
也必須驗證。

---

## 13. Error Handling

整合失敗時必須區分：

- Authentication Error
- Permission Error
- Timeout
- Rate Limit
- Invalid Data
- Service Unavailable
- Unknown Error

禁止：

API 失敗
↓
AI 自己產生一筆看起來合理的資料
↓
當成 API 真實結果

應明確回報：

成功 / 部分成功 / 失敗 / 未執行

---

## 14. Retry

只有適合重試的錯誤才 Retry。

例如：

短暫 Timeout
→ 可以有限次重試

Permission Denied
→ 重試通常沒有意義

涉及：

付款
新增訂單
寄信
修改正式資料

重試前必須考慮：

是否會重複執行。

---

## 15. Logging & Audit

重要整合應能追蹤：

- 什麼時間
- 哪個系統
- 執行什麼操作
- 成功或失敗
- 必要的錯誤資訊

但 Log 不應保存：

- Password
- API Key
- Access Token
- 不必要的敏感資料

---

## 16. Integration Design Summary

整合前輸出：

整合目的：
來源系統：
目標系統：
讀取 / 寫入：
資料類型：
即時需求：

建議方式：
REST / Webhook / SSE / WebSocket /
MCP / Database / File / Other

Authentication：
Authorization：
Secret 管理：
資料流向：
錯誤處理：
Retry：
Logging：
人工批准：
主要安全風險：

每項標示：

必要 / 建議 / 暫不需要 / 待確認

Status:
PASS / WARNING / DECISION REQUIRED / STOP