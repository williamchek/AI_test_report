# Rust 重寫買到了什麼、沒買到什麼：ArtCraft 對上原版商業軟體

> 專題｜根據 2026 年 11 月 4 日公開資料整理。不是實測報告：以下數字來自專案文件、獨立測試與二手整理，沒有在登入狀態下驗證，也沒有自己量測；標示「專案自評」「二手轉述」「未驗證」的地方，請不要當成獨立證實的數字。版本變動極快——測試多半用的是 0.9.0，寫稿時 PdfCraft 已到 0.18.1、PhotoCraft 已到 0.20.1。

## 先講結論

「用 Rust 重寫」今天具體買到的東西是：**小體積的原生程式，不用帳號、不用遙測、沒有背景常駐程式、不用訂閱；同一套 Rust 程式碼編出桌面版跟 WebAssembly 網頁版；還有一個讓腳本跟 AI agent 驅動程式的指令／CLI／MCP 引擎。** 這些是從程式碼結構、發行檔案與獨立的封包／行程檢查裡可以證實的。

買不到的東西，今天也一樣具體：Adobe 等級的功能深度、複雜檔案的保真度、生成式 AI、插件生態、雲端協作，以及在大檔案上「真的比較快」的證明。專案自己就是這麼說的：PhotoCraft 約 45% 堪用、PdfCraft 約 50%、FigCraft 約 53%（都是專案自評）。最可信的獨立數字都是針對**日常、單一檔案的工作**（PdfCraft 在 10 項真實 PDF 工作拿 9.5/10；PhotoCraft 在 10 項 Photoshop 式工作拿 7/10，但第二項測試有方法上的保留，見第二節）。一到大檔案、複雜工作，同樣的來源回報的是當機、卡頓、缺解碼器與保真度流失。

**一句話的判斷：Rust 是真的架構，不是行銷話術——但今天使用者看得到的差距，多半是年輕跟未完成，而不是語言造成的；而且有幾個缺口（訓練好的 AI 模型、色彩與字型科學、生態系的網路效應、專有格式的秘密）是換什麼語言都補不起來的。**

## 一、架構：Rust 重寫到底改了什麼

### 「淨室重寫」這個說法，先拆成兩半

每個 app 的 README 都說自己是「從公開規格與觀察到的行為出發、用 Rust 從頭寫的淨室重寫」，沒有 Adobe、微軟、Figma 的程式碼、著色器或素材。創辦人 Brandon Thomas 據報導說他用 Anthropic Claude Opus 5.5 開發，最初七個 app 約 77% 的 commit 把 Claude Opus 5.5 列為共同作者（二手轉述）。

但「淨室」是專案自己的主張，不是法律認定。Frontier Now 指出，反編譯的說法來自一篇微信貼文與留言，Hacker News 的留言說沒有反編譯的證據，也有人質疑 AI 輔助開發到底算不算淨室——爭議沒有結論。CellCog 也做了同樣的區分。截至 10 月中，沒有找到任何一家原廠（Adobe、微軟、Figma、Autodesk、Avid）的回應。

「不是包裝」這一點，在結構上是可以驗證的，不只是主張。PhotoCraft 的貢獻指南禁止 Tauri／Electron／webview 外殼：桌面版是原生 egui／eframe 跑在 wgpu 上，網頁版是同一套 Rust 編成 WebAssembly（trunk + wasm-bindgen）。這跟「包一個現成引擎」正好相反——只有一個公開的例外：PdfCraft 的頁面渲染在當時還是「借用」第三方 Rust 套件 `hayro`，自己的渲染器還在寫。所以「純 Rust」的意思是「Rust 程式碼，包含第三方 Rust 套件」，不是「零依賴」。

### 沒有垃圾回收的記憶體安全、執行緒

Rust 的基本模型——所有權（一個值只有一個擁有者，離開作用域就釋放）、編譯期借用檢查、沒有垃圾回收、沒有 GC 停頓、靠 Send／Sync 在編譯期防止資料競爭——是教科書上跟 C／C++ 手動管理記憶體的對照。這是語言的性質，**不是**任何一個 ArtCraft app 被量測出來的性質。

PhotoCraft 具體怎麼用它（專案文件與二手分析）：

- 純資料的文件模型＋指令引擎，包在一層薄薄的 egui UI 下面，用約 24 個 crate 按依賴層級分開（文件、操作、合成、PSD、輸入輸出、引擎、自動化、UI）。`ui-egui` 以下的層不准用 egui／eframe／winit／rfd。
- 兩個合成器互相對照：多執行緒（Rayon）的 CPU 參考合成器（「oracle」）跟 wgpu GPU 合成器（Metal／Vulkan／DX12／WebGPU）。
- 稀疏的 256×256 寫時複製圖磚：筆刷只改動碰到的圖磚，復原是改指標而不是整張圖複製；效果按圖層狀態快取。
- 每個動作都是登錄檔裡的一條指令（500 多條指令；2026 年 10 月有 631 個選單項目對應到一條可執行的指令），GUI、CLI、JSON 控制通道跟 MCP 伺服器走的是同一條路。

**但這證明不了什麼**：PhotoCraft 自己的對照文件還在回報 GPU 當機、150 層時 GPU 路徑崩潰、文字工具在 macOS 上凍結約 60 秒、無 GPU 機器啟動失敗，以及 fuzzing 在 PdfCraft 找到當機／卡住。記憶體安全消滅的是**幾類** bug（use-after-free、安全程式碼裡的資料競爭），不是邏輯 bug、panic、GPU 驅動失敗或算錯圖。另有二手報導說 PhotoCraft 全專案設了 `unsafe_code = "forbid"`，這次研究沒有從一手檔案確認——當成未確認。

### 一套程式碼，原生＋WebAssembly

同一套 Rust 編出原生桌面版（macOS／Windows／Linux／FreeBSD）跟 WebAssembly 網頁版，可以自己架站、檔案在瀏覽器本地處理。FilmCraft 是最初的 app 裡唯一沒有託管網頁版的；後來的整理說 12 個裡有 11 個、或除了 FilmCraft 以外都有，說法隨日期而變——要斷言某個 app 有沒有網頁版，先去看當下的官方 apps 頁面。

跟原廠對照：Adobe 的桌面 app 是原生、專有、綁帳號跟 Creative Cloud 常駐程式（見第五節）；Figma 是網頁優先加雲端，桌面版是 Electron 包裝。ArtCraft 把 Figma 的模型倒過來（原生優先，瀏覽器只是編譯目標），並把 Adobe 的常駐程式跟帳號層整個拿掉。Office 原廠的技術堆疊這次沒有找到可靠的整理——不要在沒有來源的情況下斷言 Word／Excel／PowerPoint 是 Electron 或 C++。

### 每個 app 各自的引擎，還是共用元件？兩者都有

- **每個 app 有自己的 repo、引擎跟文件模型**（例如 PhotoCraft 的點陣引擎、PdfCraft 的 PDF 引擎／CLI、FilmCraft 用公開規格寫的編解碼與時間軸，整數刻度時間軸讓 23.976／29.97 fps 永遠不漂移）。
- **跨 app 的共用部分也存在**：`storytold/craft-libs`（共用 Rust 函式庫，用不可變的 git revision 鎖定）、`storytold/craft-fonts`（內建字型，包含日文 UI 字型），以及共用的版面模式（每個 app 一個桌面 crate＋一個 `-cli` crate＋一個 `-web` WASM crate）。PhotoCraft 的 AGENTS.md 提到貢獻標準 repo `craftrules` **不公開**——一個值得標示的透明度缺口。
- **一個真正新的架構差異**：引擎從設計上就是無頭優先、可以被 agent 驅動（CLI＋MCP＋JSON），而 Adobe／Figma／Office 的自動化是後來加上去的插件／腳本層。這是「重寫」帶來原廠沒有的新能力，而不是單純重做舊功能——這一點上重寫不是複製，是加法。

