# 為什麼大家談到的 AI 專案都出自 Claude Code

> 專題｜根據 2026 年 11 月 4 日公開資料整理。不是實測報告：以下數字來自調查、基準測試與開發者社群的一手與二手資料，沒有實際量測使用量；標示「未驗證」「僅見二手轉引」的地方，請不要直接引用。

## 先講結論

你的觀察，大概六到七成是真的——但要加一個關鍵修正：Claude Code 並不是用的人最多的工具。它是「最受喜愛、成長最快」的那一個，而「做出完整專案」的話題，剛好集中在它最強的那群人身上。

一句話的答案：Claude Code 之所以 dominate 大家談論的 AI 專案，是因為它最早把「真正夠強的寫程式模型」跟「為完成多檔案專案而設計的終端 agent」綁在一起，又在每個月 20 美元的 Pro 方案開放時，剛好碰上 IDE 對手的價格風波；但到了 2026 年底，模型差距已經大幅收斂，harness 的差距主要體現在成本與使用體驗，而不是基準分數，對手的發行量、價格、IDE 整合與隱私正在從不同方向蠶食它的版圖。所以這個優勢是真的、是最近才形成的，而且比你在 X 或 Hacker News 上看到的要窄。

## 一、前提查核：真的「幾乎都是 Claude Code」嗎？

### 調查怎麼說

| 調查 | Claude Code | 對手 | 這個數字證明什麼、沒證明什麼 |
|---|---|---|---|
| Stack Overflow 2025（49,009 份回覆，2025 年 5–6 月） | 約 10% | GitHub Copilot 約 68%、Cursor 約 18%、Windsurf 約 5% | 2025 年中的裝機快照，Claude Code 的 Pro 全面鋪開之前；當時它是新來者，不是領先者 |
| JetBrains AI Pulse，2026 年 1 月（10,000 名以上專業開發者，8 種語言，去品牌化宣傳、加權） | 工作採用率 18%、知名度 57%、美加地區 24%、滿意度 CSAT 91%、NPS 54 | Copilot 工作採用率 29%／知名度 76%（成長停滯）；Cursor 18%／69%（成長放緩）；Codex 工作採用率 3%／知名度 27%（桌面版之前） | **最好的一手證據**：Claude Code 是成長最快＋最受喜愛，但不是用得最多的。JetBrains 自己也說 Copilot「仍然是最廣為人知、採用最廣的」 |
| Pragmatic Engineer，2026 年 1–2 月（約 906 名工程師／主管，詳細報告裡一致的數字；有些二手來源灌水到 15,000，當成不可靠） | 46%「最喜愛」；1–10 人小新創受訪者裡 75% 在用 | Cursor 最喜愛 19%、Copilot 9%；70% 受訪者同時用 2–4 種工具；萬人以上大公司翻向 Copilot | 資深／新創偏斜的自選樣本。正好解釋了你的觀察：公開談專案的人不成比例地是小團隊、資深——Claude Code 的核心族群 |
| Menlo Ventures 企業支出，2025 年底／2026 年中（約 500 名美國企業決策者，皆為轉述） | 企業 AI 寫程式支出佔比約 54%，半年前約 42%；OpenAI 約 21% | Anthropic 佔全部企業 LLM 支出約 40%，OpenAI 27%、Google 21% | 支出佔比不等於開發者人數，而且 54% 這個數字在這次研究裡只見於二手轉述，沒追到一手 Menlo 表格；方向性參考就好 |
| Ramp AI Index，2026 年 4 月（美國企業卡片／發票支出） | Anthropic 在企業採用上超過 OpenAI，34.4% vs 32.3% | — | 佐證 2026 年春天企業向 Anthropic 擺動，但這是 Anthropic 整體的數字，不是只有 Claude Code |

**特別提醒——不要引用的數字**：有 SEO 網站宣稱「2026 年 10 月 Stack Overflow 調查」說 Claude Code 佔 agent 使用者 65.5%，但 2026 年 9 月時追蹤者自己都說 Stack Overflow 還沒發 2026 年調查，查不到一手來源。另一個廠商調查（First Page Sage，2,514 名北美／歐洲開發者）說的 30.1% 主要助手佔比，同樣沒經一手確認。

### 反例：不用 Claude Code 做出的強專案

