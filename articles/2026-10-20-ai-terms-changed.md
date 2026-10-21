# 你點下「同意」的那份合約，三個月後就不是同一份了

> 一份研究說：175 個 AI 工具裡，六成在 15 週內改了「免費版能不能商用」的條款。這個數字是真的——但它藏了單位遊戲。另一份名單說九家公司「預設拿你的內容去訓練」，查下去，兩家被冤枉、一家是灰色地帶。這篇文章把兩個嚇人的標題拆開，看法律怎麼說，也看你能怎麼辦。

先講一個真實的案例。今年六月，一個叫 Lexica 的 AI 繪圖工具，條款白紙黑字支持「免費版產出的圖片可以商業使用」。到了十月，條款改了：要商用，得先付費。這中間沒有大張旗鼓的公告，你不會收到一封標題寫著「你的權利縮水了」的信。發現這件事的，是一個自己重讀了 175 個 AI 工具條款的網站——因為除了重讀，沒有別的辦法知道。

這就是 AI 時代的新常態：你每天用的工具，契約在你腳下慢慢移動。這篇文章要回答四個問題：變動到底有多頻繁？「拿你的內容去訓練」是真的嗎？法律管不管？一般人能做什麼？

## 一、先把那個「六成」拆開看

ClipJury 是一個專門評測 AI 創作工具的網站。它的研究方法很笨、很紮實：把 175 個 AI 影片、語音、音樂、圖片工具的條款和定價頁逐字存檔，2026 年六月二十三日做基線，九月五日和十月三日各重讀一次，比對「影響免費版變現權利」的每一處文字。結果是：**105 個工具改了東西，佔 60%**。算術沒錯。其中 15 個工具明確收緊了免費版的商業使用權——Lexica、PicLumen、音樂生成工具 Suno 都在名單上（編按：這 15 個沒有完整名單可逐項複核，我們只能確認研究原文如此主張）。

但這個研究被轉述的時候，長出了幾個原文沒有的東西，值得一條條拆。

**「31 比 6 朝不利方向」——單位錯了。** 原文的 31 和 6 不是工具數，是「評分因子」：研究給每個工具打七個面向的分數，跨兩輪比對，31 個因子朝「對創作者風險更高」移動、6 個朝「風險更低」。所以是「在有方向性變化的因子裡，不利與有利大約 5 比 1」，不是「六成工具都在坑你」。多數被記錄的改變，根本沒有反映在方向性因子上。

**「46% 降到 32%，改變在放緩」——這是窗口的把戲。** 46% 是 74 天窗口的累計值，32% 是 28 天窗口的。把時間拉平算，第二輪的變動速度其實更快，不是更慢。兩個數字並排看起來像趨勢，實際上是兩把不同的尺。

**「默默／偷偷改」——從未被量測。** 這是最關鍵的一點。研究者自己在頁面上承認：「我們沒有這些工具的帳號，所以無法說有多少廠商寄了信通知用戶。」所以「偷偷」不是數據，是推論。能確認的只有：改變寫在頁面上，靠重讀才發現。

**清單不存在。** 研究頁說完整變更清單在自家的 changelog 頁，但我們實際去開（2026-10-20），頁面顯示的是 "No terms changes recorded yet."——105、41、15 這幾個數字，沒有逐項清單可以獨立複算。

最後是利益揭露。ClipJury 由一位叫 Abdallah Amjid 的人經營，自述經營不露臉的 YouTube 頻道；收入來自聯盟行銷佣金、一次 499 到 1,499 美元的條款稽核服務，和每月 1,499 美元的「條款變更監測」訂閱。研究結論（變動頻繁、多數不利、需要持續監測）恰好直接推高他賣的服務。這不證明數據造假，他的方法有逐字引句存檔，而且他還公開承認自家早期數據有 18 處把付費版文字誤當免費版證據、全部降級重來——這是加分。但「賣解方的人自產問題數據」這個結構，讀者有權知道。

## 二、「九家公司預設拿你訓練」：拆開是四種不同的事

