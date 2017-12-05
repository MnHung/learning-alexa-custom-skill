# Alexa Custom Skill 學習手冊

這是給新手的 Alexa Custom Skill 介紹文，以範例導向、開發導向撰寫，目標是讓剛接觸 Alexa Custom Skill 的開發者可以盡快上手、開發自己的 Custom Skill。


## 前提條件

雖然是入門文，但開發所需要前提條件與技術並不會介紹，包括：

- [Amazon Develper 帳號](https://www.amazon.com/ap/register?openid.pape.max_auth_age=1&openid.return_to=https%3A%2F%2Fdeveloper.amazon.com%2Fap_login%2F68747470733A2F2F646576656C6F7065722E616D617A6F6E2E636F6D2F6564772F686F6D652E68746D6C.html&prevRID=7CPQ49X4WXMZ4654HAED&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.assoc_handle=mas_dev_portal&openid.mode=checkid_setup&prepopulatedLoginId=&failedSignInCount=0&language=en_US&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&pageId=amzn_developer_portal&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0) - 用來開發 Alexa Skill
- [Amazon aws 帳號](https://www.amazon.com/ap/signin?openid.assoc_handle=aws&openid.return_to=https%3A%2F%2Fsignin.aws.amazon.com%2Foauth%3Fcoupled_root%3Dtrue%26response_type%3Dcode%26redirect_uri%3Dhttps%253A%252F%252Fconsole.aws.amazon.com%252Fconsole%252Fhome%253Fstate%253DhashArgs%252523%2526isauthcode%253Dtrue%26client_id%3Darn%253Aaws%253Aiam%253A%253A015428540659%253Auser%252Fhomepage&openid.mode=checkid_setup&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&action=&disableCorpSignUp=&clientContext=&marketPlaceId=&poolName=&authCookies=&pageId=aws.login&siteState=registered%2CEN_US&accountStatusPolicy=P1&sso=&openid.pape.preferred_auth_policies=MultifactorPhysical&openid.pape.max_auth_age=120&openid.ns.pape=http%3A%2F%2Fspecs.openid.net%2Fextensions%2Fpape%2F1.0&server=%2Fap%2Fsignin%3Fie%3DUTF8&accountPoolAlias=&forceMobileApp=0&language=EN_US&forceMobileLayout=0&awsEmail=mnhung%40gmail.com) - 用來開發 aws lambda
- Node.js - 開發 aws lambda 所使用的語言

## 什麼是 Alexa Skill

首先快速認識一下 Alexa Skill。Alexa 是一個語音助理，她可以聽懂 user 說的話，做些事情，然後說話回應。Alexa 一開始就內建一些 skill，例如她可以幫你設定鬧鐘，當 user 說：*"Alexa, set alarm at eight o'clock AM"* 接著 Alexa 會說：*"Alarm set for eight AM"* 等早上八點到的時候鬧鐘會響起。這個「鬧鐘」是一個內建的 skill。

Skill 有點像手機裡的 app，而就像 app 有 app store 那樣，Amazon 也有一個 [Alexa Skills](https://www.amazon.com/b?node=13727921011)，就像一個 skill 的市集，你可以啟用想要的 skill，就像讓 Alexa 學會新的 skill 一般。接下來你將學會如何開發一個 Custom Skill。

##
