# 在 Node.js 中创建一个 Twitter Bot

> 原文：<https://javascript.plainenglish.io/create-a-twitter-bot-in-node-js-a09137ad2b7b?source=collection_archive---------5----------------------->

![](img/20e9cef92f6dd4676a90eee94bb94c01.png)

Photo by [freestocks.org](https://unsplash.com/@freestocks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一个简单的转发/搜索/回复推特的推特机器人怎么样？我最近做了一个——[来看看](https://twitter.com/checkra1nbot)。构建 twitter 机器人听起来可能是一件大事，但一旦你理解了 JavaScript 和 Node.js 的基础知识，这真的很容易。今天，我们将使用来自 [npm](http://npmjs.org) 的“twit”包来构建一个 twitter 机器人。请注意——您需要参考 [twit 文档](https://www.npmjs.com/package/twit)以获得更深入的参考。

## 入门指南

首先，确保您已经安装了 Node.js(当然)。要在 Linux 上安装它:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

然后

```
nvm install node
```

在新项目中

```
npm init
```

对于所有的提示，只需按回车键(默认)。

要安装 twit:

```
npm install twit
```

现在，我们只需要在[https://www.twitter.com](https://www.twitter.com)上为机器人创建一个账户

为机器人创建一个账户后，前往 https://apps.twitter.com/app/new 为机器人创建一个新的 twitter 开发者账户。

填写表格中的必要字段，然后点击“创建您的 Twitter 应用程序”按钮。创建应用程序后，在导航窗格下查找“密钥和访问令牌”,单击“生成令牌操作”,然后复制:

*   消费者密钥
*   消费者秘密
*   访问令牌
*   访问令牌秘密

创建新的 config.js 文件并添加凭据，如下所示:

```
// config.js
/** TWITTER APP CONFIGURATION
 * consumer_key
 * consumer_secret
 * access_token
 * access_token_secret
 */module.exports = {
  consumer_key: '',  
  consumer_secret: '',
  access_token: '',  
  access_token_secret: ''
}
```

## 建造机器人

在同一个文件夹中，创建一个`index.js`文件。我们需要根据需要用配置文件实例化一个新的 Twit 和 Twitter 对象。

```
var Twit = require('twit');
var Twitter = new Twit(require('./config.js');
```

## 转发功能

让我们创建一个转发东西的函数。为此，我们可以使用 Twitter 对象来转发作为参数传递给函数的 tweet。Twitter 对象(根据文档)转发作为 ID 字符串传递给 post 函数的推文。这意味着 tweet 的 ID 必须作为 post 函数的参数给出。

```
/**
 * Retweets a tweet passed into the function.
 * @param {*} tweet
 */
function retweet(tweet) {
  Twitter.post(
    'statuses/retweet/:id',
    { id: tweet.id_str },
    function(err, data, response) {
      // do something
    });
}
```

这样，在我们传递给 post 函数的函数被调用后，你就可以做一些事情了。被称为回调的函数将被返回:`err, data, response`。这意味着我们可以使用返回的`data`来告诉控制台我们转发了哪条 tweet(可选)。返回的`data`实际上是一个`twitter`对象。

否则，如果 twitter 在转发推文时向我们发送一个错误，我们可以只记录到控制台。

```
/**
 * Retweets a tweet passed into the function.
 * @param {*} tweet
 */
function retweet(tweet) {
  Twitter.post(
    'statuses/retweet/:id',
    { id: tweet.id_str },
    function(err, data, response) {
      if (err) {  // here
        console.log(err.message);
        return;
      }
    }
  );
}
```

## 搜索功能

我们也可以有一个函数搜索某个东西，用搜索后的结果调用另一个函数作为参数传入搜索函数。

```
/**
 * The searching function.
 * @callback callback function which is called after the tweet is returned.
 */
function search(callback) {/**
   * Gets a search key from data.js.
   * @private
   */
  function getSearchKey() {
    let searchKeys = require('./data.js').searchKeys;
    return searchKeys[Math.floor(Math.random() * searchKeys.length)].toLowerCase();
  }Twitter.get(
    'search/tweets',
    { q: getSearchKey(), count: 50, lang: 'en' },
    function(err, data, response) {
      let tweetList = [];
      if (!err) {
        for (let i = 0; i < data.statuses.length; i++) {
          let tweet = data.statuses[i];
          tweetList.push(tweet);
        }
        callback(tweetList);
        return;
      } else {
        console.log(err);
      }
    }
  );
}
```

如果您想搜索某样东西，并从查询中转发一个随机结果，我们可以将 retweet 函数传递给 search 函数，并设置函数执行的时间间隔。

```
function main() {
  search(retweet);
}
```

我们正在调用这个函数，但并没有实际运行它。这意味着我们没有将()括号放在`retweet`函数之后。只有在搜索函数中才会从内部调用。

现在，我们可以在设定的时间间隔后调用 main 函数。

```
main();  // Run the main function once first before running the interval ticks. This is optional.setInterval(main, Math.floor(((Math.random() * 10) + 5) * 1000) * 60);
```

请参考 [twit 文档](https://www.npmjs.com/package/twit#usage),了解更多你可以用 twit 做的事情。

感谢阅读！

*本文原载于*[*blog . raphtlw . rocks*](https://blog.raphtlw.rocks/posts/create-a-twitter-bot-in-node-js/)