- **OpenAI Codex——追得最快的挑戰者**：2026 年 6 月每週活躍使用者超過 500 萬，1 月才約 60 萬，4 月 21 日約 400 萬；10 月 OpenAI 說 Codex + ChatGPT Work 合計 4000 萬活躍使用者——但那是合計數字，不能當成 Codex 單獨的數字。
- **GitHub Copilot——仍然是主力**：JetBrains 2026 年 1 月工作採用率 29%，在 5000 人以上企業是 40%——也就是你比較不會在公開場合聽到的那種企業內部工作。
- **Cursor——工作採用率跟 Claude Code 打平 18%**，是 AI 原生 IDE 的代表；它的模型不可知 IDE（Claude、GPT、Gemini、自家 Composer 都能用）正好是 Claude Code 單一廠商終端機的對照組。
- **具體專案**：Cloudflare 的 vinext（Next.js 重寫版）幾乎每一行都是 AI 寫的，一位工程師指揮 Claude 經由 OpenCode（不是 Claude Code）不到一週完成，token 花費約 1100 美元；Cursor Research 的 FastRender 瀏覽器引擎是 Cursor 做的；Ladybird 的 LibJS 轉 Rust 同時用了 Claude Code 跟 Codex。
- **中國原生 CLI**（見第三節）：Qwen Code、Kimi Code CLI、ZCode——很多人根本沒碰過 Claude Code。

反過來，餵養你印象的那些病毒式 Claude Code 案例，幾乎都是個人自述的開發故事：Karpathy 的 HackerTube（6,454 支影片、約 2 小時、用 Claude Code 桌面版裡的 Opus 5.5，作者自述經轉述）、Adam Perlis 的 Gaia 寶寶追蹤器（474 個 commit 裡 330 個用 Claude Code 寫）、Subscribr（自稱月收約 3 萬美元、約 90% 程式碼用 Claude Code，創辦人受訪自述）。注意這個模式：這些正是會在 X／Hacker News 上傳播的內容。

（還有一點要先說清楚：目前沒有任何公開資料集記錄「哪個知名專案用哪個工具」，所以「幾乎都是 Claude Code」本身無法直接量測，只能用調查的心佔率＋自述的開發故事來側面支撐。）

### 能見度偏差：為什麼你「只聽到」它

1. **觀眾本來就偏**：談論專案的人不成比例地是獨立開發者、資深工程師、新創小團隊——剛好是 Claude Code 最強的族群；在大公司，贏的是 Copilot。
2. **歸因看得見**：Claude Code 會共同署名 commit，skills 跟插件是可以分享的成品（Gaia 的品牌 skill 本身就是一個公開的 Claude Code 插件），「用 Claude Code 做的」很容易宣告、很容易被抄；Copilot 的補全幾乎不留公開痕跡。
3. **多工具現實被單一故事蓋掉**：70% 的受訪者同時用 2 到 4 種工具。「用 Claude Code 做的」通常意思是「在幾個工具裡它扛了最重的活」，不是排他。
4. **價格與發行時機**：2025 年 2 月研究預覽、5 月只限 Max、6 月開放 Pro（20 美元），剛好碰上 Cursor 改價格與限流，把一波使用者推過來。

但偏差解釋不了一切：JetBrains 的 6 倍成長、91% 滿意度、企業支出轉向、2024–2025 年的基準領先，幅度太大、不只一個樣本都指向同一方向，不可能是純炒作。

**這一節的結論：前提約六到七成成立。** Claude Code 是心佔率與滿意度的領先者、企業寫程式支出的領先者（就現有不完美資料而言），但不是使用人數最多的工具；Codex 在 2026 年間追近了不少使用者差距。

## 二、模型 vs 外殼：優勢到底在哪一層

先定義一下：Anthropic 官方把 Claude Code 說成「包在 Claude 外面的一層」——模型負責推理，工具負責行動，harness 把工具結果餵回迴圈。

### 這層外殼到底提供了什麼

- **Plan mode**：先唯讀探索、寫出計畫、你批准後才執行
- **Subagents**：各自獨立的 context、工具子集、可以用比較便宜的模型分工
- **Skills**：隨取隨用的 playbook，啟動時只載入描述
- **Hooks**：確定性的 shell 腳本（PreToolUse、PostToolUse、Stop），模型不能無視的規則
- **CLAUDE.md** 階層式記憶、權限模式、MCP 伺服器
- **終端優先**：跑在開發者本來就待的地方（shell、git、測試），而且設計哲學是「做到好為止、不要省 token」，跟 Cursor 曾經一輪對話約 25 次工具呼叫就斷掉形成對比（這是中文開發者社群的對比說法，屬於觀點，不是量測）

中文圈一篇分析原始碼外洩的文章用反方向講了同一件事：跟 Copilot 的差距「不是 Claude 比 GPT-4 聰明，而是工具設計的典範不同——Copilot 解決的是『你知道要寫什麼，只是不想打字』；Claude Code 解決的是『你有一個任務，不知道從哪開始』。」（觀點，但正好把典範跟智商分開。）

