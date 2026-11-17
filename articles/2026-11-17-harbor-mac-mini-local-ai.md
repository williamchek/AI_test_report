# 當 Mac 變成「私人 AI 伺服器」：Harbor 預覽上線，但 16GB 的 Mac mini 能做的事早已存在

一台洩漏的 App、一台只有 Thunderbolt 4 的 Mac mini，和一場關於「等新工具，還是先動手」的拉鋸

2026年11月17日

一名網友在社群平台貼出截圖，說 Apple 旗下 Claris 出了一款叫 Harbor 的 App，能把家裡的 Mac 變成私人 AI 伺服器，資料不上雲，也不按 token 收費。

我買的 Mac mini，為什麼要等一個還沒上市的 App 才能跑本地 AI？

## 從一個洩漏的頁面說起

**洩漏者貼文。** 2026年11月10日約 18:56 UTC，MacRumors 撰稿人、長期爆料者 Aaron Perris（@aaronp613）在 X 貼出 Harbor 的 App Store 文案與截圖，寫道 Apple 正在做一款叫 Harbor 的新 App。這個時間點與貼文內容出自洩漏者本人，9to5Mac 與 MacRumors 同日都以他的貼文為來源跟進報導。

**媒體跟進。** 9to5Mac 同日轉述了整段文案；MacRumors 於美西時間 15:34（23:34 UTC）刊出報導，寫明 App Store 頁面「已經下架」。

**頁面下架。** Perris 約 23:40 UTC 再貼文說 Apple 已經把 Harbor 從 App Store 撤下。從貼文到下架約五小時。到 2026 年 11 月 16 日查核時，Apple 沒有發布官方宣布，也沒有說明下架原因，這部分尚待官方說明。

截圖裡有什麼，來自二手轉述，宜視為參考。GIGAZINE 轉述其中兩張：一張顯示 Harbor 正在下載 MLX 格式的 gpt-oss-20b，另一張顯示 Llama 3.6-2-Instruct-4bit 在一台 M5 MacBook Pro 上執行。Threads 上轉貼的第三張是一張叢集圖：「This machine」是 48GB 的 M5 Max MacBook Pro，另有兩台 M4 Pro Mac mini（16GB 與 24GB），載入的模型是 gpt-oss-20b-MXDKH4-Q4，伺服器顯示運作中，上線時間 33:55:03。這些是行銷截圖經洩漏者轉傳，不是獨立實測，讀不出任何效能數字。

檔名裡的 MLX 字樣與 Harbor 用 Apple 的 MLX 格式一致，但沒有任何媒體明言 Harbor 建在 MLX 之上，這屬於推論，尚待證實。

舊名「Fabric」有兩重佐證。AppleInsider 與 iDrop News 引述 Perris 的說法，指下載路徑仍寫著 `com.claris.fabric`；Claris 頁面自己的標頭圖片，alt 文字到 2026 年 11 月 16 日查核時仍寫著 "Claris Fabric"，這一點已查證。

## 「私人 AI 伺服器」：Claris 頁面說了什麼、沒說什麼

Claris 自己的頁面（content.claris.com/local-ai-server-get-started）在 2026 年 11 月 16 日查核時仍在線上，上面寫著「Private Preview」。已查證的說法只有這些：把你擁有的 Apple silicon 電腦變成私人 AI 伺服器；零雲端費用、無按 token 計費；資料不離開硬體；單機幾分鐘架好端點；用 Thunderbolt 5 池化統一記憶體。頁面上只有註冊表單，欄位是姓名、電郵、公司、電話、國家。

沒說的，比說了的還多。沒有價格、沒有日期、沒有下載、沒有 API 規格、沒有模型清單、沒有基準數據。API 是否相容 OpenAI 或 Anthropic，頁面隻字未提，尚待查證——而這一點決定了 Claude Code、OpenCode、Cline 這類工具能不能接上它。模型格式、叢集上限、macOS 版本、記憶體下限、並行數字，一概未公布。

「零雲端費用」描述的是使用費，卻不等於 App 本身免費。MacRumors 的說法是：Apple 會怎麼賣 Harbor、收不收費，目前無法下定論。