第二份被廣傳的名單來自 HasData，一家賣網頁爬蟲 API 的公司（這同樣是內容行銷，引用時該交代）。它點名九家公司「預設開啟用你的內容訓練 AI」。名單是真的，但逐家對照各公司自己的條款和說明頁之後，九家其實是四種不同的事：

**真的預設開（多數）。** OpenAI 最典型：個人版（含付費的 Plus 和 Pro）對話預設可用於訓練，要自己去設定裡關，而且**關掉只管之後的對話**。GitHub 也在名單上，但要標日期：2026 年四月二十四日起，個人版 Copilot 的「互動資料」（你的提問、它的建議、程式碼片段）預設用於訓練——注意，訓練的是你用 Copilot 時的互動，不是你靜靜放在 GitHub 上的程式碼本身。Microsoft 的消費者 Copilot 也是預設開，但原文有「在部分市場」的限定，六個國家被排除，企業版完全不在此列。Mistral 的聊天產品（原 Le Chat，現已改名 Vibe）個人版預設開、企業版預設關。DeepSeek 大致成立，而且是九家裡分層最弱的：沒有給 API 一個明文豁免，資料明文存在中國。Stability AI 最極端：連開發者用的 Platform API 都要自己去關。

**被冤枉的兩家。** Adobe 被 HasData 排在「最愛拿你內容」的第一名，但 Adobe 的具拘束力條款在三個地方明文寫著：不用客戶的本地或雲端內容訓練生成式 AI（投稿 Adobe Stock 市集的除外）。它真正預設開的是一個叫「內容分析」的東西——用機器學習分析你存在它伺服器上的檔案來改進產品，可以關，但那不等於「拿你的作品訓練 AI」。Quora 更直接：官方說明頁開宗明義寫著「我們目前不用用戶的回答、貼文或留言訓練生成內容的 LLM」——它預設開的是一個「未來許可」的開關：行為還沒發生，許可先備好了。你當然可以批評「許可先行」的做法，但把「目前沒在做」寫成「正在拿你訓練」，是事實錯誤。

**灰色的一家。** Anthropic 不是安靜地預設開啟。2025 年八月它改了消費者條款：每個用戶會被跳出視窗**強迫表態**，不選就不能繼續用；而且選擇畫面裡那個資料分享開關被預選在「開」，按鈕設計被媒體批評是 dark pattern（暗黑模式）。連帶效應是保留期：同意訓練的人，對話保留從 30 天拉長到 5 年。把它和 OpenAI 的「安靜 opt-out」並列成同一種「預設開」，抹掉了「強迫表態」這個關鍵差別。

把九家攤開，真正穩固的橫向結論有兩個。第一，**opt-out 普遍只管未來**：OpenAI、Microsoft 都在官方頁面明寫，關掉只排除之後的對話，已經用掉的不會吐回來。第二，**真正的分界不是免費與付費，是你簽的是哪一種契約**：ChatGPT 的 Plus/Pro、Claude 的 Pro/Max、Copilot 的付費個人版，付了錢，訓練預設照樣開；真正豁免的是 Business、Enterprise 和 API 這些「商務契約」。Google 把這件事寫得最露骨：Gemini API 的附加條款直接分成「Unpaid Services」和「Paid Services」兩節——免費節明文說會用你的輸入輸出改進產品、人工審閱者可能讀到，還用粗體警告你別提交敏感資料；付費節改口不用。最有意思的註腳是：位於歐洲經濟區、瑞士、英國的用戶，就算用免費版也適用付費版的資料條款——同一個免費帳號，人在歐洲和在台灣，權利不一樣。法規，會直接改變預設值。

## 三、歷史的節奏：改的時候沒人看，被翻出來才燒

這種事不是 AI 時代才有的。把過去十幾年的名場面排開，節奏驚人地一致：**條款先改，幾個月後被翻出來，才引爆。**