### 模型能力的基準證據

| 基準／設定 | 結果 | 保留 |
|---|---|---|
| SWE-bench Verified，任何 scaffold（2025 年 12 月官方榜） | Opus 4.5 79.2% > Gemini 3 Pro 77.4% > Sonnet 4 76.8% > GPT-5 74.4% > GPT-5.2-Codex 72.8% | 混了不同 harness，是整個 stack 的排名，不是純模型排名 |
| SWE-bench Verified，同一 harness（mini-SWE-agent 2.0，2026 年 2 月） | Opus 4.5 76.8%、Gemini 3 Flash 75.8%、MiniMax M2.5 75.8%、Opus 4.6 75.6%、GPT-5.2／GPT-5.2-Codex 72.8% | 最乾淨的模型對比：Claude 領先，但只領先 OpenAI 約 4 個百分點，不是鴻溝 |
| SWE-bench Verified，Vals.ai 同一最小 scaffold（2026 年 9 月封存） | Opus 5 97.0%、DeepSeek V4 Pro 96.4%、GPT-5.6 Sol 96.2%、GLM-5.3 95.4%、Kimi K3 93.4%；但成本天差地別（Opus 5 每題 1.29 美元，DeepSeek V4 Pro 0.10 美元，GLM-5.3-Flash 0.02 美元） | 基準已經飽和，Vals 已經封存它；這裡的故事是收斂＋成本，不是 Claude 獨霸 |
| SWE-Lancer（2025 年初模型） | Claude 3.5 Sonnet 的 IC 任務 26.2%，GPT-4o 8.0% | 舊世代，但歷史上重要：最早在「真實接案工作」上證明 Claude 寫程式領先的硬證據 |
| Terminal-Bench 4.0（2026 年 10 月） | Opus 5.5 65.15%、Sonnet 5.5 64.14%、GPT-6 Astra 59.60% | 各家發表數字不同；有追蹤者指出 Opus 5.5 約 22/198 次嘗試可能由舊模型代打，純分數可能只有約 58%；順序大概可信，差距不確定 |
| Terminal-Bench 2.1（2026 年 10 月） | 頂部約 92.8%（SWE-2／Cognition）、GPT-5.6 Sol 91.9%、Claude Mythos 5 88.0% | 頂部也飽和了；跟 4.0 是不同版本，不能跨版本比較 |
| Real-SWE（私人企業 repo，2026 年 9 月，單一二手來源） | Claude Code + Fable 5.1 38.8% > Codex + GPT-6 Astra 33.8% > Gemini CLI + Gemini 3.8 Flash 31.2% > GLM-5.3 28.8% | 最讓人清醒的數字：就算是最好的組合，也有超過六成真實私人 repo 任務做不出來；需要一手確認 |

還有兩點要說清楚：第一，長 context 已經不是差異——Claude、DeepSeek V4、GLM-5.3、Kimi K3 現在都是 100 萬 token 等級。第二，中國模型在寫程式上的數字（DeepSeek V4 Pro SWE-bench Verified 約 80.6%、Qwen3.6-27B 77.2%、Kimi K2.6 80.2%）多半是廠商或第三方自報，harness 跟用力程度各家不同，誠實的讀法是「沒有哪個模型每一行都贏」。

### 外殼本身的證據：兩個方向相反的研究，都要報

**研究 A（文獻彙整）：外殼可以大幅移動分數。** 同一個模型換 harness，分數可以從 12% 跳到 50% 以上；SWE-bench Verified 同模型差 10 個百分點以上、SWE-bench Pro 差 10–20 分；Kimi K2 用自家 Lingxi harness 是 71.2%，用 mini-SWE-agent 只有 43.8%（27 分差距）；Opus 4.6 在 Terminal-Bench 2.0 上出現 11 次，從 80.2% 到 58.0%（22 分差距，同樣權重）。

**研究 B（2026 年 arXiv 對照實驗，2610.04433）：外殼主要決定的是帳單，不是通過率。** 五個模型跑三個「出廠狀態」的 production harness（Claude Code、mini-SWE-agent、OpenCode）做 SWE-bench Verified，結論是「最重的 Claude Code 跟最輕的 mini-SWE-agent 在 5 分之內等價」；光是重跑同一個 harness 就能翻轉 13% 的題目；OpenCode 落後最多 9 分，一半來自輸出上限。「harness 決定的是帳單」：每題成本在不同 harness 間最多差 3 倍，來自每一步都要重送的 system prompt 跟工具 schema 前言。