## 二、效能：量測過的比較

**先講最重要的事實：除了下面唯一一組 PdfCraft 對 Acrobat 的單次工作測試，沒有任何獨立的、在同一台機器、同一批檔案上，把 ArtCraft app 跟原版商業軟體拿來比開機／記憶體／速度的測試。** 其他的數字不是（a）ArtCraft 對開源工具，就是（b）專案自己的預算，或（c）只有一邊被量測過的體積數字。

### 2.1 唯一一次真正的同場對照：PdfCraft vs Acrobat

來源：一位獨立的安全工程師測試者，無償、AI 配音、方法寫在描述欄：PdfCraft 0.9.0（2026 年 10 月 18 日）對 Acrobat Pro 試用版 26.002.22031，一台 Windows 11 虛擬機，每項工作只跑一次，兩個都用管理員權限跑，結果用 pypdf／qpdf 檢查而不是用眼睛看。分數：**PdfCraft 9.5/10，Acrobat 10/10**。

| 工作／量測 | PdfCraft 0.9.0 | Acrobat Pro |
|---|---|---|
| 標示＋註解；合併 2 檔／5 頁；整理頁面；填表單 | 通過 | 通過 |
| 簽名（打字） | 通過（是圖片，不是數位 ID 簽名） | 通過（一樣的保留） |
| 壓縮 17.8 MB 截圖 | **0.61 MB** | **0.50 MB** |
| PDF 轉 Word | 通過，**不到 1 秒** | 通過，**約 2 分鐘**（時間如測試者所報；品質沒有細比——Word 輸出品質的反向提醒見 3.1 節） |
| 改文字（$1,200 → $950） | 通過（第二次才成功） | 通過 |
| 掃描檔 OCR | **97.6%** 的字 | **98.8%** 的字（兩邊都要事後校對數字） |
| 塗黑：文字 | 通過 | 通過 |
| 塗黑：掃描頁 | **只對一半——整頁圖片被拿掉，存檔只剩 1,297 位元組**（公開 issue storytold/pdfcraft#2246，2026 年 10 月 20 日） | 通過，頁面保留 |
| 安裝體積 | **8 秒、167 MB、0 個服務、0 個排程工作、開機沒有東西** | **靜默安裝約 10 分鐘、約 2.6 GB、1 個更新服務、1 個排程工作、5 個 Office 增益集** |
| 工作時連網 | **0 條連線**（41 分鐘擷取） | **1 小時內約 20 台 Adobe 伺服器**，送出不到 0.5 MB（沒有上傳 18 MB 的檔案） |
| 關掉之後 | 只剩 1 個設定檔＋log | Tray 小幫手 45 分鐘後自己重啟、連了登入伺服器；log 上傳／同步小幫手在 Acrobat 關掉後還連線；使用者資料夾裡 860 多個檔案／約 220 MB |

限制：每項工作只跑一次、一台虛擬機、alpha 版「每幾天就出新版」。PDF 轉 Word「不到 1 秒對約 2 分鐘」的時間差，不要推論到這位測試者的檔案以外。

### 2.2 PhotoCraft 的 10 項工作測試——重新驗證過，但不是對照測試

來源：同一位測試者：PhotoCraft 0.9.0（2026 年 10 月 14 日），Linux，軟體渲染（沒有 GPU），每項工作只跑一次。分數：**7/10（6 項通過、2 項部分、2 項失敗）**。**關鍵保留，測試者自己說的：「Photoshop 沒有跑：每項工作是拿 Adobe 自己的說明文件來對，說 Photoshop 會怎麼做。」** 所以這是按預期 Photoshop 行為打分的能力測試，不是量測出來的 Photoshop 比較。工作內容：裁切／縮放成 Instagram 4:5 通過；粗體即時文字標題通過；曲線調整通過；1928 年老印刷品的污點修復通過；遮罩＋漸層融合通過；人像「選取主體」**失敗**（三張人像都切掉頭跟手臂；素色天空上的老鷹是乾淨的）；移除燈柱**部分**（留下灰色糊痕）；批次縮放＋浮水印 21 張照片**部分**（約 45 秒跑完 21 張，但錄製的縮放壓扁了比例，錄製的文字浮水印靜默寫出 0 個檔案）；分層 PSD 來回**通過**（文字可編輯、效果保留、編輯範圍外約 0.2% 像素改變，10/13 個點陣遮罩記錄掉了但形狀還看得到）；生成式填色**失敗**（沒有對應指令）。

同一支影片裡的 PSD 大規模檢查：用 photocraft-cli 轉全部 316 個 psd-tools 檔案，**嚴格檢查 253/316 完好、只看圖的話 268/316**——測試者明確說這是不同檔案集、更嚴格的檢查，他**沒辦法重現專案說的 307/309**，而且沒有用 Photoshop 檢查。安全性：約 16 分鐘 0 對外連線、預設不開 port，`--control`／MCP 只走 localhost＋token；但儲存／匯出時**保留了植入的 GPS＋相機序號中繼資料**，只有快速匯出 PNG／網頁儲存例外——這是真的隱私陷阱，要提醒使用者。

### 2.3 對開源基準的獨立數字（不是對 Adobe）

CraftSuite，Ubuntu 24.04、4 vCPU、16 GB RAM、**沒有 GPU**，2026 年 10 月 20 日，官方 AppImage：

**PhotoCraft 0.9.0**：

- 開到空白視窗 **0.25 秒**（三次中位數）；開 24 MP JPEG **1.5 秒**；記憶體空白 **214 MB**、開 24 MP **386 MB**（同一張照片 GIMP 約 395 MB——也就是跟 GIMP 打平，不是大勝）。
- CLI 工作（24 MP 開檔→半徑 10 模糊→縮 50%→存 JPEG）：**2.1 秒，對 GIMP 6.7 秒、ImageMagick 6.5 秒**；JPEG 轉 PNG 2.9 秒對 ImageMagick 9.9 秒。（每個工具的模糊參數不同——是最接近的等價操作，不是完全相同的操作。）
- PSD：**315/316 打得開、257 張跟 Photoshop 存的合成圖差異不到 1%**；GIMP 是 243 張打得開／103 張吻合。CLI 轉檔中位數每檔 43 毫秒。
- 濾鏡藝廊預覽在 768×512、無 GPU 下 78–84 毫秒；液化每筆刷 0.71 毫秒（回報值）。

**PdfCraft 0.9.0**：

- 117 頁教科書開檔 **0.18 秒、223 MB RAM**。
- 117 頁轉 150 dpi PNG：**10.5 秒，對 MuPDF 9.0 秒、Poppler 34.1 秒**（「跟 MuPDF 同級，比 Poppler 快 3 倍以上」）。
- 合併 10 份（1,170 頁）：**1.7 秒→6.6 MB，對 qpdf 0.4 秒→6.6 MB、MuPDF 0.5 秒→54.1 MB**（PdfCraft 像 qpdf 一樣會去重字型跟圖片）。
- 壓縮 5.3 MB 教科書：**0.8 秒→2.0 MB（−63%），30,168 個字都還抽得出來，對 Ghostscript／ebook 2.0 秒→1.5 MB（−72%）**；單張圖 2.3 MB：PdfCraft 0.1 秒→1.8 MB（−21%，無失真保留）對 Ghostscript 0.1 秒→0.05 MB（重編成失真 JPEG）。
- 渲染品質對 MuPDF 基準：PdfCraft 平均差 2.9%、Poppler 6.0%（主要是反鋸齒）——但圖案填充（剖面線）畫錯了。
- 單頁 OCR：PdfCraft **87.7%** 對 Tesseract 5.3.4 **84.9%**（英文）；德文教科書的公式跟圖表認不出來。

