## Skill 範例：記帳師

接下來我們從一個小範例來進一步了解如何實作 Custom Skill。可以從 OurGroceries 的範例來聯想，做一個相似的，例如，把「加入購物車」改為「記帳」好像不錯。來做一個記帳師：Bill Man 吧！

### 設計劇本

首先要先設想對話，記帳的對話可能像這樣子：

*"Alexa, ask Bill Man to add a 200 dollar payment"*  
*"200 dollar payment added"*

這樣就紀錄了一筆 200 元支出。

#### 設計 Intent 與 slots

接著把對話轉為 intents 與 slots。對我們來說，intent 很清楚就是要記帳，所以可以訂出第一個 intent，叫他 `AddPaymentIntent` 吧，搭配一句 Sample Utterance 就是：

```
AddPaymentIntent add a 200 dollar payment
```

其中的 `200` 元是可以變動的，應當用 slot 表示：

```
AddPaymentIntent add a {payment} dollar payment
```

Intent 的格式就是這樣，開頭先是 intent 名，命名慣例是大寫 camel。空一格之後是 utterance。使用者可能會多種方式表達這個 intent，所以我們也應該多準備一些 sample utterances：

```
AddPaymentIntent add a {payment} dollar payment
AddPaymentIntent record a {payment} dollar payment
AddPaymentIntent I paid {payment} dollar
```

這樣使用者就可以換不同的說法來記帳，像是 *"Alexa, ask Bill Man to record a 15 dollar payment"* 或者 *"Alexa, tell Bill Man that I paid 100 dollar"*

#### Intent Schema 與 Sample Utterances

現在你可以去[建立一個新的 Alexa Skill](https://developer.amazon.com/edw/home.html#/skill/create/) 了。**Invocation name** 裡填入 `Bill Man`，然後在 **Interaction Model** 頁籤裡面來填入 Intent 與 Utterances。在這裡必須寫成 Alexa Custom 所要求的格式，至少需要填寫兩部分：Intent Schema 與 Sample Utterances。

**Intent Schema** 採用 JSON 格式，記錄了用到的 Intent 與 slot，對記帳的劇本來說，只要照著格式改寫：

```json
{
  "intents": [
    {
      "slots": [
        {
          "name": "payment",
          "type": "AMAZON.NUMBER"
        }
      ],
      "intent": "AddPaymentIntent"
    }
  ]
}
```

從 Intent Schema 格式可以看到，Skill 可以包含多個 intent，每個 intent 可以有多個 slot，而每個 slot 都有 name 與 type。這裡 `{payment}` 的 type 採用 `AMAZON.NUMBER`，它是一個內建的 slot type，表示數字。

> 不要小看這個 `AMAZON.NUMBER`，重點不在於它是數字，而是 Alexa 幫我們處理好使用者「怎麼說」這個數字。例如數字 `200`，口語上可以說成 "two hundred"，也可以說成 "two zero zero"，而 Alexa 可以讓使用者不管怎麼說都正確地對應到數字 `200`。有許多[內建的 slot type](https://developer.amazon.com/docs/custom-skills/slot-type-reference.html) 都處理了不少口語上的麻煩事，相當實用。

訂好 Intent，接著要列出 Intent 使用的 **Sample Utterances**，它的格式跟上面寫的是一樣的，Intent 搭配 Utterance，一行一句 Utterance：

```
AddPaymentIntent add a {payment} dollar payment
AddPaymentIntent record a {payment} dollar payment
AddPaymentIntent I paid {payment} dollar
```

好了！Skill 的部分完成了，接下來是 skill code，也就是 Lambda 的部分。在 **Interaction Model** 頁籤下面是 **Configuration** 頁籤，當你[創建 lambda function](https://console.aws.amazon.com/lambda/home?region=us-east-1#/create) 之後，記得把 lambda 的 **ARN** 填入 Configuraiotion 頁籤之下的 **Service Endpoint Type** 裡面。

### Lambda

以 Node.js 版本來說，Lambda 就是一支 aws 上的 Node.js 程式，但它畢竟在 aws 裡面，所以有個 trigger 必須選。給 Custom Skill 使用的 Lambda 要選擇的 trigger 是 *Alexa Skills Kit*。

用 Node.js 開發 skill code 可以使用 [Alexa Skills Kit SDK for Node.js](https://github.com/alexa/alexa-skills-kit-sdk-for-nodejs)，首先當然是安裝它：

```
yarn add alexa-sdk
```

然後創建一個 `index.js` 檔案，引用 alexa-sdk 並 export 一個 `handler` function：

```js
var Alexa = require('alexa-sdk');

exports.handler = function(event, context, callback){
    var alexa = Alexa.handler(event, context, callback);
};
```
> 選擇 index 這個檔名與 handler 這個 function 名稱，是因為要搭配 Lambda 的 **handler** 設定值，它預設是 *index.handler*。如果你想要選用別的名稱，記得  **handler** 設定值也要一起改。

如此會初始 `Alexa` 物件。接下來要編寫、並註冊 handlers。

#### 處理 Alexa 所發送的 request

利用 alexa-sdk 所撰寫的 skill code，基本上就像一個事件處理機，運行著，等待著 Alexa 發送的事件，處理完然後回傳結果。總共有三種事件類型：`LaunchRequest`、`IntentRequest` 與 `SessionEndedRequest`。使用者對 Alexa 說的指令屬於 `IntentRequest` 類型。不過在 skill code 內我們不是處理這三個類型，alexa-sdk 會把事件轉成更有意義的名字，我們註冊、處理起來更容易。我們必須處理的事件主要是這四種：

- `NewSession` - Lambda 支援 session，當新的 session 啟動會觸發此事件，通常這就是在使用者對 Alexa 說出第一句話的時候會發生。
- Intent Request - 也就是 Intent 的事件，也是最重要、最常處理事件，事件的名字會與 Intent 的名稱相同，所以接下來我們會註冊一個 handler 叫做 `AddPaymentIntent`
- `SessionEndedRequest` - 當對話結束的時候會發生此事件。它和其他事件有點不同的地方是它只是一個「通知」給 skill code，讓它處理一些收尾或 log 的工作而已，在這個事件沒辦法在回應任何東西給使用者，所以也不需要在這個事件對使用者說掰掰。
- `Unhandled` - 接著我們會看到，事件 handler 是好幾個包在一個物件一起註冊，如果某個事件發生了，但在這個物件內卻找不到名稱相符的 handler，就會嘗試丟給 `Unhandled` 的 handler，萬一連 `Unhandled` 都沒有的話，會拋出例外。

用一個物件把 handlers 裝起來：

```js
var handlers = {
    'NewSession': function () {
        // ...
    },
    'AddPaymentIntent': function () {
        // ...
    }
};
```

然後一起註冊：

```js
  exports.handler = function(event, context, callback) {
      var alexa = Alexa.handler(event, context, callback);
      alexa.registerHandlers(handlers);
  };
```

#### 處理 AddPaymentIntent 