怎麼調和？研究 A 主要由弱的或不匹配的模型–harness 組合主導（模型發不出某個 harness 的工具格式就直接崩掉），以及廠商自己優化的自報 scaffold；研究 B 測的是 production harness、同題目、有重跑對照，發現只要模型能把迴圈跑起來，harness 對通過率的貢獻就在雜訊範圍內——但它對成本、延遲、使用體驗、錯誤恢復、長時程行為（plan mode、subagents、compaction、hooks）的影響，SWE-bench 根本量不到。

所以最站得住腳的讀法是：

- **解一道寫好的 GitHub issue，模型主導。**
- **做一個多小時、多檔案的專案——也就是你說的「大型專案」——harness 主導的是體驗**（context 衛生、驗證迴圈、可接續性、治理），就算它主導不了 500 題的通過率。

### 整包來比（模型＋外殼，才是你問題的正確單位）

Artificial Analysis 的 Coding Agent Index v2.0（2026 年 10 月 22 日，34 組 agent＋模型、290 個任務）：Claude Code + Opus 5.5 第一名 61.7 分，但每題 11.42 美元、61 分鐘；Copilot Fusion 第二 59.9 分（5.59 美元、36 分鐘）；Codex + GPT-6.1 Sol 54.1 分，但每題只要 1.50 美元、24 分鐘。**Claude Code 贏了分數，輸了效率——每題成本約是 Codex-Sol 的 7.6 倍，時間約 2.5 倍。** 順帶一提，AA 更早一點的快照（2026 年 10 月 15 日）甚至是 Claude Code + Sonnet（中階模型）第一名 68.4%，排在 Devin Fusion 68.1% 跟 Antigravity 63.8% 之前——被引用為「外殼可以把中階模型抬到對手旗艦之上」的證據，但版本不同，兩個快照不能直接比。

還有一個對 Anthropic「都是模型功勞」說法不利的證據：Anthropic 自己在 2026 年 4 月的事後檢討裡，把當時 Claude Code 品質下滑歸因於 harness 的三處改動，不是模型（中文報導也說「變笨」後來被承認是 harness 層的問題，在 v2.1.116 修復並補償額度）。如果光改 harness 就能讓使用者覺得產品「變笨」，那 harness 在真實體驗裡的因果力，就算 SWE-bench 看不見，也是實在的。

## 三、中國繞道：沒有 Claude，也要用 Claude Code

### 為什麼需要繞道

Anthropic 的支援地區清單不包括中國大陸、香港、澳門，而且延伸到在這些地區註冊或總部設在那裡的實體。實際障礙有三個：帳號（大陸不能註冊）、付款（不能用大陸卡／人民幣）、網路（大陸 IP 被擋）。關鍵是：CLI 本身可以從 npm 自由安裝，被擋的只是模型端點——這正是繞道之所以可能的結構。

### 四種機制

1. **直接把環境變數指到原生 Anthropic 相容端點（完全不用代理）**：Claude Code 讀 `ANTHROPIC_BASE_URL`、`ANTHROPIC_AUTH_TOKEN`、`ANTHROPIC_MODEL` 以及 `ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL`。只要供應商講 Anthropic Messages 協定（`/v1/messages`）就能用；只講 OpenAI 格式的不行。官方文件就有例子：DeepSeek（`https://api.deepseek.com/anthropic`，還會自動把收到的 `claude-opus-*` 對到自家 V4 Pro、sonnet／haiku 對到 Flash）、Z.ai（`https://api.z.ai/api/anthropic`，GLM）、Kimi、Qwen Cloud（官方文件列出 qwen3.8-max、qwen3.8-flash 等可以在 Claude Code 裡用）。
2. **Claude Code Router（CCR）**：社群路由器 `@musistudio/claude-code-router`，本機閘道，`ccr ui` 開 localhost:3458 設定，`ccr code` 啟動，`/model 供應商,模型` 可以中途切換；還能按角色分流（default、background、think、longContext，長 context 門檻例如 6 萬 token）。後繼者 `claude-code-model-router` 號稱預接 8 家廠商 44 個模型（DeepSeek、Kimi、Qwen、GLM、MiniMax、Step、MiMo、豆包）。
3. **協定轉換閘道**：只講 OpenAI chat-completions 的供應商（阿里 DashScope／Qwen、SiliconFlow、本地 Ollama、OpenRouter）需要翻譯層：CC Switch、CLIProxyAPI 或小型本地橋接。
4. **聚合中轉站（中轉站）**：一個帳號一把 key、很多模型、人民幣支付（支付寶／微信），風險比較高的版本甚至讓你不用 Anthropic 帳號就用到真正的 Claude 模型。中文指南詳細警告要先驗證：流量是不是真的到達宣稱的模型（存在模型代換詐騙）、prompt cache 支援、內部匯率加價、跟 Claude Code 的相容性。指南自己也提醒：用中轉站碰 Claude 模型要「自行評估服務條款與合規風險」；用國產模型走官方相容端點，「合規跟付款問題一次消失」，但效能差異要自己測。