### 2.4 專案自己的效能證據——以及它自己的承認

PhotoCraft 對照文件（2026 年 10 月 10 日重測，對 Photoshop 2026 27.11.0）：**效能就緒度 25%；25 個預算場景裡只有 3 個達標**（v0.10.0 發行說明也一樣）；15000×10000、16-bit 圖層移動中位數 2.7 秒；150 層微調會讓 GPU 路徑當機；筆刷／游標延遲有回報。VectorCraft「20,000 個形狀在 retina 下約 27 毫秒」是專案量測、經 Frontier Now 引用，不是獨立測試。**沒有找到任何專案或測試者做的大檔案對照測試**（例如多 GB 的 PSD、1,000 頁 PDF、4K 時間軸）——那一格是空的，不是對 ArtCraft 有利。

### 2.5 不要當成量測來引用的數字

- 一支「重量測試」影片（PhotoCraft 0.16.0、M1 Mac）：下載 99 MB、安裝 202 MB、啟動 82 MB、開 24 MP 是 873 MB、再加 3 層是 1.4 GB；Adobe 26 個行程約 2.5 GB、關掉後還剩 18 個約 287 MB——**但影片自己說 Photoshop 的數字是 Adobe 公布的範圍／常見說法，「測試用的 Mac 上沒有裝 Photoshop」**。只有 PhotoCraft 那邊是量測的。
- 粉絲／行銷頁面說 PhotoCraft「閒置不到 200 毫秒／40–42 MB、冷啟動不到 50 毫秒、8K 60+ FPS」（例如 photocraft.si、artcrafthub.org 說 0.42 秒對 Photoshop 8.4 秒、在「相同工作站」上，但沒有機器跟方法），**跟 CraftSuite 量到的空白 214 MB／0.25 秒矛盾**，也沒有可重現的方法。當成行銷，不是資料。中文報導（iplaysoft、eogee）重複「效能非常高／降維打擊」但沒有任何測試，也一樣。
- Wilson／TechRadar 的安裝體積（PhotoCraft 80 MB 對 Photoshop 6.4 GB、FilmCraft 90 MB 對 Premiere 6.8 GB，舊款 Intel Mac）是真實觀察、但來自約 v0.2 的版本，而且那是**安裝體積，不是執行效能**。

## 三、功能：現在有什麼、缺什麼，測試者怎麼說

### 3.1 PdfCraft vs Acrobat——深入

**專案自評**（對 Acrobat DC continuous 26.002.21931，macOS）：功能廣度加權約 67%、不加權 57.0%（808 項裡 423 項已出＋75 項部分），必備（P0）94.2%；實際堪用完整版約 50%（45–53%）、主流使用者約 55%、基本需求約 66%。範圍是**離線版 Acrobat 而已**——23 項雲端功能（Document Cloud、Acrobat Sign 請求、共享審閱、AI Assistant、Express、Adobe 核准信任清單）被標成 `na`，從每個百分比裡排除。剩下到 beta 約 400–700 個 agent 工時，到完全對等約 780–1,400。

**現在有的（專案跟測試者一致）**：瀏覽／搜尋（包含閱讀順序選字、直欄／RTL／CJK）、整理頁面（旋轉／刪除／插入／重排／擷取、Bates 編號、裁切／頁面框）、合併／分割（每檔書籤保留）、多數註解、AcroForm 填寫＋製作（沙箱 JavaScript）、填寫與簽名（打字／手繪／圖片／掃描簽名）、密碼 40-bit RC4 到 AES-256、真正的塗黑（已驗證：CraftSuite 的稽核先找到一次假塗黑，真的塗黑後 `pdftotext`＋位元組搜尋完全找不到那組數字）、清理文件、基本數位簽章、列印、無障礙檢查、離線 OCR、壓縮、CLI＋MCP、當機復原／自動儲存、修復損壞檔案（專案說 983 個 pdf.js 檔案 0 當機；CraftSuite：35 個 pypdf 樣本 34 個直接打開，1 個有密碼的用公開密碼打開）。

**測試者 verdict**：

- 10 項工作測試：9.5/10（2.1 節）——整個家族裡最強的成績。
- How-To Geek，v0.9.0，三份自製 PDF（發票、工作說明書、30-60-90 計畫）：改現有文字（名字／項目／費率，存檔重開後格式完整）、標示＋註解保留、整理頁面、合併（編輯＋註解都保留）、填寫與簽名、壓縮 277→115 KB（−59%）全部通過；**PDF 轉 Word 只有部分**（表格底色／框線掉了、項目符號消失）；他那次沒有當機，但 GitHub 上有列印當機、複雜頁面渲染慢、希伯來／阿拉伯文變方塊＋RTL 問題、段落編號改到錯的段落、多欄匯出順序錯誤的回報。
- XDA（2026 年 10 月 16 日）：「功能相當多」、號稱 80 多種工具、日常工作「出乎意料地順」，轉檔「還在進行中，特別是複雜格式／版面」，「還沒有 Acrobat 那麼精緻」。
- CraftSuite：圖案填充渲染錯誤是唯一明確的渲染缺陷；有些工具還掛著未來的里程碑標籤（M7／M11／M12，例如附加檔案）還不能用；按 Esc 會丟掉文字編輯（要點外面才提交）；OCR 認不出公式。

**缺的／薄的（專案＋測試者）**：可靠地改現有文字（特別是 CJK／RTL、字型替換）；拉丁字母以外的 OCR；Office 匯入／匯出跟 Office 轉 PDF（Word 匯出會掉版面）；PDF／A／X／UA 預檢；XFA 表單（完整版）；簽章 token／硬體 token 簽名＋線上長期驗證；印刷生產；網頁版列印＋實體印表機保真度；Adobe 雲端＋AI Assistant（「沒有淨室等價物」——專案原話）。早期版本（0.4／0.5）甚至產出 Acrobat 打不開的加密檔案（#774，已在 #836 修復）——提醒你「在 PdfCraft 打得開」不等於「在 Acrobat 打得開」。

### 3.2 PhotoCraft vs Photoshop——深入

**專案自評**（對 Photoshop 2026 27.11.0）：廣度約 76%（選單 631/631 都有對應指令＝接線 100%，工具 59/68、面板 30/35、格式按使用加權約 80%、AI 約 15%、生態約 20%）；實際堪用約 45%（40–50%）、主流使用者約 43%、基本需求約 61%。文件自己警告，10 月 3 日「已經完成 82%」的估計「太樂觀了」，而且首次發行以來已經開了約 1,230 個 issue（10 月 10 日還有 579 個未關），多半是針對已經算「有」的功能。剩下：到 beta 約 1,100–1,800 個 agent 工時，到完全對等約 2,300–4,100。

**現在有的**：67 種工具（XDA 數的；專案算 59/68 種工具箱工具＋631 條選單指令）、圖層／遮罩／群組、調整圖層（16 種，順序跟 Photoshop 一樣，CraftSuite 認證）、圖層樣式（陰影／光暈／斜角／緞面／描邊／覆蓋，可編輯）、智慧型物件＋智慧型濾鏡、文字（點／段落、畫布上、路徑上部分）、形狀／鋼筆／布林路徑、筆刷、濾鏡藝廊（47 種效果）、液化、曲線、選取主體／快速／物件選取＋選取並遮住（傳統、本地）、Camera Raw（部分）、PSD／PSB＋TIFF／PNG／JPEG／WebP 等、`.atn` 動作載入／儲存（112 個事件）、預設集（`.abr`／`.grd`／`.pat`／`.aco`／`.ase`／`.cube`）、CLI／MCP／JSON 控制。

**測試者 verdict**：