上市時間，兩家媒體的說法正好相反。9to5Mac 認為公開上市在未來幾週到幾個月內似乎合理；AppleInsider 則說一般使用者短期內不該期待。兩種說法都沒有 Claris 的來源，宜視為推測。

有一則單一來源的系統需求說法，本文不採信。cellcog.ai 一家稱 Harbor 需要 M1 以上、macOS 27、16GB 記憶體，這個說法在 Claris 頁面、MacRumors 與 9to5Mac 的報導裡都找不到。

另外要劃清界線：Apple ML Research 在 2026 年 10 月 9 日發表的 arXiv 論文「Minions」，是把閒置 Mac 串成虛擬 GPU 雲的研究計畫，沒有基準、沒有程式碼、不是產品，不宜與 Harbor 混為一談。

## 16GB 的天花板：這台 Mac mini 跑得動什麼

Harbor 的賣點合理，但要放在比例尺上看：它承諾的兩件事，一件是單機好用，一件是多機池化。對一台 16GB 的 M4 Mac mini 來說，第一件早已有人做到，第二件根本輪不到它。

先看硬體天花板。Base M4 是 10 核 CPU、10 核 GPU，記憶體頻寬 120 GB/s，16GB 統一記憶體由 CPU、GPU 與 macOS 共用，不可升級。GPU 實際可用約 11.2 至 11.8 GiB：CanRunLLM 的規劃模型給出約 11.2GB，ComputingForGeeks 在 2026 年 10 月以 Ollama 0.40.6 實測 16GB M4 機器為 11.8 GiB。一顆 13GB 的模型裝不進 GPU 全數，層會溢出到 CPU，速度隨之掉下來。

**小模型跑得動。** Qwen3 8B（Q4_K_M，約 5.2GB）是日常主力；Llama 3.1 8B 約 4.7 至 5GB，估計約 15 tok/s；Gemma 3 12B 約 7 至 8GB，估計約 12 tok/s；Qwen3 14B 約 10.9GB（8K context），估計約 9 tok/s，偏緊；寫程式首選 Qwen2.5-Coder 7B／14B，14B 是 agent 用途的實際上限。速度數字是 CanRunLLM 的校準估計（正負 20%），不是實測，宜視為參考。

**gpt-oss-20b 只是邊緣。** 它約 13GB、總參數 20.9B／啟用 3.6B、MXFP4、原生 context 128K。裝得進，但幾乎塞滿機器，可用 context 小，Ollama 預設路徑速度差。速度說法彼此衝突約七倍：24GB MacBook Air 經 Ollama 約 5 tok/s；LLMCheck 估計 34 tok/s；runitlocal 的計算器直接稱 base M4 16GB「太大」。本文不挑一個數字。誠實的說法只有一句：Ollama 預設路徑先預期個位數，MLX 或 GGUF 版本加小 context 會好一些，承諾之前要在自己的機器上實測。

**30B 級裝不進。** Qwen3-30B-A3B 的 Q4 GGUF 是 18.6GB，加上工作空間約 23GB，16GB 裝不進正常全載推論；Qwen3-Coder-30B 下載 19GB、256K context，agent 用途同樣裝不進，另有 Ollama #17276 的工具呼叫解析問題。網路上流傳的 113 tok/s，出自 mlx-lm 在 M4 Max 64GB 上的基準，不宜套用在 base M4 上。

**Context 才是真正的瓶頸。** Ollama 在 24GiB VRAM 以下預設只有 4K context，而且會默默截斷過長的提示。Agent 用途要設到 32 至 64K；但加大 context 會吃掉 KV 快取，正是 13 至 19GB 模型最缺的記憶體。工具呼叫品質也要打折：本地 8 至 20B 模型在工具呼叫上的可靠度，明顯低於雲端旗艦模型，單檔修改、小腳本做得來，整庫自主重構做不穩。這屬於實務觀察，不是基準數據。

## 「本地」的包裝：Harbor 帶來什麼，現有工具缺什麼

這個推論合理，但要放在比例尺上看：Harbor 帶來的是包裝——圖形介面加模型目錄加自動叢集，不是新能力。

