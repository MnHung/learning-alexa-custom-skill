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

Invocation name 是用來對映 skill 用的，也就是告訴 Alexa，使用者說出的要求應當交給哪一個 skill 來處理。以 [Alexa Skills](https://www.amazon.com/b?node=13727921011) 上面幾個熱門的 skill 來看，喚醒 [Sleep Sounds: Ocean Sounds](https://www.amazon.com/gp/product/B071KYWH2L?ref=skillrw_dsk_tes_gw_8) 的句子是：*"Alexa, open Ocean Sounds"*；指示 [OurGroceries](https://www.amazon.com/HeadCode-OurGroceries/dp/B01D4F1J0M/ref=lp_14284822011_1_2?s=digital-skills&ie=UTF8&qid=1512461485&sr=1-2) 變更購物清單的句子是：*"Alexa, tell OurGroceries to remove olive oil from Walmart"* 或者 *"Alexa, ask OurGroceries to add milk to shopping list"*。這幾句話裡的 *Ocean Sounds* 和 *OurGroceries* 就是兩個 skill 各自的 invocation name，各自對映，我們當然不能 ask Ocean Sounds 去 add milk，Ocean Sounds 很可能根本不會做 *add*，即使它會，意思跟 OurGroceries 的 add 也不一樣，它們是兩個不同的 skill。