- 10 項工作測試：7/10，但有 Photoshop 沒跑的保留（2.2 節）。
- CraftSuite（2.3 節）：PSD 渲染對 GIMP 最強，但選取主體漏掉暗部（黑色領口／頭盔變成透明洞）、缺字型時替換（Arial）、raw 很弱（Pentax PEF 只剩內嵌 JPEG 預覽；失真 DNG 不支援；Canon CRW 認不出來）、全新 Ubuntu 上缺 Linux 函式庫＋跳 GPU 備援提示、曲線面板預設太小。
- XDA：UI「極度相似」，移除工具留下「模糊的髒污塊」，對比 Photoshop 的生成式重建；沒有生成式填色／擴展；進階專業功能「還在進行中」。
- Wilson／TechRadar 早期版本（約 v0.2，Intel Mac）：HEIC 打不開、橢圓選取抓不住、拖曳只動外框、放開滑鼠才動，**存檔只有 JPG、沒有格式選項**。**這段不要不加保留地往下傳**：後來的證據（CraftSuite、10 項測試、專案文件）顯示到 v0.9.0 分層 PSD 儲存已經能用了——這是版本漂移，也是每個 verdict 都要標日期的原因。
- McHenry's Lair（v0.7.0，第一印象、不是系統測試）：傳統工作「大概跟舊版 Photoshop 差不多，大概是 Photoshop 2020 的水準」；「對業餘愛好者跟某些傳統工作有意思，但還不是要求高的工作流程的真正替代品」。單一測試者的意見。
- Korben 的 12 款測試：他自己的 7 層 logo PSD 全部圖層都載入、跟 Photoshop 的平面合成圖 100% 像素吻合，重存的 PSD 保留 7 層、重開一模一樣——有利的單一檔案軼事，不是語料庫結果。

**缺的／薄的（專案）**：生成式填色／擴展、神經濾鏡、天空取代、超級變焦（AI 約 10%）；完全沒有 ML 執行環境（計分卡原話）；沒有 `.8BF` 插件、沒有 UXP／ExtendScript、沒有 Libraries（生態約 15–20%）；10 月統計有 14 種工具缺失（消失點／內容感知填色只有對話框、沒有工作區等）；沒有 Photoshop PDF／EPS／JPEG 2000／JPEG XL／AVIF 讀取（到 v0.20.1 還是沒有）；CR3 raw；HDR／EDR 輸出；Windows 上的筆壓傾斜／旋轉、Wayland 筆壞掉；螢幕色彩描述檔自動偵測只有 macOS；11/183 個偏好設定沒有作用（11 月 1 日計分卡）；沒有浮動面板群組；影片；在地化（印地／阿拉伯文沒有、沒有 RTL UI）。

### 3.3 FigCraft vs Figma——第三個 early alpha

**專案估計約 53% 堪用、主流使用者約 50%**（二手整理）。有的：熟悉的工具列／圖層／設計面板、自動版面（水平／垂直、換行、網格、Hug／Fill／Fixed、限制、格線）、元件＋變體（Primary／Ghost），覆寫在編輯後還留著、樣式＋變數與模式（淺色／深色）、向量網路／鋼筆／布林／可變筆觸、原型（連結、Smart Animate 風格、變數／條件、流程）、開發模式（CSS／HTML／React／Vue／SwiftUI／Compose 輸出）、400 多條指令、212 個 Figma 選單項目全部有對應（2026 年 10 月 17 日自動化測試）。XDA 實測（2026 年 11 月 1 日）：「我實際用的東西都有了」，自動版面「感覺跟 Figma 一樣深」（評測者承認自動版面不是他的強項——意見，不是量測）。**缺的**：`.fig` 匯入只有盡力而為、「從來沒有在別人做的真實檔案上測過」，存回 `.fig` 還是實驗性；協作只有預覽版（對比 Figma 即時多人協作的核心）；Figma 社群插件跑不了（自己的沙箱 WASM 插件系統、沒有目錄）；AI 功能 0%；FigJam／Slides／Sites／Make 不在範圍。Verdict：三個 alpha 裡，對照目標的**編輯器**表面覆蓋最廣，但目標的**定義性功能**（即時協作）最弱。

### 3.4 Adobe 另外七款，簡述（除了上面三款，10 月時都是「開發中」；比的主要是缺口）

- **VectorCraft vs Illustrator**：鋼筆、路徑管理員、漸層網格、封套扭曲、透視格線、影像描圖都有人宣稱；PDF 相容的 `.ai` 匯入也有人宣稱；專案量測 20,000 個形狀約 27 毫秒（二手轉述）。沒有找到獨立的 Illustrator 對照測試。缺口（早期專案筆記）：3D／材質、點陣效果藝廊、直式 CJK、變數／腳本、修飾鍵保真度。
- **FilmCraft vs Premiere Pro**：專案檢查清單約 87%，但堪用約 50–60%（二手轉述）；H.264／HEVC／ProRes／AAC 編解碼來自公開規格、整數刻度時間軸；缺口：輸出只有 H.264（沒有 HEVC／AV1 輸出）、沒有 VST3／OpenFX 託管、Windows／Linux 測試較少、早期沒有託管網頁版。測試者（Wilson，早期版本）：啟動快、時間軸熟悉，但不能手動拖曳修剪、舊 Intel Mac 上「慢得離譜」、剪輯音訊是很大的雜音（編解碼缺口）。
- **LightCraft vs Lightroom**：滑桿、色調曲線、色彩分級、預設、非破壞編輯、RAW、遮罩、鏡頭校正、HDR／全景、批次（XDA 測試者）。缺口：色彩——DNG 用自己的矩陣，其他 raw 退回中性矩陣（顏色偏淡）、CR3 跟某些壓縮 raw 只顯示內嵌預覽（二手轉述）；跟 Lightroom 並排看 raw「看起來完全不同」（Hacker News 攝影師軼事）；舊款 Panasonic RAW 問題＋預設顏色差異（XDA 測試者）；AI 遮罩要另外下載 Meta SAM 3 模型（不是內建）。
- **EffectCraft vs After Effects**：早期 README 沒有可以查核的進度數字（Frontier 二手整理）；還不能開 `.aep` 專案（CraftSuite 整理）。比的只有缺口。
- **DesignCraft vs InDesign**：後來的筆記直接說還在 pre-alpha；IDML 可讀可寫（CraftSuite 二手整理）；沒有可以查核的早期數字。比的只有缺口。

### 3.5 Office／CAD／音訊（Adobe 以外）

