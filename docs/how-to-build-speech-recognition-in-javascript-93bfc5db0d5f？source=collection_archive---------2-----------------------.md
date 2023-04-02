# 用 JavaScript 构建语音识别

> 原文：<https://javascript.plainenglish.io/how-to-build-speech-recognition-in-javascript-93bfc5db0d5f?source=collection_archive---------2----------------------->

## 了解如何使用 TensorFlow.js 构建语音识别。

![](img/c39d28833cada5c6dfeda103973da7c6.png)

Photo by [Clay Banks](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天在本文中，我们将讨论如何使用 **TensorFlow.js** 构建一个语音识别工具。实际上，当我们要构建人工智能、深度学习、机器学习时，我们经常会想到使用 Python 语言。但是使用 JavaScript 呢？它是一种强大的语言，我们可以利用一个叫做**的框架**

在深入讨论这个话题之前，让我们来看看关于 **TensorFlow.js** 的讨论。

**TensorFlow.js** 是谷歌开发的一个 JavaScript 库，用于在浏览器和 **Node.js** 中开发和部署机器学习项目。

**TensorFlow.js** 不仅仅是一个库。这并不意味着您不应该使用 TensorFlow.js，但我认为它是一个部署和开发 ML 应用程序的很好的库，让我们在本文的其余部分中了解一下。

# 使用 TensorFlow.js 开发项目

正如我之前所说，它是一个强大的库，我们可以使用许多不同的功能，如图像捕获、视频处理和语音识别。今天我决定做一个基本的语音识别项目。

在这段代码中，我们可以通过麦克风收听并识别用户在说什么，最多几句话，因为我们对这个示例模型有一些限制。

我们需要做的第一步是安装库。为了安装 **TensorFlow.js，**有几个选项。在我们的项目中，我们可以从 CDN 导入它。

```
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@2.0.0/dist/tf.min.js"></script>
<script src="https://unpkg.com/@tensorflow-models/speech-commands"></script>
```

然后添加一些 HTML 来显示一些单词列表。

```
<div class="demo">
    <div>
        <label class="form-switch">
            <input type="checkbox" id="audio-switch">
            Microphone
        </label>
        <div id="demo-loading" class="hidden">Loading...</div>
    </div>
    <div id="sp-cmd-wrapper" class="grid"></div>
</div>
```

它包含一个复选框、一个加载元素和一个用于呈现单词列表的包装元素，所以我们接下来要做的是:

```
const wrapperElement = document.getElementById('sp-cmd-wrapper');
for (let word of wordList) {
    wrapperElement.innerHTML += `<div id='word-${word}'>${word}</div>`;
}
```

我们需要点击麦克风复选框，这将触发事件监听器开始监听测试演示工作条件的过程。

```
document.getElementById("audio-switch").addEventListener('change', (event) => {
    if(event.target.checked) {
        if(modelLoaded) {
            startListening();
        }else{
            loadModel();
        }
    } else {
        stopListening();
    }   
});
```

我们有 3 种不同的场景，当用户启用复选框而模型未被加载时，在这种场景中，我们使用 **loadModel()** 函数。当模型已经被加载时，我们触发监听过程。当用户禁用该复选框时，我们停止监听麦克风。

# loadModel()

它负责创建一个实例并加载模型。当加载模型时，我们需要得到模型已经用**识别器训练过的标签列表。**

```
async function loadModel() { 
    // Show the loading element
    const loadingElement = document.getElementById('demo-loading');
    loadingElement.classList.remove('hidden');

    recognizer = speechCommands.create("BROWSER_FFT");  
    await recognizer.ensureModelLoaded()

    words = recognizer.wordLabels();
    modelLoaded = true;

    // Hide the loading element
    loadingElement.classList.add('hidden');
    startListening();
}
```

# 开始监听()

**startListening()** 用于在模型加载后或用户启用麦克风时调用。它负责访问麦克风 API，该 API 将评估模型，以了解我们能够识别哪个单词。

```
function startListening() {
    recognizer.listen(({scores}) => {

        scores = Array.from(scores).map((s, i) => ({score: s, word: words[i]}));

        scores.sort((s1, s2) => s2.score - s1.score);
        const elementId = `word-${scores[0].word}`;
        document.getElementById(elementId).classList.add('active');

        // This is just for removing the highlight after 2.5 seconds
        setTimeout(() => {
            document.getElementById(elementId).classList.remove('active');
        }, 2500);
    }, 
    {
        probabilityThreshold: 0.70
    });
}
```

# 停止列表()

**停止收听()**用于停止访问麦克风及其评估。

```
function stopListening(){
    recognizer.stopListening();
}
```

# 最终部署

现在，您需要在 web 上构建您的第一个语音识别模型。

```
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@2.0.0/dist/tf.min.js"></script>
<script src="https://unpkg.com/@tensorflow-models/speech-commands"></script>

<script type="text/javascript">
    let recognizer;
    let words;
    const wordList = ["zero","one","two","three","four","five","six","seven","eight","nine", "yes", "no", "up", "down", "left", "right", "stop", "go"];
    let modelLoaded = false;

    document.addEventListener('DOMContentLoaded', () => {
        const wrapperElement = document.getElementById('sp-cmd-wrapper');
        for (let word of wordList) {
            wrapperElement.innerHTML += `<div class='col-3 col-md-6'><div id='word-${word}' class='badge'>${word}</div></div>`;
        };

        document.getElementById("audio-switch").addEventListener('change', (event) => {
            if(event.target.checked) {
                if(modelLoaded) {
                    startListening();
                }else{
                    loadModel();
                }
            } else {
                stopListening();
            }   
        });
    });

    async function loadModel() { 
        // Show the loading element
        const loadingElement = document.getElementById('demo-loading');
        loadingElement.classList.remove('hidden');

        recognizer = speechCommands.create("BROWSER_FFT");  
        await recognizer.ensureModelLoaded()

        words = recognizer.wordLabels();
        modelLoaded = true;

        // Hide the loading element
        loadingElement.classList.add('hidden');
        startListening();
    }

    function startListening() {
        recognizer.listen(({scores}) => {
            scores = Array.from(scores).map((s, i) => ({score: s, word: words[i]}));

            // After that we sort the array by score descending
            scores.sort((s1, s2) => s2.score - s1.score);

            // And we highlight the word with the highest score
            const elementId = `word-${scores[0].word}`;
            document.getElementById(elementId).classList.add('active');

            // This is just for removing the highlight after 2.5 seconds
            setTimeout(() => {
                document.getElementById(elementId).classList.remove('active');
            }, 2500);
        }, 
        {
            probabilityThreshold: 0.70
        });
    }

    function stopListening(){
        recognizer.stopListening();
    }
</script>

<div class="demo">
    Please enable the microphone checkbox and authorize this site to access the microphone.
    <br />
    Once the process finished loading speak one of the word bellow and see the magic happen.
    <br /><br />
    <div>
        <label class="form-switch">
            <input type="checkbox" id="audio-switch">
            Microphone
        </label>
        <div id="demo-loading" class="hidden">Loading...</div>
    </div>
    <div id="sp-cmd-wrapper" class="grid"></div>
</div>
```

# 结论

我希望你会喜欢，无论你在这篇文章中学到了什么，都只是生成结果的几行代码。但是 TensorFlow.js 是一个强大的开发机器学习模型的库。

感谢阅读！