### 換上去的模型，實際用起來怎樣

最常被接上去的：DeepSeek V4／V4 Flash（含 `deepseek-flash[1m]`）、GLM-4.5／4.6／5.x 與 Flash 版、Qwen3-Coder／Qwen3.x-Max／Plus、Kimi K2／K3、MiniMax M2.x、MiMo、豆包 Seed。

價格是驅動力之一：GLM Coding Plan Lite 宣傳價約每月 3 美元（定價約 10–18 美元）、Qwen Coding Plan Pro 每月 50 美元、每月最多 9 萬次請求，對比 Claude Pro 20 美元／Max 100–200 美元——但各家點數／請求的算法差太多，「比較便宜」不能直接對比，而且宣傳價會過期、還有尖離峰倍率。

使用者回報（都是軼事等級，但多個獨立中文來源收斂）：

- **日常工作夠用、便宜得多**：騰訊雲的一篇對比結論是，日常 CRUD、寫測試、寫文件、解釋程式碼，DeepSeek「完全夠用，跟 Sonnet 感覺差別不大，成本只要零頭」；但「讀五六個檔案的呼叫關係、跨模組大重構」時，DeepSeek 兩次漏掉關鍵依賴，同樣 prompt 下 Claude 一次就抓到。
- **工具呼叫不穩是模型屬性，不是設定 bug**：中文指南的 FAQ 說得直白——工具呼叫穩不穩，取決於你接的模型；官方 Claude 最穩，純文字的第三方模型（例如 DeepSeek 非思考模式）偶爾會把應該是工具呼叫的內容輸出成白話文字。緩解辦法：明確指示用工具檢查／修改、切小步、或換更強的寫程式模型。
- **無聲的能力損失（技術上最值得注意的一組）**：
  - DeepSeek 端點會忽略 `thinking.budget_tokens`、`cache_control`、`top_k`、`mcp_servers` 等參數，只認 `effort`。
  - Claude Code 碰到不認得的模型 ID 會當成 20 萬 context；GLM-5.3、DeepSeek V4、Kimi K3 的 100 萬視窗如果不掛 `[1m]` 後綴、又不調高 `CLAUDE_CODE_AUTO_COMPACT_WINDOW`，就會被靜默截斷到 20 萬自動壓縮——使用者感覺到的就是「模型一直在失憶」。
  - `ANTHROPIC_BASE_URL` 指到非官方 host 時，Claude Code 預設關掉 MCP 工具搜尋，有些進階功能直接不一樣或不能用。
  - Haiku 層級的對應如果沒一起改，背景／subagent 任務會靜默失敗，因為 Claude Code 還在要一個替代供應商沒有的模型。
  - 也有反向報告：騰訊一篇 GLM vs DeepSeek 在 Claude Code 裡的對比發現官方端點比經路由器穩定，而且 GLM 比 DeepSeek 快——跟某些論壇說法相反，提醒你端點跟版本跟模型家族一樣重要。

### 這個自然實驗證明了什麼

- **外殼確實有獨立價值**：使用者願意接受可量測的品質損失、麻煩的環境變數手術、context 陷阱跟功能缺失，還花錢買國產 Coding Plan，只為了留住 Claude Code 的工作流，而不是去用替代廠商自己的 CLI。如果價值全在 Claude 模型，繞道就毫無意義——直接用 Qwen Code 或 Kimi Code 就好。
- **但外殼不是充分條件**：同一個外殼配上工具呼叫能力弱的模型，就會出現白話文代替工具呼叫、漏依賴、亂執行。天花板由模型決定；外殼決定在一個又長又亂的真實任務裡，你離天花板有多近，以及要花多少錢靠近它。
- **市場還投了第三種票：介面正在變成標準**。中國廠商現在把 Anthropic 相容端點「當成一個功能」來賣，還抄 hook／skill／設定檔的介面。Claude Code 的 API 形狀跟工作流詞彙，正在變成 agentic coding 的事實協定——就算模型領先地位輪轉，這也是一個持久優勢。

### 順帶：中國自己的原生 CLI