- **WordCraft vs Word**：功能區、樣式、表格、追蹤修訂、參照、合併列印；可讀可寫 `.docx`，也開得了舊 `.doc`（二手整理）。專案路線圖說法：493/508 個選單／工具列按鈕已接線（97%），但只有約 60% 堪用（二手轉述）。TechCommune（Ubuntu，v0.5.0）：只驗證了一個很小的 Markdown→DOCX→PDF 樣本轉換。Office Watch（二手轉述）說自己的 Microsoft 365 DOCX／XLSX／PPTX 檔案大多開得了、改得了，只有某些試算表函數有問題——二手、方法沒有在這裡重現。
- **GridCraft vs Excel**：`.xlsx` 當原生格式、500 多個函數、動態陣列、樞紐分析表、圖表（號稱 17 種）（二手轉述）。複雜檔案、巨集／VBA、Power Query 跟外部資料的保真度：**沒有找到證據**——先假設沒有／未驗證。
- **DeckCraft vs PowerPoint**：`.pptx` 可讀可寫、母片／主題、150 多種形狀、轉場（含 Morph）、動畫、講者檢視（中二手轉述）。TechCommune v0.5.0：樣本投影片存成 37 KB `.pptx`、轉 PDF、全部投影片都打開——只有樣本證據。
- **CADCraft vs AutoCAD**：DXF R12–2018 可讀可寫（文字＋二進位，寫入 2000–2018），DWG R13–2018 可**開**——但是經 `acadrust` 函式庫把 DWG 轉 DXF（代理物件、ACIS 資料、自訂物件會掉，掉多少沒量過），DWG **存檔**是經 R2000 DXF 轉換；存出的 DWG「把沒建模的東西丟掉，而且從來沒有在 AutoCAD 裡驗證過」；沒有用 AutoCAD 產的檔案當測試樣本；pre-alpha，5 項核心工作流程裡 4 項（精確繪圖、圖塊、出圖、無損 DWG 存檔）沒過 alpha 門檻（專案文件的二手整理）。這是整個家族裡「格式支援不等於保真度」最清楚的例子。
- **SoundCraft vs Pro Tools 風格 DAW**：多軌錄音／編輯／混音＋MIDI、路由／自動化都有人宣稱（二手轉述）。沒有找到獨立 DAW 測試；插件格式（AAX／VST）、硬體、延遲：沒有證據。
- **TextureCraft／MaterialCraft vs Substance Painter／Designer 風格工具**：最新（TextureCraft v0.3.0、MaterialCraft v0.1.0，2026 年 11 月初首次發行，二手轉述）：只有宣稱——3D 模型上的分層 PBR 塗裝、烘焙網格貼圖、貼圖匯出；節點式材質設計器，材質檔可攜到 TextureCraft；18 種筆刷預設、字型當筆刷／貼紙（發行說明，明確寫「沒測過」）。**沒有找到動手測試——只當成發表等級看待。**

### 3.6 Craft Anything——不是複製品

2026 年 11 月 1 日推出（二手轉述）：指著一個網站／已安裝的 app／遊戲／檔案，它會檢查（314 個 Rust 檢查工具），再由 Claude Code（用你自己的帳號／方案）用你選的語言／引擎（包含 Unity／Unreal／Godot）做一個新版本。「進階／深度」模式可以反編譯程式、重用你已安裝副本裡的原美術／音效／地圖，只限私人使用、標明不能分享。這跟 Crafting Apps 的淨室說法**有張力**：Craft Anything 明確是一個*重製引擎*，不是淨室重寫，ArtCraft 把責任推給使用者（「只能是你擁有或被允許檢查的軟體」）。不要把它的手法跟 PhotoCraft／PdfCraft 說的方法混為一談。

## 四、相容性：什麼會壞

| 格式／領域 | ArtCraft 的位置 | 壞掉的地方／邊界案例（有來源） |
|---|---|---|
| PDF | PdfCraft：完整工作台；983 個 pdf.js 檔案 0 當機（專案）；35 個 pypdf 樣本 34 個（測試者） | 圖案填充／剖面線畫錯（測試者）；TikZ 圖案 issue #794（專案）；CJK／RTL 文字編輯＋字形方塊；掃描頁塗黑會毀掉頁面圖片（測試者 10 項測試）；Word 匯出掉版面／表格／項目符號（測試者）；XFA 不完整；沒有 PDF／A／X／UA 預檢；沒有 Office 轉 PDF |
| PSD／PSB | PhotoCraft：家族裡測得最透的格式；專案說重存渲染 307/309，獨立測試嚴格檢查 253–257/316 | 智慧型濾鏡重算 5/30（專案）；Photoshop 拒收某些匯出（#1281、#2469，專案）；缺字型時字型替換（測試者）；溶解混合＋某些實色填滿圖層有差（測試者）；點陣遮罩記錄掉了（形狀還看得到，測試者）；沒有「最大化相容性」的檔案存的預覽是空白的，會混淆比較（測試者） |
| AI（Illustrator） | VectorCraft：只有 PDF 相容的 `.ai`（依整理） | 原生 `.ai`（非 PDF）保真度：沒有證據；CMYK／印刷、漸層網格深度：沒有跟 Illustrator 對照測試 |
| INDD／IDML | DesignCraft：IDML 可讀可寫（宣稱） | INDD 原生：沒有證據；pre-alpha |
| AEP | EffectCraft：還不能開 `.aep` | — |
| DOCX／XLSX／PPTX | Word／Grid／Deck 可讀可寫（宣稱）；GridCraft 以 `.xlsx` 當原生格式 | 複雜檔案「可能會有差異，先用副本測試」（二手轉述）；巨集、複雜格式、試算表函數：部分（Office Watch 二手轉述）；沒有系統性的語料庫測試 |
| DWG／DXF | CADCraft：DXF 穩定；DWG 經轉換（見 3.5 節） | 代理／ACIS／自訂物件掉了；存出的 DWG 沒在 AutoCAD 驗證過；DWG R13／R14 是函式庫宣稱、沒測過 |
| FIG | FigCraft：`.fig` 盡力匯入、實驗性匯出 | 從來沒有在別人的真實檔案上測過（專案經指南轉述）；表格儲存格、影片填滿排除；連接器標籤部分 |
| 字型 | 經 craft-fonts 內建字型；有的地方用系統字型 | 缺字型→替換，存檔前不要蓋掉原檔（測試者）；CJK UI 字形方塊（#826）、CJK 文件問題（#758）、希伯來／阿拉伯文方塊／RTL（測試者）；直式 CJK 排版缺失（VectorCraft） |
| 色彩管理 | PhotoCraft：RGB／灰階／CMYK／Lab 8／16／32-bit、ICC（宣稱）；LightCraft DNG 矩陣 vs 中性備援 | 印刷關鍵色彩「還不行」（測試者）；螢幕描述檔自動偵測只有 macOS；沒有 HDR／EDR（專案）；LightCraft 跟 Lightroom 顏色肉眼可見不同（二手軼事＋測試者預設差異） |
| 圖層／效果 | PSD 圖層／樣式／遮罩／調整多半可編輯（測試者） | 智慧型物件／濾鏡部分；圖層效果在語料庫裡算得不錯，但智慧型濾鏡重算很差（專案） |
| 腳本／插件 | 見第五節 | 沒有 `.8BF`／UXP／ExtendScript／Libraries（PhotoCraft）；沒有 Figma 插件（FigCraft）；PdfCraft 只有沙箱表單 JS，而且測試用的 PDF JavaScript 在 PdfCraft 裡**沒有跑**，在 Acrobat 裡一開就跑（10 項測試）——既是相容性缺口，也是安全性差異 |
| GPU 加速 | wgpu（Metal／Vulkan／DX12／WebGPU）＋CPU 備援 | GPU 當機、150 層崩潰、無 GPU 啟動失敗、CPU 備援在縮放／濾鏡預覽比較慢（專案＋測試者）；上面的軟體渲染測試都是只用 CPU，所以 GPU 速度宣稱在這裡沒有測過 |
| AI 功能 | PhotoCraft 只有傳統本地選取主體／移除；PdfCraft 的 AI 關著、等自己的供應商；LightCraft SAM 3 另外下載；FigCraft AI 是 0% | 沒有 Firefly／Sensei 等價物：沒有生成式填色／擴展、神經濾鏡、天空取代、超級變焦、Acrobat AI Assistant、Figma AI（專案）；Acrobat AI Assistant 在原廠預設是開的，一份 3.8 KB 合約摘要送了 31 KB 上去（測試者）——ArtCraft 沒有這種服務可以比較，這既是隱私勝利，也是功能缺席 |
| 雲端協作 | 沒有當核心功能；FigCraft 多人是預覽版 | 沒有 Document Cloud、共享審閱、Acrobat Sign 請求、Figma 即時共同編輯、Office 共同撰寫的等價物。PdfCraft 的對照文件明確把 23 項雲端功能排除在百分比之外——所以「50% 堪用」是離線版 Acrobat 的 50%，不是市售 Acrobat 的 50% |

