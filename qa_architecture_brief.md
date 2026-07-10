# AI Test 分層架構簡報版

## 1 頁簡報版條列

### 架構目的
- 將 AI 測試流程從「一次性 prompt」升級為「可重複、可維護、可擴充」的分層架構。
- 把流程、裝置、帳號、語系、驗證標準拆開管理，降低人為輸入差異與平台耦合。
- 讓同一套測試能力可以跨裝置、跨國別、跨場景重用，同時保留管理層可追蹤的測試證據。

### 五層結構
- **核心規則層**：`CLAUDE.md` 定義全域規則、載入順序與專案共通原則，是整個 AI 測試系統的總控入口。
- **命令入口層**：`.claude/commands/` 將高頻任務封裝成標準化入口，例如 `purchase-content-flow`，降低每次人工重寫 prompt 的成本。
- **流程編排層**：`flows/` 將登入、搜尋、購買、播放、交易紀錄等行為拆成模組，負責串起 SOP、裝置設定與 testcase。
- **配置資料層**：`devices/`、`accounts/`、`lang/`、`elements/` 將變動資訊抽離，處理裝置差異、帳號切換、語系差異與 UI 定位問題。
- **驗證輸出層**：`testcases/` 定義驗證標準，`reports/` 與 `screenshots/` 保存結果與證據，支撐回溯、審查與管理決策。

### 具體應用方式
- 當團隊要執行一條測試任務時，先由 `commands` 提供標準入口，再由 `flows` 串接對應 SOP、裝置與 testcase。
- 如果更換國別、帳號、裝置或語系，不需要改 flow 本身，只需替換對應設定檔與資料層內容。
- 若未來擴充新流程，例如退款、訂閱、家長控制或播放 DRM，只需新增 flow 或 testcase，而不用重寫整體架構。

## 樹狀圖版面

```text
AI_TEST/
├─ CLAUDE.md
│  └─ 全域規則與總控入口
│
├─ .claude/
│  ├─ commands/
│  │  └─ purchase-content-flow.md
│  │     └─ 標準化任務入口
│  ├─ sop_android_tv.md
│  ├─ sop_mobile.md
│  ├─ sop_mobile_chrome.md
│  └─ sop_tvos.md
│     └─ 各平台 SOP 與執行規範
│
├─ flows/
│  ├─ login.md
│  ├─ purchase.md
│  ├─ playback.md
│  ├─ transaction_history.md
│  ├─ search_movie.md
│  ├─ parental_control_pin.md
│  ├─ video_favorite_list.md
│  └─ view_profile.md
│     └─ Flow 用來串起 SOP、裝置、testcase
│
├─ testcases/
│  ├─ TC-AUTH-001_login.md
│  ├─ TC-E2E-001_login_purchase_chain.md
│  ├─ TC-HISTORY-001_transaction_history.md
│  ├─ TC-PLAYBACK-001_playback_drm.md
│  ├─ TC-PURCHASE-001_purchase_single_...
│  └─ ...
│     └─ 定義驗證標準與預期結果
│
├─ devices/
│  ├─ iphone.env
│  ├─ pixel7.env
│  ├─ pixel8.env
│  ├─ appletv.env
│  ├─ chromecast.env
│  ├─ mibox3.env
│  └─ mobile_chrome.env
│     └─ 各裝置相容性與執行配置
│
├─ accounts/
│  ├─ login.tw.env
│  ├─ login.sg.env
│  └─ login.id.env
│     └─ 帳號與環境切換
│
├─ lang/
│  ├─ zh-TW.env
│  ├─ en-GB.env
│  └─ id-ID.env
│     └─ 多語系驗證
│
├─ elements/
│  └─ web_elements.json
│     └─ UI 元素定位資料
│
├─ knowledge/
│  ├─ accounts.md
│  ├─ command_conventions.md
│  ├─ device_config.md
│  ├─ ios_tvos.md
│  ├─ tv_automation.md
│  ├─ web_chrome_ecpay.md
│  └─ allpay_page_structure.html
│     └─ 補充知識、規範與頁面結構
│
├─ reports/
│  └─ 測試報告輸出
│
├─ screenshots/
│  └─ 測試證據截圖
│
├─ scripts/
│  └─ 執行腳本與輔助工具
│
└─ .agents/skills/
   └─ agent-device/SKILL.md
      └─ AI 額外技能模組
```



### Test Credit Card

# Taiwan region — credit cards
# ── Credit cards ──────────────────────────────────────────────────────────
CARD_EXP=0636               # 06/2036 → MMYY
CARD_CVC=737                # Master CVC (all TW test cards share 737) — mirrors sg/id for cross-region symmetry

CARD_1_NAME="台灣一般信用卡1"
CARD_1_NO=4311951111111111
CARD_1_CVC=737

CARD_2_NAME="台灣一般信用卡2"
CARD_2_NO=4311952222222222
CARD_2_CVC=737

CARD_3_NAME="台灣海外信用卡"
CARD_3_NO=4000201111111111
CARD_3_CVC=737

CARD_4_NAME="台灣美國運通國內卡"
CARD_4_NO=340353278080900
CARD_4_CVC=737

CARD_5_NAME="台灣美國運通國外卡"
CARD_5_NO=371222222222222
CARD_5_CVC=737

CARD_6_NAME="台灣永豐30期信用卡"
CARD_6_NO=4938177777777777
CARD_6_CVC=737


# Indonesia region — credit cards
# ── Credit cards ──────────────────────────────────────────────────────────
CARD_EXP=0330               # 03/2030 → MMYY
CARD_CVC=737
# Per-card CVC aliases (all cards share same CVC) — required by SOP scripts referencing $CARD_N_CVC
CARD_1_CVC=$CARD_CVC
CARD_2_CVC=$CARD_CVC
CARD_3_CVC=$CARD_CVC
CARD_4_CVC=$CARD_CVC
CARD_5_CVC=$CARD_CVC
CARD_6_CVC=$CARD_CVC

CARD_1_NO=4444333322221111
CARD_2_NO=4111111111111111
CARD_3_NO=4000160000000004
CARD_4_NO=4166676667666746
CARD_5_NO=4000620000000007
CARD_6_NO=4293189100000008


# Singapore region — credit cards
# ── Credit cards ──────────────────────────────────────────────────────────
CARD_EXP=0330               # 03/2030 → MMYY
CARD_CVC=737
# Per-card CVC aliases (all cards share same CVC) — required by SOP scripts referencing $CARD_N_CVC
CARD_1_CVC=$CARD_CVC
CARD_2_CVC=$CARD_CVC
CARD_3_CVC=$CARD_CVC

CARD_1_NO=4444333322221111
CARD_2_NO=4111111111111111
CARD_3_NO=4000160000000004