2012 年是祖師爺。Instagram 公布新條款，一句「企業可付費展示你的用戶名、肖像、照片，不給補償」被讀成「Instagram 要賣你的照片」。共同創辦人 Kevin Systrom 先澄清、再道歉，說了一句後來被引用無數次的話：「我們不擁有你的照片——你才擁有。」廣告相關條款直接退回 2010 年的原始版本。

2023 年的 Zoom 是 AI 版。條款其實是三月改的：授權 Zoom 把「服務生成資料」用於「任何目的」，包括訓練和調校 AI 模型。五個月沒人注意，直到八月被部落格翻出才爆發。電子前哨基金會的 Katharine Trendacosta 對美聯社拋出一個到今天都無解的問題：「如果按同意的是管理員、是你老闆，而你被要求必須用 Zoom——那算哪門子同意？」Zoom 先在八月七日補上「未經你同意不用你的音訊、視訊、聊天訓練 AI」，被質疑「同意」有漏洞後，八月十一日乾脆把「未經你同意」拿掉，給出無條件承諾——但請注意，行為和使用數據這類「服務生成資料」至今不在承諾範圍內。

2024 年的 Adobe 是最大場。爭議條款二月就改好了，寫著 Adobe 可以用自動和人工方式「存取、檢視或聆聽你的內容」，自動系統會用機器學習分析它。六月，一個「不接受就不能用軟體」的強制彈窗把所有人逼到條款面前，創作者炸鍋——包括受 NDA 保護的客戶專案會不會被看光。Adobe 在 12 天內發了兩篇部落格，六月十八日推出新版條款，把「不用你的本地或雲端內容訓練生成式 AI」寫進具法律效力的正文。首席策略長 Scott Belsky 說：「我們從未用客戶內容訓練生成式 AI，從未取得客戶作品的所有權。」但一位用戶的回應點出了整件事的本質：「我對 Adobe 的澄清和部落格文章一個字都不在乎。你強迫人們簽了新條款。法律上，只有那個才算數。」

同一年還有兩個對照組。Slack 的隱私原則寫著會分析客戶資料開發 AI/ML 模型、退出要由工作區管理員寄 email 申請，被翻出後它只改措辭澄清「傳統機器學習只用去識別化的彙總數據、不讀訊息內容」——政策沒變，一個字都沒退。GitHub Copilot 則是完全不退讓的那型：爭議從社群打進法院，打了四年；今年九月第九巡迴上訴法院維持駁回 DMCA 主張，但違反開源授權的契約主張還在訴訟中。

四種結局：寫進條款（Adobe）、刪掉限定詞（Zoom）、只改措辭（Slack）、法院見（Copilot）。哪一種會發生，取決於反彈的音量和公司的盤算——很少取決於條款本身寫了什麼。

## 四、法律怎麼說：三地卡在同一個點

單方面改條款，到底站不站得住？三個法域的答案出奇地一致：**卡在「通知」和「真正的同意」。**

美國最常被引用的判例是 2007 年的 Douglas 案。第九巡迴法院寫得很直白：「契約的一方沒有義務定期回網站檢查條款有沒有被對方改掉。而且，一方不能單方面變更契約條款；必須先取得他方的同意。」只把新條款貼上網站、沒通知用戶，變更不可執行。2024 年二月，美國聯邦貿易委員會（FTC）發文把話說得更白：企業採取更寬鬆的資料作法（例如開始把資料分享給第三方、或拿去訓練 AI），卻只用「偷偷的、溯及既往的」條款修訂來告知，「可能構成不公平或欺騙」。FTC 早在 2004 年就執法過同型案件，當時的消費者保護局局長留下一句名言：「比賽打完了才改規則，是不公平的。」

歐盟的工具是兩件。1993 年的《不公平條款指令》把「讓業者在沒有契約明定正當理由的情況下單方變更條款」列入可能不公平的灰名單，逐案審查——注意，它不是全面禁止：有正當理由、合理通知、讓消費者能自由解約的變更，指令明文容許。另一件是 GDPR 的同意標準：同意必須是「明確的肯定動作」，歐盟的資料保護委員會明言「僅僅繼續使用服務」不算主動的同意表示。「付費或同意」（consent or pay）模式在 2024 年也被 EDPB 出具意見，認為多數情況下不構成有效同意。

