Last Updated: 2026-04-08 20:05:55 Asia/Taipei

# FALO Source Inbox V1 教材底稿

## 系統說明

FALO Source Inbox V1 的核心不是 AI，也不是完整 KM。它先解一個更基礎的問題：

把來源收進來，保留原始參照，手動管狀態，必要時再送回自己。

這一版是：

- Source Inbox
- 狀態管理系統
- Google-first 的 V1 平台
- 可教、可理解、可重建的系統底座

這一版不是：

- 不是完整 KM
- 不是 AI workflow engine
- 不是 relation / container 系統
- 不是 Telegram bot

核心原則：

- `Google-first`
- `HTML-first`
- `Force GAS 開發法`
- `status` 一律人工控制
- 文件優先描述系統行為與資料流，而不是只描述程式細節

## 架構

### 三層理解

#### Capture

入口層。使用者從外部前端建立 source，之後再補 note、clean text、reference。

#### Core

資料與索引層。Google Sheet 承接：

- `source`
- `attachment`
- `activity_log`
- `user_account`
- `app_config`

#### Execution

行動輸出層。Gmail 寄送、Drive 附件鏈接、外部 frontend 深連結，都屬於行動層的一部分。

### 平台分工

- `Excel`：schema 藍本
- `Google Sheet`：雲端執行資料庫
- `GAS`：後端邏輯與 API
- `HTML`：外部前端與展示語言
- `Drive`：原始附件層

### V1 主體物件

- `source`：一筆被收進來的來源單位
- `attachment`：指向 Drive 的檔案關聯
- `activity_log`：可擴展的行為紀錄
- `user_account`：輕量登入控管
- `app_config`：系統設定與前端基底網址

## 資料流

### Flow A｜Create Source

- 使用者從外部前端送出最小欄位
- GAS 驗證登入、必填欄位與 dedupe hint
- 寫入 `source`
- 同時寫入 `activity_log`

### Flow B｜Attach Drive File

- 使用者提供 Drive URL 或 file ID
- GAS 驗證 source 存在、檔案可讀
- 寫入 `attachment`
- 同時寫入 attachment log

### Flow C｜Manual Status

- 使用者手動把 `status` 從 `inbox` 改成其他狀態
- GAS 驗證合法值
- 更新 `source`
- 寫入 `status_changed` log

### Flow D｜Send to Self

- 使用者觸發寄給自己
- GAS 讀取 source detail
- 組合 email 內容
- 寄出
- 寫入 `email_sent` log

### Flow E｜Open from Email

- 使用者點開 email 深連結
- 外部 frontend 讀取 `source_id`
- 呼叫 GAS 的 `get_source_detail`
- 顯示 source 與 attachment

### 目前已驗證成功的完整鏈路

Google Sheet / Drive → GAS backend → Email 深連結 → 外部 frontend → Source detail

## 功能

### Source List

目前可展示的代表資料有兩筆：

#### 測試來源 02

- status: `inbox`
- type: `note`
- ref: `manual-test-02`
- 已成功掛上一個 Drive 附件
- 已成功寄送至 Gmail

#### 測試來源 01

- status: `inbox`
- type: `note`
- 曾被 duplicate 檢查攔下
- 可作為 dedupe 行為示例

### Source Detail

目前展示版使用的代表資料：

- title: `測試來源 02`
- status: `inbox`
- type: `note`
- reference: `manual-test-02`
- note: `這是第二筆手動測試資料`
- clean text: `這是第二筆 clean text 測試內容`

### Attachment 顯示

目前示例附件：

- `Kimi_Agent_AI回覆概念總結.zip`

### Status 操作

V1 支援的狀態：

- `inbox`
- `processing`
- `done`
- `archived`

重要原則：

- 只人工控制
- 不自動流轉
- 不由 AI 覆寫

### Send to Self

目前 email 會包含：

- source title
- status
- reference
- note / clean text
- attachment 連結
- Open in FALO Frontend 深連結

## Play-SOP

這裡的 Play-SOP 不是 function，也不是 module。

它描述的是：

一段可被執行、可被理解、可被重建的能力單元。

### create_source

- 行為：建立一筆 source
- 流程：輸入最小欄位 → 驗證 → dedupe hint → 寫入 source 與 activity_log
- 系統位置：Capture + Core
- 資料：`source`, `activity_log`
- 工具：HTML, GAS, Google Sheet
- V1 對應：`save_source`
- 設計意圖：先收進來、之後再整理，避免資料遺失或分散

### attach_file

- 行為：把 Drive 檔案掛到既有 source
- 流程：提供 Drive URL / file ID → 驗證檔案 → 寫入 attachment 與 log
- 系統位置：Core + Execution
- 資料：`attachment`, `activity_log`, `source_id`
- 工具：HTML, GAS, Google Drive, Google Sheet
- V1 對應：`add_attachment`
- 設計意圖：保留原始資產，不把原始檔內容硬塞進 source 主表

### update_status

- 行為：人工改變 source lifecycle
- 流程：選擇下一個 status → 驗證合法值 → 更新 source → 寫入 log
- 系統位置：Core
- 資料：`source.status`, `activity_log`
- 工具：HTML, GAS, Google Sheet
- V1 對應：`update_source_status`
- 設計意圖：讓 status 成為流程骨架，但保留人機協作的判斷權

### send_to_self

- 行為：把一筆 source 送回自己
- 流程：取得 source detail → 組合 email → 寄出 → 寫入 `email_sent` log
- 系統位置：Execution
- 資料：`source`, `attachment`, `activity_log`, `user_account`
- 工具：GAS, Gmail, Google Sheet, Drive
- V1 對應：`send_source_to_self`
- 設計意圖：讓系統不只收資料，還能把資料推回行動現場

### open_source_from_email

- 行為：從 email 深連結重新打開 source
- 流程：點開前端連結 → 讀取 `source_id` → 呼叫 GAS detail API → 顯示來源內容
- 系統位置：Execution + Frontend
- 資料：`app_config.FRONTEND_BASE_URL`, `source`, `attachment`
- 工具：HTML, GAS, Gmail
- V1 對應：外部 `index.html` + `get_source_detail`
- 設計意圖：把 email 從終點變成入口，形成可回流的知識操作鏈
