Last Updated: 2026-04-08 20:05:55 Asia/Taipei

# A｜實際版（Run / System）

Force Cheng × FALO
Version: v1.01
Date: 2026-04-08

## 定位

A 版是「真正運作的系統」。

它不是展示稿，也不是教材稿，而是目前 V1 的實際運作組成。

## 核心組成

- GAS backend
- Google Sheet / Drive
- 外部 HTML frontend
- 真實資料與操作流程

## 這一版的目的

- 讓系統能跑
- 讓流程能用
- 讓功能能驗證
- 讓後續可以抽成 B / C / D

## 對應實體

### GAS

- [Code.gs](/Users/force/AI-CodeX/ai-smart-km-20260408/gas/Code.gs)
- [setup.gs](/Users/force/AI-CodeX/ai-smart-km-20260408/gas/setup.gs)
- [appsscript.json](/Users/force/AI-CodeX/ai-smart-km-20260408/gas/appsscript.json)

### Frontend

- [index.html](/Users/force/AI-CodeX/ai-smart-km-20260408/frontend/index.html)

### Schema Blueprint

- [FALO_Source_Inbox_V1.xlsx](/Users/force/AI-CodeX/ai-smart-km-20260408/drafts/excel/FALO_Source_Inbox_V1.xlsx)

### System Docs

- [README.md](/Users/force/AI-CodeX/ai-smart-km-20260408/README.md)
- [system-behavior.md](/Users/force/AI-CodeX/ai-smart-km-20260408/docs/system-behavior.md)
- [data-flow.md](/Users/force/AI-CodeX/ai-smart-km-20260408/docs/data-flow.md)
- [sheet-schema.md](/Users/force/AI-CodeX/ai-smart-km-20260408/docs/sheet-schema.md)
- [design-decisions.md](/Users/force/AI-CodeX/ai-smart-km-20260408/docs/design-decisions.md)

## V1 已驗證能力

- login
- create source
- duplicate hint
- attach Drive file
- activity log
- send to self
- open source from email link