**Ollama 已經是預設的 agent 後端。** MIT 授權，背景伺服器加模型庫，OpenAI 相容端點在 port 11434，也支援 Anthropic 相容的 `/v1/messages`。Claude Code 指向 `http://localhost:11434` 就能接上，舊版本才需要轉譯代理。缺點是預設一次只跑一個模型，以及上面提到的 4K 預設 context。

**LM Studio 常常是圖形介面最快的路。** 桌面 GUI、Hugging Face 搜尋、每顆模型可選 GGUF 或 MLX，OpenAI 相容伺服器在 port 1234。新推出的 Link 功能用 Tailscale 把機器連起來，那是遠端推論，卻不等於記憶體池化。

**llama.cpp 是最能調的引擎。** Ollama 與 LM Studio 底下都是它，GGUF、Metal 後端、OpenAI 相容伺服器，context 溢出時回 HTTP 400，而不是像 Ollama 那樣默默截斷。代價是沒有模型管理，檔案、context 與卸載全要自己選。

**MLX-LM 是 Apple 自家的路。** `mlx_lm.server` 是 OpenAI 相容的 HTTP 伺服器（127.0.0.1:8080），支援工具呼叫與連續批次，Apple WWDC26 第 232 場演講就是用這套接 OpenCode 與 Xcode。同一量化等級在 Apple Silicon 上通常比 llama.cpp／Metal 快，來源說法是快 20% 至 55%，Ollama 的 MLX 後端縮小了差距。要注意伺服器預設輸出上限只有 512 個 token，記得調高 `--max-tokens`。

**vLLM 不是 Mac 工具。** 它是給 NVIDIA／AMD GPU 的生產級 serving 引擎，Apple Silicon 不是一線目標，不宜納入這台機器的規劃。Harbor 也不是 vLLM 的替代品。

**exo 才是 Harbor 真正的對照組。** 開源的 Mac 叢集工具，自動探索、拓樸感知的張量平行、OpenAI／Claude／Ollama 相容 API 在 port 52415。MacRumors 的比較正是這一句：Harbor 看起來像 LM Studio 的簡單介面加上叢集。exo 自述的數字是兩台機器快 1.8 倍、四台快 3.2 倍，RDMA 延遲降低 99%，這些是專案自己的說法，宜視為參考。

## 池化這件事：Thunderbolt 4 的 Mac mini 參加不了

讀者的 Mac mini 是 Thunderbolt 4，三個埠最高 40 Gb/s。Thunderbolt 5 是 M4 Pro 以上才有，雙向 80 Gb/s，視訊 boost 到 120 Gb/s。TB5 裝置插進 TB4 埠，會降速到 TB4。

池化的底層技術是真的：macOS Tahoe 26.2 加入的 RDMA over Thunderbolt 5，把節點間延遲從約 300 微秒降到 3 至 50 微秒，讓 MLX 做分散式推論。但它要求兩端都是 Thunderbolt 5，而且每台機器的 macOS 版本相同；exo 還要求全網狀 TB5 接線。Apple 自己的驗證在頂端：兩台 M5 Ultra Mac Studio、各 512GB，池化成 1TB 跑 816B 參數的模型。

換句話說，洩漏截圖裡的叢集本身就是全 TB5 硬體——M5 Max MacBook Pro 加兩台 M4 Pro mini。這不是偏好，是硬性門檻。要拿到 Harbor 式的池化，讀者得換成 TB5 機器，再買第二台 TB5 Mac，數千美元等級的承諾，換來的功能尚待證實。

## 短影音：ComfyUI 在 16GB 上能剪多長

ComfyUI 本身支援 macOS，但影片擴散模型吃 VRAM 也吃算力，M4 的 10 核 GPU 加 16GB 共享記憶體不是它的形狀。綁住它的有兩件事：文字編碼器比影片模型本身還大，以及每支片子都要算好幾分鐘。

