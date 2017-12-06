## 理解 Custom Skill 如何運作

在動手實作之前，我們先要理解 Custom Skill 如何運作。我必須說，實作階段要做的事情只有一點點，但其實 Skill 的運作流程步驟非常多，而大部分的麻煩事都由 Alexa 幫我們做完了。

看看下面的簡單對答，背後是如何達成的呢？

- 使用者說："Alexa, ask Greeter to say Hello"
- Echo 說："Hello"

這段對答必須經過這些路徑：

1. Echo 偵測到 wake word: "Alexa"，將語音送往 AVS(Alexa Voice Service)
1. AVS 將語音辨識為文字、解析文字，找到 invocation name
1. 根據 invocation name 找到對映的 Alexa Custom Skill，根據它的 Sample Utterances 解析出 intent 和 slots
1. 將 intent 以 JSON 格式傳送給 Alexa Custom Skill 的 Service Endpoint，通常是一個 lambda
1. 在 lambda 內處理 intent，並回傳一段文字給 Alexa 
1. Alexa 將回應的文字轉為 voice，傳給 Echo
1. Echo 說出回應

語音辨識、解析、判斷 intent 都由 Alexa services 做完了，我們要做的部分其實只有兩個：

1. 設計 intents、slots 與 Sample Utterances
2. 實作收到 intent 的 handlers 程式碼

接下來我們看看一個範例，可以了解如何實作出一個 custom skill。

[Next: Custom Skill 範例](custom-skill-example-bill-man.zh-tw.md)