台灣的《消費者保護法》有三道防線。第十三、十四條：條款沒向消費者明示、或屬消費者無從預見的，「不構成契約內容」——換句話說，改了也不算改進契約裡。第十二條：條款違反誠信原則、對消費者顯失公平者，無效——這是逐案挑戰的路。第十七條最值得玩味：法律明文授權主管機關把「企業經營者保留契約變更權」列入「不得記載事項」，一旦某個行業被公告，寫了就無效；但它是一個授權機制，不是一條自動生效的全面禁令。本研究沒有查到台灣法院針對「網路服務條款單方變更」的指標判決，也沒有查到消基會就 AI 服務條款的具體發言——這條戰線在台灣，基本上還沒開打。

## 五、自保：評級不可盡信，但有三件事今天就能做

那一般人能怎麼辦？先看一個讓人清醒的實查。ToS;DR（「服務條款；沒人讀」）是一個 2012 年起由志工逐條拆解條款、給 A 到 E 評級的社群專案，官網標語是：「『我已閱讀並同意條款』是網路上最大的謊言。」我們在 2026 年十月二十日實際查了它給主流工具的評級：**OpenAI 是 C、Adobe 是 C、GitHub 是 D、Claude 是 D、Canva 是 D**——沒有一家拿到 A 或 B。而且 Canva 的條目停在 2023 年沒更新，Claude 的 D 是在「未完整評審」狀態下給的。連看評級這件事本身，都要打折看。

再說一個真空：**專門做給消費者的條款追蹤工具，幾乎不存在。** EFF 早在 2009 年就做過 TOSBack，把各網站條款的增刪並列標色給公眾看，如今只剩歷史地位；現在有叫 TermsMonitor 的服務在追蹤條款變更，但那是賣給企業做法遵的。一般人能用的，是通用工具拼裝：

**第一，存證。** 用 Wayback Machine 的 Save Page Now，把你重度依賴的工具的條款頁和定價頁存一份帶時間戳的快照。哪天權利被改掉，你至少能指出「我當初同意的是這一版」。限制要說清楚：需要登入或動態載入的頁面可能存不完整，快照是證據輔助，不是公證。

**第二，監測。** 把條款頁丟進 Visualping 這類網頁變更監測工具（免費版可監測五個頁面），或自己架開源的 changedetection.io，設定只在出現 train、retain、license 這類關鍵字時才通知你。它只告訴你「頁面變了」，變好變壞還是要人讀——但至少你不會是最後一個知道的。

**第三，管好開關，認清契約。** 每個產品的訓練開關是分開的：同一個集團裡，GitHub Copilot 和消費者 Copilot 是兩套政策；Mistral 明說 Vibe 和 API 的開關互不相通。你用三個產品，就有三個開關要找。更重要的是認清那條分界：**付費個人版不等於不被訓練，商務契約才等於。** 如果你的工作內容真的敏感（客戶資料、未發表作品、NDA 專案），該考慮的是商務版或 API，而不是把個人版升到最貴的那一級。

最後，一個誠實的註腳：這篇文章的台灣現場是空的。我們用多組關鍵字查過，找不到任何台灣創作者或自媒體工作者具名陳述「因 AI 工具條款變更受害」的報導或調查——台灣有使用規模（金管會調查金融業 AI 導入率 43%），有繁中語料的著作權論戰，但「條款變更打到台灣用戶」的故事，公開紀錄裡還不存在。這可能代表台灣用戶還沒被燒到，也可能代表燒到了也沒人寫。哪一種，要等第一個具名站出來的人。

## 六、追查：改版之後，有人跳出新的同意視窗嗎？

文章寫到這裡，有一個自然的追問：這些工具改條款之後，有沒有哪怕一家，認認真真跳出一個視窗，對用戶說「條款改了，請重新同意」？我們把 ClipJury 監看的工具名單攤開，逐家去讀它們現行條款裡「變更如何通知」的那一段，並搜尋 2026 年實際出現同意視窗的公開紀錄。71 個廠商完成查核，59 家可以分類。