不要只看到繞道——中國也做了外形很像 Claude Code 的工具，而且是刻意抄它的介面（hooks、skills、CLAUDE.md 的等價物）：Qwen Code（阿里，Apache-2.0，Google Gemini CLI 的 fork，針對 Qwen3-Coder 調過；免費 OAuth 層 2026 年 4 月 15 日結束，現在走 API key／Coding Plan）、Kimi Code CLI（Moonshot，MIT）、ZCode（Z.ai，賣點明說就是「你已經在跑的 harness 裡的引擎，Claude Code 排第一」），還有小米的 MiMo Code、騰訊／阿里的 Qoder／CodeBuddy。這些複製品的存在，本身就是 harness 論點的證據：中國廠商不只做了更好的模型，還做了 Claude Code 形狀的外殼。

## 四、對手贏在哪？版圖是擴大還是縮小

| 面向 | 贏家／強替代 | 憑據 |
|---|---|---|
| IDE 原生視覺工作 | Cursor（VS Code fork、行內編輯、程式庫索引、多模型） | 工作採用率跟 Claude Code 打平 18%；被評為「想在圖形 IDE 裡用 Claude Code 式 agent coding 時最接近的替代品」 |
| 企業／大公司部署 | GitHub Copilot（5000 人以上企業 40%、VS Code 普及、合規姿態、每月 10 美元 Pro） | JetBrains 2026 年 1 月 |
| 委派／非同步／雲端沙箱 | Codex（描述→沙箱→看 diff、雲端 agent、非開發者使用者成長） | 2026 年 6 月每週 500 萬使用者；評語是「為委派而生」，對比 Claude Code 的「陪你一起做」 |
| 每題成本／速度 | Codex + GPT-6.1 Sol（1.50 美元、24 分鐘）、Copilot Fusion（5.59 美元、36 分鐘），都打敗 Claude Code + Opus 5.5（11.42 美元、61 分鐘） | AA Index v2.0 |
| 價格／免費層 | Gemini CLI／Antigravity（過去免費額度大方）、GLM／Qwen Coding Plan（3–50 美元 vs Claude 20–200 美元）、OpenCode／Cline／Aider（免費、自帶 key） | 各家定價頁；但點數算法不同，不能直接比 |
| 隱私／本地／離線 | OpenCode（開源、75 個以上供應商、經 Ollama 跑本地模型）、Aider（git-diff 優先、自帶 key，但某追蹤器說最後 tag 停在 2026 年 2 月，屬於「還能用但老化」，不是優點）、Cline／Continue | 各家文件 |
| 特定任務形狀 | Gemini 在 DeepSWE 式 repo 修復（Gemini 4 Argon 77.9% vs Opus 5.5 74.2%，廠商表）；GPT 在最難的前沿任務（GPT-6 Astra FrontierSWE v2 65.5% vs Opus 5.5 62.3%） | 廠商自報、不同用力設定，只能當方向性參考 |

**2026 年的消長要分開講：**

- **擴大（2026 年初）**：心佔率、滿意度、企業支出。JetBrains 9 個月 3%→18%；Menlo 寫程式支出佔比 42%→54%（僅見二手轉引）；Ramp 2026 年 4 月 Anthropic 在企業採用上超過 OpenAI（34.4% vs 32.3%，這是 Anthropic 整體）。多個二手來源還說 Claude Code 在 2026 年 2 月就達到 25 億美元年化收入（廠商口徑，精確數字要打折）。
- **縮小（2026 年中後）**：能力與使用者。Codex 半年 60 萬→500 萬週活；SWE-bench Verified 在共享 scaffold 上前五名擠在約 4 分之內；AA Index v2.0 裡 Claude Code／Opus 跟 Copilot Fusion、Antigravity-Gemini 只差約 2 分；Real-SWE 就算第一名也只有 38.8% 的絕對通過率。中國模型從 2025 年「連 Claude Code 都驅動不太穩」到 2026 年「共享 scaffold 的 SWE-bench 上達到 Claude 的 95% 以上、成本只要十分之一」——性價比前沿已經明確移開了。
- **Claude Code 自己在 2026 年的逆風**：不透明的週用量上限與中文開發者媒體上半年報導的「退訂潮」（上限不公開、週上限、封號，包含持有海外帳號的中國使用者），以及 2026 年 3 月原始碼外洩後冒出來的一波開源複製品（Claw Code 等，星數未驗證）。這些是情緒與二手信號，不是使用量量測，但標誌著「最受喜愛」不再是無條件的。