**測試者反覆出現的通則**：日常檔案都過；壞掉的集中在複雜／邊界檔案（多欄、CJK／RTL、圖案／技術圖、智慧型物件、DWG 內部、別人做的真實 `.fig`）。每個推薦某個 app 的測試者也都說「交給客戶前先檢查」／「用副本工作」。

## 五、功能以外的日常差別

| 面向 | ArtCraft | 原廠（有來源的對照） |
|---|---|---|
| 價格／授權 | 免費；雙授權 MIT 或 Apache-2.0（有些 GitHub 後設資料只寫 Apache-2.0；README 寫 MIT 或 Apache-2.0）；商業使用可以。**例外**：ArtCraft 的名字／logo 在 `docs/brand/` 裡是商標，不是開源（PdfCraft README）。另外有一個付費的 ArtCraft AI studio（TechPilot 說每月 10–60 美元，2026 年 11 月）——不要跟免費的 Craft apps 搞混。 | Acrobat Pro 每月 19.99 美元（年繳月付）或年繳 239.88 美元；Photoshop 每月 22.99 美元；Creative Cloud Pro 每月 69.99 美元；七個單買加起來每月約 146.93 美元（Frontier／OrcaRouter，Adobe 頁面 2026 年 10 月；10 項測試者引 adobe.com 2026 年 10 月佐證）。訂閱一停就不能用（創辦人自己說的痛點：取消費用，XDA 測試者轉述）。 |
| 帳號／遙測／離線 | 不用帳號、沒有遙測、可以離線、文件不離開機器（每個 README 都這樣寫）。獨立佐證（測過的版本）：PdfCraft 41 分鐘 0 連線、PhotoCraft 約 16 分鐘 0 對外連線（10 項測試影片）；PdfCraft 0 服務／工作／開機項目，對 Acrobat 的服務＋工作＋同步小幫手（測試者）。**保留**：PhotoCraft 的中繼資料外洩（2.2 節），以及 PdfCraft 會把 Windows 使用者名稱寫進註解（對 Acrobat 寫 Adobe 帳號名稱）（測試者）——「沒有遙測」不等於「檔案裡沒有中繼資料」。MCP／控制 port 是自選開啟，只走 localhost＋token（測試者）。 | 要 Adobe 帳號；Creative Cloud＋Acrobat 背景服務、排程工作、關掉後還會重連的 tray 小幫手；AI Assistant 預設開啟；測試者在 Acrobat 偏好設定裡找不到用量資料開關（要去線上帳號設定）（10 項測試）。Figma：要帳號＋雲端儲存、訂閱才有的功能（XDA FigCraft 測試者）。 |
| 平台 | Windows（x64／x86／ARM64）、macOS 通用版、Linux（AppImage／deb／rpm／Flatpak／tarball）、FreeBSD tarball、網頁（WASM）（CraftSuite 下載清單，例如 PdfCraft 還有 RISC-V Linux 版）。PhotoCraft 也有獨立註記的 FreeBSD port（Hofstede 二手整理）。 | Photoshop／Acrobat：只有 Windows＋macOS（CraftSuite 對照表）；Figma：瀏覽器＋桌面版，沒有原生 Linux（beta 回饋裡提到，來源較弱，當成約略）；AutoCAD／Pro Tools 的平台限制這裡沒有來源，不要斷言。 |
| 更新方式（Launcher） | ArtCraft Launcher（11 月 1 日 v0.12.0，到 Update 13 時 v0.14.0，二手轉述）裝全部 app、挑作業系統／CPU 版本、驗 SHA-256（`SHA256SUMS.txt`）、常駐 tray／選單列通知更新；一鍵全部更新；可走 proxy、可以從本地 repo 安裝而不是 GitHub；macOS 只裝有簽名的 app（二手整理）。Launcher 自己還是 early alpha；每個 app 也有獨立安裝程式。發行節奏很快：幾乎每天出新版，有時一天多個——好處是修得快，代價是 verdict 幾天就過期。 | Creative Cloud 桌面 app／Adobe 更新服務＋排程工作；Figma／Office 經帳號綁定的安裝程式自動更新。沒有找到更新速度的對照量測。 |
| 插件／腳本生態 | **新的、不相容的生態**：PhotoCraft WASM 插件（自己的系統）＋`.atn`（112 個事件）＋CLI／MCP／JSON；FigCraft 自己的沙箱 WASM 插件；PdfCraft CLI／MCP＋沙箱表單 JS；每個 app 從設計上都有 CLI＋MCP 伺服器（專案）。沒有找到目錄／社群市集的等價物。 | Photoshop `.8BF`＋UXP＋ExtendScript＋Libraries＋Adobe Stock／Fonts 生態；Figma 社群插件／檔案；Acrobat 插件（對照文件裡列的無障礙、數位簽章等）；Office 增益集／VBA；AutoCAD ObjectARX／LISP；Pro Tools AAX。這些在 ArtCraft 裡都跑不了——是 AI 之後最大的鎖定缺口。 |
| 熟悉度（UX 護城河） | 刻意複製選單／快捷鍵／面板（「會用 Photoshop 就會用 PhotoCraft」，專案原話）——測試者反覆確認（XDA、CraftSuite、How-To Geek、FigCraft 的 XDA）。這是真的降低轉換成本，也是沒有解決的法律／外觀問題（見 1.1 節）。 | — |
| 支援／責任 | 社群 GitHub issues（PhotoCraft 10 月 10 日約 579 個未關；全部 app 在 Update 13 時 12,499 個未關 issue＋8,526 個未關 PR＋358 個失敗檢查，二手轉述）；沒有廠商 SLA、沒有支援合約。寬鬆授權允許專案停擺時 fork——標準的反方論點，Frontier Now 有提。 | 廠商支援、企業部署、認證輸出（例如 PDF／A 驗證、印刷生產）——ArtCraft 沒有。 |

## 六、Rust 解決不了什麼

**A. 年輕／未完成（換什麼語言都一樣，而且是差距的大頭）**：缺工具／對話框／工作區、算「有」的功能深度不夠（PhotoCraft 未關 issue 裡約 244 個在核心路徑上，專案數據）、核心路徑的 bug（選取抓不住、修剪拖曳、早期版本的音訊雜音）、沒測過的邊界案例、沒有跟 Acrobat 做系統性的並排渲染語料庫、DWG 從沒在 AutoCAD 驗證過、`.fig` 從沒在別人做的真實檔案上測過。這些會隨時間跟 agent 工時縮小（專案估計：PhotoCraft 到 beta 要 1,100–1,800 個 agent 工時；PdfCraft 400–700），不是換語言會解決的。

**B. Rust 一個人補不起來的東西**：

