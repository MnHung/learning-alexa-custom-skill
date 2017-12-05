# Alexa Custom Skill 學習手冊

這是給新手的 Alexa Custom Skill 介紹文，以範例導向、開發導向撰寫，目標是讓剛接觸 Alexa Custom Skill 的開發者可以盡快上手、開發自己的 Custom Skill。


## 前提條件

雖然是入門文，但開發所需要前提條件與技術並不會介紹，包括：

- [Amazon Developer 帳號](https://www.amazon.com/ap/register?openid.pape.max_auth_age=1&openid.return_to=https%3A%2F%2Fdeveloper.amazon.com%2Fap_login%2F68747470733A2F2F646576656C6F7065722E616D617A6F6E2E636F6D2F6564772F686F6D652E68746D6C.html&prevRID=7CPQ49X4WXMZ4654HAED&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.assoc_handle=mas_dev_portal&openid.mode=checkid_setup&prepopulatedLoginId=&failedSignInCount=0&language=en_US&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&pageId=amzn_developer_portal&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0) - 用來開發 Alexa Skill
- [Amazon aws 帳號](https://portal.aws.amazon.com/billing/signup#/start) - 用來開發 aws lambda
- Node.js - 開發 aws lambda 所使用的語言
- 噢還有，你最好要有一台 [echo](https://www.amazon.com/Amazon-Echo-And-Alexa-Devices/b/ref=sv_devicesubnav_0?ie=UTF8&node=9818047011)，不然你只能用難用得要死的 [echo 模擬器](https://echosim.io/welcome)

## 什麼是 Alexa Skill

首先快速認識一下 Alexa Skill。Alexa 是一個語音助理，她可以聽懂 user 說的話，做些事情，然後說話回應。Alexa 一開始就內建一些 skill，例如她可以幫你設定鬧鐘，當 user 說：*"Alexa, set alarm at eight o'clock AM"* 接著 Alexa 會說：*"Alarm set for eight AM"* 等早上八點到的時候鬧鐘會響起。這個「鬧鐘」是一個內建的 skill。

看個影片可以更快了解 Alexa：https://youtu.be/xenOYWVwkGY 

Alexa skill 有點像手機裡的 app，只是從手機上的圖形介面（GUI），換成聲音介面（VUI - Voice User Interface），而就像 app 有 apps store 那樣，Amazon 也有一個 [Alexa Skills](https://www.amazon.com/b?node=13727921011)，就像一個 skill 的市集，你可以啟用想要的 skill，就彷彿讓 Alexa「學會」新的 skill 一般。

接下來你將學會如何開發一個 Custom Skill。

## 什麼是 Custom Skill

Alexa Skills 分為好幾種：Custom Skill、Smart Home Skill、Flash Briefing Skill、Video Skills...。其中 Custom Skill 自由度高，方便開發者客製自己想要的功能，它和大多數 skill 有個不一樣的地方是，它需要一個 **invocation name**。

### Invocation Name

Invocation name 是用來對映 skill 用的，也就是告訴 Alexa，使用者說出的要求應當交給哪一個 skill 來處理。以 [Alexa Skills](https://www.amazon.com/b?node=13727921011) 上面幾個熱門的 skill 來看，喚醒 [Sleep Sounds: Ocean Sounds](https://www.amazon.com/gp/product/B071KYWH2L?ref=skillrw_dsk_tes_gw_8) 的句子是：*"Alexa, open Ocean Sounds"*；喚醒 [OurGroceries](https://www.amazon.com/HeadCode-OurGroceries/dp/B01D4F1J0M/ref=lp_14284822011_1_2?s=digital-skills&ie=UTF8&qid=1512461485&sr=1-2) 並指示它變更購物清單的句子是：*"Alexa, tell OurGroceries to remove olive oil from Walmart"* 或者 *"Alexa, ask OurGroceries to add milk to shopping list"*，還有查詢清單的句子是：*"Alexa, ask OurGroceries what are my lists"*。這幾句話裡的 *Ocean Sounds* 和 *OurGroceries* 就是兩個 skill 各自的 invocation name，各自對映，我們當然不能 ask Ocean Sounds 去 add milk，Ocean Sounds 很可能根本不會做 *add*，即使它會，意思跟 OurGroceries 的 add 也不一樣，它們是兩個不同的 skill。

我們繼續拆解喚醒 OurGroceries skill 句子，看看除了 invocation name 之外還有什麼，首先再看一次它如何在購物清單上增加商品：

*"Alexa, tell OurGroceries to remove olive oil from Walmart"*

它可以拆成這些部分：

 - Wake word（喚醒詞） - Alexa
 - **Invocation name**（喚醒詞） - OurGroceries
 - Invocation phrase（喚醒語句） - tell ... to ...
 - **Intent**（意圖） - remove olive oil from Walmart
 - **Slot**：其中 intent 中又包含一個 slot - Walmart
  
粗體字的 **Invocation name**、**Intent** 與 **Slot** 是 Alexa Skill 的專有名詞，其中 **Intent** 與 **Slot** 是組成一個 Skill 的重點。

### Invocation phrase

喚醒語句是整個句型的結構，對使用者來說，要說出一個要求當然可能有非常多種句型、連接詞的組合，例如:

- ask...to...
- ask...for...
- ask...about...
- ask...if...
- tell...to...
- tell...that...

也可以用非常多種動詞，例如：ask, begin, launch, load, open, start, tell, use...

更完整的介紹可以看 [Understanding How Users Invoke Custom Skills](https://developer.amazon.com/docs/custom-skills/understanding-how-users-invoke-custom-skills.html)

### Intent 與 Sample Utterances

Intent（意圖）是整個 Alexa Skill 最重要的一個腳色，也是 Alexa 表現語音辨識人工智慧裡面最「智慧」的元素。一般在對話中，最重要的就是了解對方「想幹嘛」，而 Intent 就是用來描述「想幹嘛」。我們再來看一次 OurGroceries 的三句話：

 - *"Alexa, tell OurGroceries to remove olive oil from Walmart"*
 - *"Alexa, ask OurGroceries to add milk to shopping list"*
 - *"Alexa, ask OurGroceries what are my lists"*

這三句話剛好代表三個不同的 Intent。甚至可以套用到應用程式常見的 crud 操作來思考，剛好是對購物清單做：移除、新增、與查詢。知道使用者想做什麼，離我們可以開始實作就不遠了。

回頭想想 invocation phrase，它支援那麼多種「說法」，卻都只為了做同一件事，原因當然是因為自然語言是含糊的，而且一個意思可以有很多種不同說法。在單一個 skill 裡面要支援一個 intent 也必須考慮使用者可能會用各種說法來表達相同的 intent，所以其實 intent 不會只對映一個 utterance，而是一堆 sample utterances，越多越好。官方是建議盡量找出所有可能的說法，寧多勿少。以 OurGroceries 的查詢購物清單為例，它網站雖然只寫了一句 *"what are my lists"*，但這肯定是因為版面有限沒寫出來，可以想見，使用者想查詢購物清單，以下這些說法它可能都支援：

 - *"what are my shopping lists"*
 - *"show my lists"*
 - *"tell me the current shopping lists"*
 
 這三句話，就是「查詢購物清單」intent 的三個 Sample Utterances。
 
### Slots

Slots 是 Intent 的一部分，可以根據使用者所說不同的詞彙填入不同的東西，像 OurGroceries 的 *"add milk to shopping list"*，其實是一個這樣的句型：

*"add ... to shopping list"*

可以代換不同的東西，來加入到購物清單，更具體一點來說當然是某種商品，像是 paper, cake, iphone 之類的。因此這句型更像是：

*"add {product} to shopping list"*

這 *{product}* 就是讓使用者填空的東西，組合起來就是完整的 intent：使用者想要將什麼 {product} 加進購物清單中。

### 組合成 Custom Skill

以上，由 sample utterances 對映到 intent，也就是使用者所說的話，Alexa 會幫我們轉為使用者的意圖（intent）。接著再由一個或多個 intents 組合起來，就幾乎可以組合為一個 custom skill 了，就只差寫程式去處理 intent，然後再 render 出回應、說給使用者聽。

接下來我們看看一個範例，可以了解如何實作出一個 custom skill。

[Next: Custom Skill 範例](custom-skill-example-bill-man.zh-tw.md)