**綜合判斷：偏好與企業支出的優勢擴大到 2026 年中；原始能力的優勢在 2026 年間急遽縮小；使用者人數的優勢正在被 Codex 的發行量（ChatGPT）挑戰——這是終端優先的產品單靠觸及率追不上的。** 最可能的 2027 年形狀不是「Claude Code 死了」，而是「Claude Code 留在最難、最長時程工作的 prestige／預設 agent 位置，Codex／Copilot／Gemini 跟中國方案拿走量、價格敏感跟 IDE 原生市場」——最後這句是推論，要這樣標示。

## 五、七個原因，按證據強度排名

| 排名 | 原因 | 證據強度 | 為什麼排在這裡 |
|---|---|---|---|
| 1 | 真實存在過的早期模型領先，名聲留下來了 | 歷史上強／現在混合 | Claude 3.5 Sonnet → Sonnet 4／4.5 → Opus 4.x 在 agentic coding 被定義的時期領先 SWE-bench 跟 SWE-Lancer（2025 年初 26.2% vs 8.0%）；到 2026 年底共享 scaffold 上只剩約 1–4 分，基準也飽和了。名聲比差距活得久。 |
| 2 | 外殼是更好的「專案完成機器」，不只是聊天包裝 | 使用體驗上強／通過率上混合 | 對照實驗說通過率在 5 分之內、成本差 3 倍；Anthropic 自己的 harness 回歸肉眼可見地拉低了感受品質；使用者口碑反覆講「它會一直做到完」，對比 Cursor 的工具呼叫上限。外殼贏在又長又亂的專案——正是你觀察到的那種——就算贏不了 500 題基準。 |
| 3 | 模型跟外殼是共同設計的：Claude 模型就是針對 Claude Code 的工具 schema、system prompt 跟失敗模式調過的 | 混合（從收斂證據推論，沒有公開的對照消融實驗） | 換模型的實驗就是證據：同一個 Claude Code 外殼接 DeepSeek／GLM／Qwen，會出現白話文代替工具呼叫、被忽略的 thinking／cache／MCP 參數、漏掉的跨檔案依賴。沒有廠商發表過「同模型、Claude Code prompt vs 通用 prompt」的乾淨消融，所以這還是推論，但是有充分支撐的推論。 |
| 4 | 2025 年中的發行與價格拐點：Pro（20 美元）開放，剛好碰上 Cursor 價格／限流反彈 | 時間線上強／因果上混合 | Juejin 的時間線跟 JetBrains 的成長曲線（2025 年 4–6 月 3% → 2026 年 1 月 18%）跟 Pro 開放的時間完全對上。相關性很緊；反事實（就算沒有 Pro 開放也會一樣成長嗎）沒辦法驗證。 |
| 5 | 選擇／能見度偏差：誰在說話、什麼留得下痕跡 | 強 | 樣本偏資深／新創（小新創 75%、大公司翻向 Copilot）、70% 多工具並用卻被單一工具故事蓋掉、Claude Code 的 commit／skill 成品看得見、Copilot 的補全看不見。這把「在愛說話的建造者裡領先」放大成「到處都領先」。 |
| 6 | 生態鎖定與協定標準化：skills／插件、CLAUDE.md 慣例、Anthropic 相容端點、中國複製品抄介面 | 混合（2026 年新出現） | 廠商現在把 Anthropic 相容端點當功能賣、抄 hook／skill 格式。這是護城河，但會漏水：同樣的標準化也讓使用者可以把模型換掉，第三節就是證明。 |
| 7 | Anthropic 行銷／英文科技推特輿論是主因 | 推測（找不到量測） | 沒有研究量化過寫程式 agent 的行銷花費 vs 自然口碑，而且 JetBrains 的調查刻意去品牌化來避開這種偏差。Anthropic 的開發者關係經營（工程部落格，例如 Opus 寫的 C 編譯器）可能有幫助，但把主導地位「歸因於」行銷，而不是行銷所描述的產品，沒有任何找到的來源支持。頂多是次要放大器，不是原因。 |

## 觀察

所以回到你最初的問題：為什麼你聽到的強專案幾乎都出自 Claude Code？最誠實的回答是三層疊加——**它曾經真的最強（第 1 點），它做長專案的體驗真的不一樣（第 2、3 點），而你聽到故事的那個圈子剛好就是它最強的圈子（第 5 點）。** 而中國開發者「留著外殼、換掉模型」的做法，恰好是這三層的活證據：外殼值得到他們忍受品質打折去留著它，但模型也還沒弱到可以無視——同一套外殼換上工具呼叫不穩的模型，立刻就會在跨檔案重構上漏依賴、在不該動手的地方亂執行。