- **訓練好的 AI 模型跟它的資料／算力**：Firefly（用 Adobe Stock＋公開授權內容訓練，Adobe 新聞資料）、Sensei／神經濾鏡、Acrobat AI Assistant、Figma AI。PhotoCraft「完全沒有 ML 執行環境」（專案原話）；一個 Rust 執行檔變不出模型、訓練集或推論堆疊——LightCraft 的 SAM 3 是*另外下載*的 Meta 模型，正好說明這一點。
- **色彩科學、字型塑形、raw 色彩**：ICC／CMYK／Lab 正確性、每台相機的 DNG 矩陣、harfbuzz 等級的文字排版、CJK／RTL 組版——這些是幾十年的量測、描述檔與規格邊界案例。Rust 套件存在，但*知識*（相機矩陣、字型行為、印刷標準）要重新推導或授權。LightCraft 的中性矩陣備援跟 PhotoCraft 只有 macOS 的描述檔偵測就是症狀。
- **GPU 運算的成熟度**：wgpu 是 Metal／Vulkan／DX12／WebGPU 之上真實的抽象層，但 Adobe 的 Mercury Graphics Engine、Premiere 的硬體編解碼路徑、GPU 加速效果是調過的管線＋驅動程式 workaround。ArtCraft 的 GPU 當機、CPU 備援跟 3/25 個效能預算，說明抽象層本身不是成就。
- **專有格式的秘密跟生態系的網路效應**：PSD／AI／DWG／.fig 的內部是從公開規格＋觀察行為反推的；有些東西（Acrobat 的 JavaScript 引擎、Photoshop 的插件 SDK、Figma 的多人協作 CRDT、Office 協作、插件目錄、字型庫、圖庫）是*服務／生態系*，不是檔案格式。沒有語言能重寫一個市集。
- **驗證的規模**：Adobe／Figma／微軟有數百萬個生產檔案跟企業 QA；ArtCraft 有語料庫測試（psd-tools 316 個、pdf.js 983 個）跟幾千個 GitHub issue——有價值，但不等於十年的客戶檔案。Rust 編譯器抓得到記憶體 bug；抓不到「Photoshop 這個混合模式算得不一樣」——只有 oracle 語料庫抓得到，而 PhotoCraft 的 oracle 自己在 Photoshop 做的檔案上也只有 133/256。
- **法律／外觀風險**：複製選單、快捷鍵跟版面就是產品策略；能不能挺過原廠挑戰是法律問題，Rust 回答不了，而且 Craft Anything 的反編譯重用模式（限私人使用）讓整個家族的法律姿態比 Crafting Apps 單獨的淨室主張更不統一。

**C. Rust 主動換掉的東西**：編譯期嚴格會拖慢迭代（這裡靠 AI agent 寫程式碼來緩解）；egui 立即模式 UI 很好寫，但測試者說感覺不原生（早期 PhotoCraft 沒有 macOS 選單列、沒有浮動面板群組、面板太薄）；WASM 版本有記憶體上限（PhotoCraft PSD 匯入預算原生 2 GiB／瀏覽器 1 GiB，二手格式指南）——瀏覽器版在大檔案上不是桌面版。

## 七、總表與 verdict

### 差異總表（證據強度：● 強＝獨立測試或有方法的專案文件；◐ 中＝有說明方法的專案自評；○ 弱＝單一軼事、二手整理或行銷）

| 面向 | ArtCraft vs 原廠 | 證據強度 |
|---|---|---|
| 開機／安裝體積 | 小得多、開窗快得多（PhotoCraft 空白 0.25 秒、下載 77–99 MB、安裝約 200 MB；PdfCraft 安裝 167 MB／8 秒）對 Adobe 數 GB 的安裝（Photoshop 6.4 GB、Premiere 6.8 GB 是觀察值；Acrobat 約 2.6 GB／10 分鐘） | ● 體積／安裝（測試者觀察，但 Adobe 那邊是舊版本）；◐ 開機（CraftSuite 0.25 秒，同一台機器沒有量過 Adobe 開機） |
| 執行時記憶體 | 測過的工作上大概是 GIMP 等級（PhotoCraft 空白 214 MB、24 MP 386 MB；PdfCraft 117 頁 223 MB）；遠低於 Adobe *公布*的範圍，但**沒有找到同一台機器的 Adobe 執行時量測** | ○ 對 Adobe（只有公布範圍＋重量測試的保留）；● 對 GIMP／MuPDF 的數字 |
| 真實檔案的速度 | 在測過的 CLI 工作上比 GIMP／ImageMagick／Poppler／qpdf 附近快；PDF 渲染是 MuPDF 等級；壓縮／合併有些工作比 Ghostscript／qpdf 慢；**對 Adobe 的速度對照只有 PdfCraft 轉 Word 不到 1 秒對 Acrobat 約 2 分鐘（單次）** | ● 對開源基準；○ 對 Adobe（一次測試、跑一次） |
| 大檔案處理 | 圖磚／寫時複製設計原則上有幫助；實際上：150 層 GPU 崩潰、15000×10000 移動中位數 2.7 秒、25 個預算只達標 3 個、PSD 匯入上限估計 2 GiB（網頁版 1 GiB） | ◐（專案自己的預算——異常誠實，而且結果不好看） |
| 功能（日常） | PdfCraft 約 9.5/10 項工作；PhotoCraft 約 7/10 項工作；FigCraft 覆蓋評測者日常用的 Figma——日常工作真的能用 | ●（10 項測試＋CraftSuite＋XDA／How-To Geek 收斂） |
| 功能（專業深度） | PhotoCraft 約 45%、PdfCraft 約 50%（只算離線）、FigCraft 約 53% 堪用（專案）；AI 約 10–15%、生態約 15–20% | ◐（專案估計，有說明權重／方法；方向被測試者確認） |
| 相容性（常見檔案） | PSD／PDF／DOCX／XLSX／PPTX／DXF 日常檔案可開可存；語料庫 PSD 315/316 打得開、PDF 樣本 34/35 | ● 語料庫；◐ Office（只有小樣本） |
| 相容性（複雜／邊界檔案） | 壞掉：智慧型物件／濾鏡、CJK／RTL、圖案、多欄、DWG 內部、真實 `.fig`、複雜 Office 格式 | ●（測試者＋專案承認收斂） |
| 隱私／遙測 | 不用帳號、離線、量到 0 連線、沒有常駐程式 | ● 測過的版本（封包／行程檢查）；◐ 當成一般保證（檔案內中繼資料的保留） |
| 價格／授權 | 0 元、MIT／Apache-2.0、商業可用、可 fork | ●（授權＋發行檔案看得到） |
| 平台 | Win／mac／Linux／FreeBSD／網頁，對 Adobe 桌面版 Win／mac——ArtCraft 嚴格更廣 | ●（發行檔案清單） |
| 更新 | Launcher 一鍵＋SHA-256、幾乎每天發版；也是波動成本 | ◐（Launcher 的二手整理；發版節奏看得到版本紀錄） |
| 生態（插件／腳本／AI／雲端） | 只有自己的 WASM／CLI／MCP；沒有 Adobe／Figma／Office 插件相容、沒有生成式 AI、沒有即時協作 | ●（專案文件＋測試者確認：生成式填色沒有、Figma 插件跑不了、`.8BF` 沒有） |

### 白話 verdict

**「用 Rust 重寫」今天實際買到的：**

1. 一套免費、私人、離線、跨平台（包含 Linux／FreeBSD／瀏覽器）的工具，長得、用起來像原廠，日常工作能處理——特別是 PdfCraft 處理 PDF、PhotoCraft 處理 PSD 圖片編輯——不用帳號、不用訂閱、沒有背景常駐程式，還有原廠沒有用這種形式提供的可腳本化 CLI／MCP 引擎。
2. 下載小、冷啟動快，因為沒有 Electron 外殼、沒有 Creative Cloud 常駐程式、沒有 GC 執行環境——不過檔案載入後記憶體是普通的（GIMP 等級），不是奇蹟。
3. 看得到、可以 fork、可以自己建的原始碼——如果專案停擺，這一點比任何一項基準都重要。

**還停留在行銷、超前現實的說法：**

1. 任何「Photoshop／Acrobat 替代品」「90–100% 對等」「比 Adobe 快」的說法——專案自己的數字是 45–55% 堪用、25 個效能預算只達標 3 個，而且不存在同一台機器的 Adobe 速度／記憶體基準。病毒式傳播的「90%」是轉述錯誤（選單*接線*，不是行為）。
2. 引用 Adobe 數字卻沒有在同一台機器量 Adobe 的開機／記憶體對照表（重量測試影片、粉絲基準頁）——不要理它們；誠實的數字是 CraftSuite 的開源對照跟單次 10 項工作測試。
3. 「純 Rust＝正確／快速／安全」這種一概而論的推論——PdfCraft 還在借渲染器、PhotoCraft 在 GPU 路徑跟大圖層堆疊上還會當機，而且 Rust 變不出 AI 模型、色彩科學、字型行為或插件市集。