**Wan 2.1 1.3B 是唯一現實可行的選擇。** 480p、2 至 5 秒一支，每支算好幾分鐘。文字編碼器 UMT5-XXL 的 FP8 版約 5.2 至 6.7GB，FP16 版約 10.6 至 11.4GB，整條管線用 FP8 編碼器約 8 至 9GB；FP16 編碼器單獨就幾乎塞滿機器。唯一實測的 M4 mini 數字是 32GB 機型：832×480、81 幀花了 12 分 14 秒。16GB 機型沒有實測數字，時間只會更長，宜視為推估。

**LTX-Video 2B 是第二候選。** 量化後約 6 至 8GB，480p 至 720p 短片做得到，畫質低於 Wan 14B，早期 Mac 社群回報有靜止或損壞的案例。

**出局的名單很長。** Wan 14B 在 64GB M1 Max 上跑 2 秒 480p 花了 1 小時 22 分；HunyuanVideo 13B 需要 45 至 60GB，官方建議 80GB；LTX 13B 約 14 至 18GB FP8，LTX-2 標準版需要 80GB 以上。720p 以上、5 秒以上、14B 級模型，都是雲端 GPU 的工作。

## 台灣讀者能做什麼

**先從單機免費工具起，風險最低。** Ollama 或 MLX-LM Server 當引擎，Qwen3 8B 或 Llama 3.1 8B 當常駐模型，寫程式換 Qwen2.5-Coder 7B／14B；Ollama 記得把 context 設到 32 至 64K，否則長提示會被默默截斷。Agent 工具指到 localhost 就能用：OpenCode、Cline，以及經 Anthropic 相容端點接上的 Claude Code。

**再考慮短影音。** ComfyUI 加 Wan 2.1 1.3B，480p、5 秒以內，用 FP8 文字編碼器，算圖時關掉其他 App；編碼器在 MPS 上出問題時，改丟給 CPU 跑。720p 以上、5 秒以上、14B 級模型，交給雲端 GPU。

**最後才是等 Harbor。** 台灣已在 Claris 註冊表單的國家清單內，可以登記意願；但它會不會上市、賣多少錢、API 長什麼樣子，仍待觀察。預覽會不會開放給台灣，尚待官方說明。

台灣的硬體價錢，先講清楚時間點。M4 mini 上市價 NT$19,900 起（16GB／256GB），16GB／512GB 為 NT$33,900，這是上市定價，不是現價。M4 mini 已於 2026 年 8 月 25 日停產，沒有可靠的台灣現價，本文不報價。2026 年 7 月 Threads 上的二手開價約 NT$21,000 至 23,000，屬未查證的傳聞，宜視為參考。

隱私與成本，也要放在比例尺上看。本地推論把提示、程式碼與文件留在自己的硬體上，這點 Ollama 與 MLX 早已做到；但模型要從 Hugging Face 下載，插件、網頁搜尋與遙測仍有外洩路徑，「本地」不等於「離線」。成本上，讀者已有硬體，邊際成本只剩電費與時間；輕量使用想靠本地省錢，回收期要以年計。雲端價目數字出自 2026 年 6 至 10 月的舊資料，宜視為示意，引用前要重查。

## 觀察：包裝不是能力，門檻才是

這個現象的核心，其實不在 Harbor 本身，而在更老的問題：你的機器夠不夠格參加下一輪本地 AI，還是單機就已夠用？

接下來有三件事值得追蹤：Harbor 是否公開上市、價格與 API 規格為何；Claris 會不會說明 Thunderbolt 4 機器能不能以任何形式參與；本地小模型的工具呼叫品質會不會追上雲端。

至於 Harbor 會不會成為 Mac 本地 AI 的預設入口，還是只是一次提前曝光的預覽頁面，目前無法下定論。

*說明：本文以 2026 年 11 月 16 日的研究報告為事實來源。主軸來源是 Claris Harbor 頁面（查核當日仍在線上）與 MacRumors、9to5Mac 2026 年 11 月 10 日的報導；截圖內容與叢集細節出自洩漏者貼文與二手轉述，未經獨立實測。模型速度數字多是 CanRunLLM 的校準估計或各專案自述，宜視為參考；gpt-oss-20b 的速度說法彼此衝突，本文不採單一數字。價格、上市日期、API 規格與台灣供貨，尚待官方公布。*