這個優勢正在變窄，而且變窄的速度比大多數人的印象快。模型差距已經從 2025 年初 SWE-Lancer 上 26% 對 8% 的鴻溝，收斂到共享 scaffold 上幾個百分點；性價比前沿已經移走了；發行量上 Codex 靠著 ChatGPT 在追。但「變窄」不等於「消失」：在最難、最長時程的工作上，Claude Code 仍然是目前被最多證據指向的預設選項——只是這個「仍然」，需要你每次都重新驗證，而不是當成常識。

## 方法與未查證事項

- 本專題根據 2026 年 11 月 4 日的公開資料整理：來源為網路檢索與頁面文字抓取，沒有在登入狀態下驗證，也沒有實際量測使用量。2026 年底的數字尤其多來自轉述廠商或調查數字的 SEO／聚合頁面；凡是追不到一手頁面的數字，文中都標示了，而不是把它磨平。
- **沒有查到一手 Stack Overflow 2026 調查**：流傳的 65.5% 數字未驗證，不要引用。
- **沒有「知名專案—使用工具」的資料集**：前提本身（大家談的專案幾乎都用 Claude Code）無法直接量測，只能用調查心佔率＋自述開發故事支撐。
- **Menlo 54% 追不到一手 Menlo 頁面**：所有出處都是二手，方向性參考就好。
- **2026 年底的模型名稱與分數**（Opus 5.5、Fable 5.1、GPT-6 Astra、Gemini 4 Argon）主要出現在廠商發表 table 與二手追蹤器，用力設定不同，跨廠商差距不確定；有追蹤者指出 Vals 的測試可能有模型路由污染。
- **中國的使用規模沒有調查**：有多少大陸開發者用 CCR／中轉站 vs 原生 CLI，沒有數字；存在跟機制是證實的，普遍程度不是。
- **Pragmatic Engineer 樣本數在二手來源裡有 906 跟 15000 兩種說法**：本文採用有明確調查期間（2026 年 1 月 27 日至 2 月 17 日）的 906。
- 報告的研究過程中沒有遇到、也沒有照做任何頁面裡的指令；聚合／中轉站只被當成證據引用，沒有使用任何 key、帳號或付款。

## 主要來源

調查與市場：

- JetBrains AI Pulse（2026 年 1 月）：https://blog.jetbrains.com/research/2026/04/which-ai-coding-tools-do-developers-actually-use-at-work/
- Pragmatic Engineer 調查整理：https://zyvop.com/three-surveys-three-1-tools-all-of-them-correct-v855p
- Stack Overflow 2025 調查整理：https://sqmagazine.co.uk/best-ai-coding-tools/
- Menlo／Ramp 企業支出二手整理：https://www.mejba.me/blog/anthropic-vs-openai-ai-coding-war-2026

基準：

- SWE-bench Verified 官方榜：https://www.benchleader.com/benchmarks/swe_bench_verified_any
- 同一 harness 對比整理：https://localaimaster.com/models/best-ai-coding-models
- Vals.ai 飽和測試整理：https://yagni.app/blog/best-open-source-llm-for-coding/
- 對照實驗（arXiv 2610.04433）：https://arxiv.org/html/2610.04433v1
- Artificial Analysis Coding Agent Index v2.0：https://enjoy.dev/blog/coding-agent-index-2/
- SWE-Lancer 早期數據：https://www.downcodes.com/wz/238120.html

外殼與產品：

- Claude Code 官方文件（運作方式）：https://code.claude.com/docs/zh-CN/how-claude-code-works.md
- 品質回歸檢討二手整理：https://cloud.tencent.com/developer/article/2659945
- 工具設計典範中文分析：https://cloud.tencent.com/developer/article/2655531

中國繞道：

- DeepSeek 官方 Claude Code 整合文件：https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code/
- Claude Code 中國指南：https://github.com/TimeMachineLab/claude-code-china-guide/blob/main/README.md
- Claude Code Router：https://github.com/destenson/musistudio--claude-code-router/blob/main/README_zh.md
- Claude Code Model Router：https://github.com/luwill/Claude-Code-Model-Router/blob/main/README.zh-CN.md
- Juejin 時間線：https://juejin.cn/post/7523712612534665242
- Qwen Cloud Claude Code 文件：https://docs.qwencloud.com/developer-guides/clients-and-developer-tools/claude-code

對手與反例：

- OpenAI Codex 使用者數整理：https://aiwiki.ai/wiki/openai_codex
- Claude Code 替代品對比：https://www.datacamp.com/blog/claude-code-alternatives
- Vibe-coded 軟體目錄（含 vinext、FastRender、Ladybird）：https://www.vibeloadout.com/vibe-coded-software/index.html
- Gaia 開發故事：https://adamperlis.com/how-i-built-gaia/
