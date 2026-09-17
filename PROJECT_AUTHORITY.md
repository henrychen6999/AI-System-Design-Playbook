# PROJECT AUTHORITY

## 1. 專案目的
這是一個用來測試 AI 團隊協作治理方式的測試專案。

## 2. 角色
- Owner：Henry
- 主控 AI：負責讀取專案狀態、提出下一步、建立工單、驗收結果
- 執行 AI：負責依工單施工
- Auditor：負責獨立檢查，不直接修改

## 3. 核心規則
1. AI 不可自行改變專案重大方向。
2. 執行 AI 只能依正式 Work Order 施工。
3. 施工完成後必須留下 Report。
4. 重要功能不可由施工者自己驗收。
5. 新主控第一次接手時只能讀取與重建狀態，不得直接施工。
6. 重大決策需由 Owner 確認。

## 4. 正式來源
- Authority：本文件
- Roadmap：ROADMAP.md
- Current：CURRENT.md
- Work Order：WORK_ORDER.md
- Report：REPORT.md

## 5. 目前階段
此 Repository 僅作為 GitHub 與 AI 協作流程測試用途。