**從證據來的實用守則**：今天用 ArtCraft 做日常、不趕死線的工作跟自動化（CLI／MCP）——用副本，交件前先在原廠 app 裡檢查輸出。客戶交件、複雜檔案、印刷、AI 生成、插件、協作，還是留給原廠（或成熟的開源工具：GIMP／Krita、Stirling-PDF、LibreOffice、DaVinci Resolve、darktable）。大版本跳號之後要重測：以現在的發版節奏，v0.2 甚至 v0.9 的 verdict 幾週內就可能過期。

## 觀察

所以回到最初的問題：Rust 重寫之後，跟原版差在哪裡？答案不在語言本身，而在時間。Rust 給了這個家族一個乾淨的起點——小、快、不偷看你、到處都能跑、連 AI agent 都能指揮它；但「跟原版差在哪裡」的清單，讀起來幾乎全是年輕的症狀：功能還沒長齊、複雜檔案還會壞、AI 還是零、插件生態還是空的。這些會隨著 agent 工時一點一點縮小，而語言換不掉的——訓練好的模型、色彩與字型的知識、專有格式的秘密、插件市集的網路效應——才是要長期盯著看的。

最值得記住的是一個區分：**免帳號、不訂閱、沒有遙測，那是授權跟政策的選擇，不是 Rust 帶來的；Rust 真正帶來的是小體積原生程式、記憶體安全，跟同一套程式碼出桌面版和網頁版。** 把這兩件事分開，你就不會被「Rust 重寫」這句話騙去買它還沒做到的東西，也不會錯過它已經做到的。

## 方法與未查證事項

- 本專題根據 2026 年 11 月 4 日的公開資料整理：來源為網路檢索與頁面文字抓取，沒有在登入狀態下驗證，也沒有自己量測。證據標籤全文通用：［專案］＝專案自己的文件／README（自評，不是獨立證明）；［測試者］＝具名動手測試、有說明機器跟方法；［二手轉述］＝轉述專案資料的二手整理；［未驗證］＝行銷風格頁面上的說法，沒有方法或一手來源。
- **沒有同一台機器、同一批檔案的開機／記憶體／速度基準**，拿任何 ArtCraft app 跟它的*商業*原廠比（除了單次 PdfCraft 工作測試）——效能的核心問題還是開著的。
- **沒有獨立測試**：VectorCraft vs Illustrator、EffectCraft vs After Effects、DesignCraft vs InDesign、SoundCraft vs Pro Tools、TextureCraft／MaterialCraft vs Substance 工具，或 GridCraft／DeckCraft 在複雜真實 Office 檔案上（巨集、Power Query、Morph 重度投影片、追蹤修訂重度 DOCX）的表現。
- **沒有找到原廠回應**（Adobe／微軟／Figma／Autodesk／Avid）；淨室狀態是專案主張，沒有裁定。
- 沒有驗證 PhotoCraft 全專案的 `unsafe_code = "forbid"`、Adobe C++ 程式碼組成的精確數字、Office 原廠的技術堆疊——這些對照本文沒有斷言。
- 中文獨立測試：用中文找過，只找到轉載／行銷報導（AGI Hunt 起源、iplaysoft、eogee、Frontier 中文版）重複專案說法，沒有中文動手基準。上面的英文測試者語料就是證據基礎。
- 版本漂移：所有發現都是 2026 年 10 月至 11 月初的；PdfCraft v0.18.1／PhotoCraft v0.20.1 已經比多數測試用的 v0.9.0 新。行動之前請重查。

## 主要來源

專案文件：

- PdfCraft repo／README：https://github.com/storytold/pdfcraft
- PdfCraft 對照文件（專案）：https://github.com/storytold/pdfcraft/blob/main/docs/target-app-parity.md
- PhotoCraft 對照文件（專案）：https://raw.githubusercontent.com/storytold/photocraft/main/docs/target-app-parity.md
- PhotoCraft 架構／貢獻規則（專案）：https://github.com/storytold/photocraft/blob/HEAD/AGENTS.md
- PhotoCraft v0.10.0 計分卡（專案）：https://github.com/storytold/photocraft/releases/tag/v0.10.0

獨立測試：

- CraftSuite PdfCraft 0.9.0 測試：https://craftsuite.net/pdfcraft
- CraftSuite PhotoCraft 0.9.0 測試：https://craftsuite.net/photocraft
- 10 項工作 PdfCraft vs Acrobat：https://www.youtube.com/watch?v=ELmmosIsNdI
- 10 項工作 PhotoCraft vs Photoshop（Photoshop 沒跑——見保留）：https://www.youtube.com/watch?v=yradgS_aGLQ
- 重量測試（Adobe 那邊沒量過——見保留）：https://www.youtube.com/watch?v=vSIy-Disu1s
- Update 13，16 個 app／版本／issue 數（二手轉述）：https://www.youtube.com/watch?v=VKLwdepalSQ

動手評測與整理：

- Frontier Now 總覽＋Adobe 價格：https://frontiernow.dev/en/resources/articles/adobe-open-source-alternatives/
- XDA 動手評測：https://www.xda-developers.com/tried-replacing-photoshop-premiere-pro-lightroom-with-free-vibe-coded-alternatives/
- How-To Geek PdfCraft：https://www.howtogeek.com/adobe-acrobat-costs-money-pdfcraft-does-most-of-the-same-everyday-work-for-free/
- Wilson／TechRadar 早期版本：https://www.wilsonsmedia.com/one-dev-is-recreating-7-adobe-apps-in-rust-for-free-so-i-tested-it-out/
- XDA FigCraft：https://www.xda-developers.com/ditched-figma-for-free-open-source-clone/
- FigCraft 指南／對照整理：https://transjt.ai/blog/figcraft-artcraft-figma-adobe-killer
- CADCraft DWG 分析：https://pasqualepillitteri.it/en/news/22966/cadcraft-free-autocad-alternative-rust
- 12 款 app 總覽＋Launcher：https://axeetech.com/artcraft-free-adobe-microsoft-office-alternatives/
- Repo、craft-libs、Launcher 指南：https://artcraftsuite.org/blog/artcraft-github-guide/
- 架構分析：https://blog.hofstede.it/artcraft-rust-agent-desktop-apps/
- Claude 共同作者、無 Electron、網頁版：https://madrobot.blog/2026/10/10/artcraft-photocraft-free-adobe-photoshop-alternatives-built-with-claude-opus/
- Craft Anything：https://madrobot.blog/2026/11/02/craft-anything-clone-any-website-app-game/
- 淨室是主張、不是法律認定：https://cellcog.ai/blog/artcraft-open-source-adobe-office-clones/
- Rust 記憶體安全背景：https://thenewstack.io/rust-vs-c-a-modern-take-on-performance-and-safety/
- Figma Electron 桌面版：https://www.astrolytics.io/blog/top-successful-products-companies-using-electron-js
- McHenry v0.7.0 第一印象：https://mchenryslair.com/2026/10/12/artcraft-the-adobe-and-microsoft-killer/
- Korben 12 款測試：https://korben.info/en/i-tested-the-12-free-clones-of-adobe-and-microsoft.html
- TechCommune Ubuntu 測試：https://www.techcommune.org/docs/new-software/artcraft.html
- Office app 二手整理：https://toknow.ai/posts/wordcraft-gridcraft-deckcraft-cadcraft-rust-office/index.pdf
- 中文報導（轉載／行銷、沒有獨立測試）：https://www.iplaysoft.com/artcraft.html、https://eogee.com/article/250