先說最乾淨的結論：**2026 年被證實跳出「條款變更強制再同意視窗」的案例，一家都沒有，零。** 唯一在條款文字裡寫明會這樣做的，是印度影片工具 InVideo：它承諾重大變更提前 30 天以 email 和產品內通知，而且付費用戶與歐洲用戶的重大變更「要等你下次登入時，在產品內通知裡明確確認後，才對你生效」。這是目前找到最接近「真正的同意」的設計——但它只存在於條款文字，2026 年實際這樣執行的公開紀錄，同樣是零。

條款層面其實比想像中好看一點。完成分類的 59 家裡，31 家（53%）承諾重大變更會以 email 或產品內方式通知，少數是帶著天數的「硬承諾」：OpenAI 寫明重大不利變更提前至少 30 天、Meta 至少 30 天、Google 現行條款至少 15 天並以 email 通知，Fliki 也是 30 天。但魔鬼在附帶條件：這些承諾幾乎只限「重大」變更，而什麼算重大，多由廠商單方認定；好幾家的承諾是「努力義務」——HeyGen 的條款寫的是會以「合理努力嘗試通知你」（reasonable efforts to *attempt* to notify you），它承諾的是試著通知，不是通知到。13 家屬 B 級：只在頁面張貼、繼續使用等於接受，PixVerse 甚至明文保留「不通知而變更」的權利。其餘 15 家，含糊帶過或根本沒有變更通知條款。而就算在 A 級裡，「接受」的方式也幾乎一律是繼續使用——通知給了你看的機會，不給你按「不同意」的按鈕。

「承諾」和「做到」之間，找不到證據銜接。被研究點名的 PicLumen，條款白紙黑字是 A 級——重大變更會 email 或站內通知；但它在 2026 年拿掉免費版商用權這件事，找不到任何一位用戶回報收到過通知。反過來，被點名的 Suno，我們沒能查證它的條款（網站封鎖了研究工具），它卻留下了公開紀錄：2026 年八月十日公開宣布九月三日改版，舊條款頁頂端還掛過「我們的條款即將變更」的橫幅——那是公告，不是同意視窗。最諷刺的是對照組：歷史上真的用過強制再同意彈窗的，正是 Adobe（2024 年），也就是引爆創作者怒火的那一次。機制存在、被用過；而 2026 年整個產業的標準動作是：發個通知（或不發），你繼續用，就算你同意了。

還有 12 家連分類都做不到，而原因本身就是發現：LOVO 已申請破產清算、網站消失；PlayHT 已在 2025 年底落日關站，2026 年根本沒有條款可以改；Gling、Kaiber、FlexClip、Lumen5 連一份找得到的官方條款頁都沒有。連「它打算怎麼通知你」都無從查起——這也是答案的一部分。

## 結語：唯一不會變的條款，是你自己存的那一版

把三個法域的判決和指引疊起來看，會發現法官和監管者其實在說同一句話：規則可以改，但要先通知，要取得真正的同意，不能溯及既往。這句話現在還停在判例和指引裡，沒有變成「條款一改你就會收到人話通知」的產品現實。在那之前，你和平台之間那份契約，唯一你能確定不變的版本，是你自己存下來的那一版。

簽約那天存一份快照，每季花十分鐘檢查一次開關——這是目前為止，一個普通用戶能為自己做的、最不浪漫但最有效的兩件事。

---

*資料說明：ClipJury 研究（2026 年 6–10 月，175 個 AI 影音圖工具）與 HasData「九家公司」名單為本文出發點，兩者的利益結構與方法限制已在文中揭露；九家公司的實際狀態逐家對照各公司官方條款與說明頁查證（2026-10-20）。法律部分引美國第九巡迴法院 Douglas 案判決、FTC 2024 年指引、歐盟 93/13/EEC 指令與 GDPR、台灣《消費者保護法》條文原文。台灣具名受害案例經多輪搜尋查無，已如實註明。第六節的同意視窗追查，逐家條款原文查證於 2026-10-20；登入後才觸發的視窗無法從外部觀察，文中「未發現公開紀錄」不等於「確定不存在」。*

