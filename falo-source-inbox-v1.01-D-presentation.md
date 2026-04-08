Last Updated: 2026-04-08 20:05:55 Asia/Taipei

# FALO Source Inbox V1

呈現版草稿（D 版）

版權宣告：Copyright © Force Cheng 2026. All rights reserved.

---

## 這是什麼

FALO Source Inbox V1 不是 AI 平台，也不是完整 KM。

它先解一個更基礎的問題：

**把來源收進來，保留原始參照，手動管狀態，必要時再送回自己。**

---

## 一句話定義

**一套 Google-first、HTML-first 的 Source Inbox 與狀態管理系統。**

---

## 這一版要解的痛點

- 資料分散
- 收了之後找不到
- 有附件但回不到原始脈絡
- 很難知道目前處理到哪
- 很難快速把一筆資料送回行動現場

---

## 這一版是什麼

- Source Inbox
- 狀態管理系統
- Google Sheet / Drive / GAS 系統
- 可教、可理解、可重建的底座

---

## 這一版不是什麼

- 不是完整 KM
- 不是 AI workflow engine
- 不是 relation / container 系統
- 不是 Telegram bot

---

## 平台策略

- `Excel`：schema 藍本
- `Google Sheet`：雲端資料庫
- `Google Drive`：附件層
- `GAS`：後端邏輯與 API
- `HTML`：外部前端與輸出語言

---

## 三層理解

- `Capture`
  收來源、建 source、補 note 與 reference

- `Core`
  管理 `source`、`attachment`、`activity_log`、`user_account`、`app_config`

- `Execution`
  Email、Drive 連結、前端深連結、回流到操作現場

---

## 核心物件

- `source`
  一筆被收進來的來源單位

- `attachment`
  指向 Drive 的檔案關聯

- `activity_log`
  跨物件行為紀錄

- `user_account`
  輕量登入控管

- `app_config`
  系統設定與前端網址

---

## 核心資料流

**External HTML → GAS API → Validation → Sheet row → Optional Drive linkage → Manual status → Query → Email output → Audit trail**

---

## 五個核心流程

1. Create Source  
2. Attach Drive File  
3. Manual Status Change  
4. Send to Self  
5. Open from Email

---

## Flow 1：Create Source

- 外部前端送出最小欄位
- GAS 驗證登入與必填欄位
- dedupe hint 檢查
- 寫入 `source`
- 同時寫入 `activity_log`

---

## Flow 2：Attach Drive File

- 輸入 Drive URL / file ID
- 驗證 source 存在
- 驗證 Drive file 可讀
- 寫入 `attachment`
- 同時寫入 attachment log

---

## Flow 3：Manual Status

- `status` 是主幹
- 但一律人工控制
- 不自動流轉
- 不由 AI 覆寫

V1 狀態：

- `inbox`
- `processing`
- `done`
- `archived`

---

## Flow 4：Send to Self

寄給自己的 email 會包含：

- source title
- status
- reference
- note / clean text
- attachment 連結
- frontend 深連結

---

## Flow 5：Open from Email

- 使用者點 email 裡的深連結
- 進入外部 frontend
- 讀取 `source_id`
- 呼叫 GAS `get_source_detail`
- 顯示 source 與附件

---

## 已驗證成功的完整鏈路

**Google Sheet / Drive → GAS backend → Email 深連結 → 外部 frontend → Source detail**

這代表 V1 不只是設計完成，而是端到端已經跑通。

---

## V1 目前已成立的能力

- 可以登入
- 可以建立 source
- 可以提示 duplicate
- 可以掛 Drive 附件
- 可以留下 activity log
- 可以寄給自己
- 可以從 email 深連結回到 frontend

---

## 一筆代表資料

### 測試來源 02

- type: `note`
- status: `inbox`
- ref: `manual-test-02`
- 已掛上一個 Drive 附件
- 已寄送到 Gmail
- 已可從前端深連結打開

---

## 這一版最重要的治理原則

- 原始資料要保留
- `status` 只人工控制
- duplicate 檢查是提醒，不是破壞
- `activity_log` 要能擴展到更多物件
- 文件描述系統行為與資料流，而不是只描述 code

---

## FALO Play-SOP 是什麼

Play-SOP 不是 function，不是 module，不是 code。

它是：

**一段可被執行、可被理解、可被重建的能力單元。**

---

## V1 的五個 Play-SOP 候選

- `create_source`
- `attach_file`
- `update_status`
- `send_to_self`
- `open_source_from_email`

---

## 為什麼要有 Play-SOP

因為我們不是只在做系統。

我們要把系統變成：

- 可教
- 可理解
- 可重建

---

## A / B / C / D 四版本模型

- `A`：實際版  
  跑系統

- `B`：POC 版  
  看系統

- `C`：教學版  
  學系統

- `D`：呈現版  
  傳播系統

---

## 四層關係

`A（實際系統）`
→ 抽象成 `B（POC）`
→ 結構轉譯成 `C（教學）`
→ 視覺化轉換成 `D（呈現）`

核心原則：

**四者同源，不互相替代。**

---

## 這一份 D 的角色

這份內容不是 runtime，也不是教材全文。

它是：

- 給 NotebookLM 的輸入材料
- 給簡報的母稿
- 給圖卡 / mindmap 的摘要骨架
- 給快速講解時的主線版本

---

## 最後一句

**系統只寫一次，但可以有四種表達方式。**

FALO Source Inbox V1 現在已經具備：

- A：可跑
- B：可看
- C：可學
- D：可傳播

---

版權宣告：Copyright © Force Cheng 2026. All rights reserved.
