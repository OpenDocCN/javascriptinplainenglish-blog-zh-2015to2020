# 使用 Node 开发一个 Twitter 机器人。射流研究…

> 原文：<https://javascript.plainenglish.io/develop-a-twitter-bot-using-node-js-4aae715187a1?source=collection_archive---------8----------------------->

## 了解如何使用 Node.js 创建 Twitter bot，并将其部署到 Heroku 中

![](img/ac96edf713f66ce6045abca9af530620.png)

Photo by [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天在这篇文章中，我们将学习如何使用 **Node.js** 创建一个 Twitter 机器人，并将其部署到 Heroku 中。这个机器人将增强我们在 Twitter 的任务，并能够做一些像转发等任务。这个功能可以通过使用 Twitter API 来实现。

在进入主题之前，让我们看看我们将要遵循的步骤列表。

*   在 Twitter 申请开发者账号。
*   创建应用程序。
*   设置环境。
*   实现代码。
*   避免重复转发。
*   部署。

现在让我们深入正题。

# 在 Twitter 申请开发者账户

这是最重要的一个，也是最困难的一个。

*   登录 Twitter。
*   去开发者控制台申请一个开发者账号。
*   选择您要创建的应用类型。
*   提及你的应用程序的目的。

在申请之前，请确保您已经阅读了所有的开发商协议和政策。不符合这些条件将导致你的申请被拒绝。

# 创建应用程序

一旦你的账户被 Twitter 批准。转到您的开发人员控制台。

*   在 apps.twitter.com 创建应用程序
*   填写所有必需的详细信息。
*   生成唯一的 API 密钥。
*   进入应用详情，导航至**密钥和令牌**。
*   对您的 API 密钥保密。

# 设置环境

首先使用各自的命令在你的电脑上安装 N **ode.js** 和 **npm** 。

创建一个新目录， **your-** **bot-name** 。

初始化 git 环境，并使用 **npm** 安装所有依赖项 **twit** 。在您的 bot 目录中输入以下命令。

```
$ git init
$ npm init
$ npm install twit
```

现在成功设置我们的开发环境。

# 实现代码

首先，我们必须认证**Twitter**，并使用我们之前生成的 API 密钥链接 Twitter 应用程序和代码。

创建一个名为 **config.js** 的新文件，并将代码粘贴到其中。

```
module.exports = {
consumer_key: APPLICATION_CONSUMER_KEY_HERE,
consumer_secret: APPLICATION_CONSUMER_SECRET_HERE,
access_token: ACCESS_TOKEN_HERE,
access_token_secret: ACCESS_TOKEN_SECRET_HERE
}
```

从 Twitter 仪表板应用您的唯一键。

现在让我们用 **bot.js** 为我们的机器人写一段代码。

```
const config = require('./config')
const twit =  require('twit')

const T = new twit(config)

function retweet(searchText) {
    // Params to be passed to the 'search/tweets' API endpoint
    let params = {
        q : searchText + '',
        result_type : 'mixed',
        count : 25,
    }

    T.get('search/tweets', params, function(err_search, data_search, response_search){

        let tweets = data_search.statuses
        if (!err_search)
        {
            let tweetIDList = []
            for(let tweet of tweets) {
                tweetIDList.push(tweet.id_str);

                //more code here later...
            }

            // Call the 'statuses/retweet/:id' API endpoint for retweeting EACH of the tweetID
            for (let tweetID of tweetIDList) {
                T.post('statuses/retweet/:id', {id : tweetID}, function(err_rt, data_rt, response_rt){
                    if(!err_rt){
                        console.log("\n\nRetweeted! ID - " + tweetID)
                    }
                    else {
                        console.log("\nError... Duplication maybe... " + tweetID)
                        console.log("Error = " + err_rt)
                    }
                })
            }
        }
        else {
            console.log("Error while searching" + err_search)
            process.exit(1)
        }
    })
}

// Run every 60 seconds
setInterval(function() { retweet('#DataScience OR #DataVisualization'); }, 60000)
```

我们使用我们的配置初始化了 twit 对象。

我们将参数传递给搜索 API

*   **问** -搜索查询。
*   **result_type** -热门或老的推文。
*   **计数** -检索到的推文数量。

**JSON** 对象用于检索所有推文，并将每个推文传递给 **status/retweet/:id**

现在使用以下命令在本地测试 bot。

```
node bot.js
```

# 避免重复转发

在 **bot.js** 的 for 循环中添加以下代码

```
if(tweet.text.startsWith("RT @")) {
 if(tweet.retweeted_status) {
    tweetIDList.push(tweet.retweeted_status.id_str);
 }
 else {
    tweetIDList.push(tweet.id_str);
 }
}
else {
    tweetIDList.push(tweet.id_str);
}
```

然后在循环之外加上这个。

```
/ Utility function - Gives unique elements from an array
function onlyUnique(value, index, self) { 
    return self.indexOf(value) === index;
}

// Get only unique entries
tweetIDList = tweetIDList.filter( onlyUnique )
```

*   每条推文都有唯一的 id， **id_str。**
*   每条转发都有不同的 **id_str** 。
*   搜索 API 用于查找它是原创的还是转发的。
*   这将避免我们的机器人重复。

# 部署

首先在 heroku 中创建一个帐户，然后使用“您的僵尸名称”创建一个应用程序名称。

保持本地目录文件名和 heroku 项目名相同。

安装 heroku-cli 并使用以下命令创建一个“Procfile”。

```
worker: node bot.js
```

登录 heroku CLI。

```
$ heroku login
```

在部署之前，转到 heroku dashboard，启动 worker dyno 并对其进行配置。

使用以下命令部署到 heroku。

```
$ heroku git:remote -a your-botname
$ git add
$ git commit -am "Add code"
$ git push heroku master
```

# 结论

我希望你喜欢它，并学习了如何使用 node.js 创建一个 twitter 机器人，并使用 heroku 部署它。

感谢阅读！

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**