---

## 附錄：ClipJury 監看工具清單

先講清楚一個限制：ClipJury 從未公布 2026 年六月基線那「175 個」的逐項名單——這份名單不存在公開版本。以下刊出的是它 changelog 頁的**現行監看清單**，共 180 項（其中 Quso 與 Vidyo.ai 為同一產品重複列計，去重後約 179 個；另有 4 項為開源權重模型）。現行清單會隨覆蓋擴大而增長，極可能近似 175 基線的超集，但無法逐項確認對應關係。★ 為研究頁具名、在期間內發生對創作者有實質影響之改變的工具（Google Flow 為有利改變，其餘為不利或服務消失）。

1. Veo（Google，policies.google.com）2. Kling（kling.ai）3. Higgsfield（higgsfield.ai）4. HeyGen（heygen.com）5. ElevenLabs（elevenlabs.io）6. Runway（runway.com）7. Sora（OpenAI，openai.com）8. Pika（pika.art）9. Synthesia（synthesia.io）10. Fliki（fliki.ai）
11. Submagic（submagic.co）12. Speechify（speechify.com）13. Hailuo（MiniMax，hailuoai.video）14. Luma Dream Machine（lumalabs.ai）15. Descript（descript.com）16. OpusClip（opus.pro）17. Murf AI（murf.ai）18. D-ID（d-id.com）19. Krea AI（krea.ai）20. Revid.ai（revid.ai）
21. Vizard（vizard.ai）22. Crayo AI（crayo.ai）23. Vidu（vidu.com）24. Klap（klap.app）25. Pictory（pictory.ai）26. PixVerse（pixverse.ai）27. Argil（argil.ai）28. Captions（captions.ai）29. CapCut（capcut.com）30. VEED（veed.io）
31. Wan（阿里巴巴，開源模型）32. Hedra（hedra.com）33. Fish Audio（fish.audio）34. LTX Studio（ltx.io）35. Dreamina（dreamina.capcut.com）36. Pollo AI（pollo.ai）37. Vidnoz AI（vidnoz.com）38. Colossyan（colossyan.com）39. DeepBrain AI／AI Studios（aistudios.com）40. Firefly（Adobe，adobe.com）
41. Genmo（genmo.ai）42. ★LOVO（lovo.ai，公司已清算）43. Cartesia（cartesia.ai）44. Quso（quso.ai）45. 2short.ai（2short.ai）46. Munch Studio（munchstudio.com）47. Steve AI（steve.ai）48. AutoShorts.ai（autoshorts.ai）49. Vrew（vrew.ai）50. Kapwing（kapwing.com）
51. Creatify（creatify.ai）52. Lumen5（lumen5.com）53. InVideo AI（invideo.io）54. ★Suno（suno.com）55. Midjourney（midjourney.com）56. Ideogram（ideogram.ai）57. Leonardo AI（leonardo.ai）58. Gling（gling.ai）59. Udio（udio.com）60. Mubert（mubert.com）
61. Riverside（riverside.com）62. Soundraw（soundraw.io）63. Boomy（boomy.com）64. Vmake（vmake.ai）65. Reve（app.reve.com）66. Recraft（recraft.ai）67. Canva（canva.com）68. AIVA（aiva.ai）69. Hour One（hourone.ai）70. WellSaid Labs（wellsaid.io）
71. Loudly（loudly.com）72. Soundful（soundful.com）73. Adobe Express（adobe.com）74. Resemble AI（resemble.ai）75. TimeBolt（timebolt.io）76. Beatoven.ai（beatoven.ai）77. Playground AI（playgroundai.com）78. Wisecut（wisecut.ai）79. Freepik AI（監看來源 magnific.com）80. Soundverse（soundverse.ai）
81. LALAL.AI（lalal.ai）82. Simplified（simplified.com）83. Magnific（magnific.com）84. Fotor（fotor.com）85. Kaiber（kaiber.ai）86. FlexClip（flexclip.com）87. HunyuanVideo（騰訊，開源模型）88. NotebookLM（Google）89. Topaz Video AI（topazlabs.com）90. Filmora（萬興，filmora.wondershare.com）
91. Renderforest（renderforest.com）92. Speechelo（speechelo.com）93. Elai.io（elai.io）94. Animaker（animaker.com）95. Seedance（字節跳動，seed.bytedance.com）96. Viggle AI（viggle.ai）97. Listnr AI（listnr.ai）98. Hume AI Octave（hume.ai）99. Adobe Podcast（podcast.adobe.com）100. Photoroom（photoroom.com）
101. Riffusion（riffusion.com）102. Synthesys AI Studio（synthesys.io）103. FLUX（Black Forest Labs，bfl.ai，開源權重）104. Stable Diffusion（Stability AI，stability.ai）105. DALL-E／GPT Image（OpenAI，openai.com）106. Nano Banana（Google，ai.google.dev）107. Grok Imagine（xAI，x.ai）108. Pippit（CapCut／字節跳動，pippit.ai）109. Rask AI（rask.ai）110. Arcads（arcads.ai）
111. Akool（akool.com）112. DomoAI（domoai.app）113. Gamma（gamma.app）114. Vozo AI（vozo.ai）115. Zebracat（zebracat.ai）116. Clipdrop（clipdrop.co）117. remove.bg（remove.bg）118. Picsart（picsart.com）119. Remini（remini.ai）120. Civitai（civitai.com）
121. Vidyo.ai（quso.ai，與第 44 項為同一產品）122. Spikes Studio（spikes.studio）123. Camb.ai（camb.ai）124. Voicemod（voicemod.net）125. Narakeet（narakeet.com）126. Qwen Image（阿里巴巴，開源模型）127. Seedream（Dreamina，capcut.com）128. Sesame AI（sesame.com）129. Mureka（mureka.ai）130. Topview AI（topview.ai）
131. Moonvalley／Marey（moonvalley.com）132. Bing Image Creator（bing.com）133. Getimg AI（getimg.ai）134. NightCafe（nightcafe.studio）135. Tensor Art（tensor.art）136. Mage Space（mage.space）137. StarryAI（starryai.com）138. SeaArt（seaart.ai）139. ★PicLumen（piclumen.com）140. Vivago AI（vivago.ai）
141. DeepMotion（deepmotion.com）142. Morph Studio（morphstudio.com）143. Decohere（decohere.co）144. Minimax Audio（minimax.io）145. Wondercraft AI（wondercraft.ai）146. Typecast（typecast.ai）147. ★PlayHT（play.ht，平台已落日）148. Treblo（sonauto.ai）149. Phot AI（phot.ai）150. Perchance AI image generator（perchance.org）
151. Pixlr AI（pixlr.com）152. DeepAI（deepai.org）153. PixAI（pixai.art）154. Kits AI（kits.ai）155. Uberduck（uberduck.ai）156. Voicify AI／Jammable（voicify.ai）157. Musicfy（musicfy.lol）158. Craiyon（craiyon.com）159. NovelAI（novelai.net）160. Bria AI（bria.ai）
161. Dezgo（dezgo.com）162. Artbreeder（artbreeder.com）163. ImagineArt（imagine.art）164. ★Lexica（lexica.art）165. NaturalReader（naturalreaders.com）166. Luvvoice（luvvoice.com）167. Revoicer（revoicer.com）168. Voicemaker（voicemaker.in）169. Speechma（speechma.com）170. DupDub（dupdub.com）
171. Yepic AI（yepic.ai）172. Lalals（lalals.com）173. Segmind（segmind.com）174. AiEase（aiease.ai）175. insMind（insmind.com）176. ★Google Flow（support.google.com）177. Meta Vibes（meta.com）178. OpenArt（openart.ai）179. Vheer（vheer.com）180. Google AI Studio（ai.google.dev）
