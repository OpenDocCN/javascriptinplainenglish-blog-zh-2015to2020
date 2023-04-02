# 如何使用 NodeJS 和 Commander.js 创建您的命令行程序(CLI)

> 原文：<https://javascript.plainenglish.io/how-to-create-a-command-line-npm-module-cli-using-commander-js-1073e616aee7?source=collection_archive---------7----------------------->

![](img/b6980259e68b1577c5f6901194fcd989.png)

Create your own command line

这篇文章将向你展示如何使用 Commander.js 模块创建命令行 npm 模块(CLI)。

Commander.js 是一个非常受欢迎的模块，可以让你创建自己的 CLI 程序。

首先，开始您的新项目——假设我的项目名称是“json-now”

```
$ git clone https://github.com/yourname/json-now.git
$ cd json-now
```

现在，创建您的 package.json 文件:

```
{
  "name": "json-now",
  "version": "0.0.1",
  "bin": {
    "json-now": "./bin/index.js"
  },
  "dependencies": {
    "commander": "^3.0.1"
  }
}
```

然后，安装依赖项:

```
$ npm install
```

“bin”部分指定您的命令行名称。如您所见，继续创建一个包含“index.js”文件的“bin”目录:

```
#!/usr/bin/env nodeconst program = require('commander');
const ver = require('../lib/ver');program
  .usage('[options] <file>')
  .option('-v, --version', 'show version', ver, '')
  .option('-p, --port <port>', 'use custom port')
  .option('-f, --flag', 'boolean flag', false)
  .action((file, options) => {
    console.log('file name: ', file);
    // more hanlder: require('../lib/moreHandler')(options);
  })
  .parse(process.argv);
```

让我们创建第一个名为“-v”或“-version”的选项，它显示版本号。创建一个名为“lib”的目录和一个新文件“ver.js ”:

```
const package = require('../package.json')module.exports = () => {
    console.log(package.version);
};
```

到目前为止，它看起来直截了当。您创建了一个 commander“程序”，它通过运行“ver.js”来处理类似“-v”的选项

打开终端并试用它:

```
$ node bin/index.js -v
0.0.1$ node bin/index.js sample.json
file name: sample.json
```

现在，是时候发布您的命令行供全世界使用了！

```
$ npm login
$ npm publishTry out your shiny new command:$ npm install json-now -g
$ json-now -v
```

上述代码位于此处，供您参考:

 [## ngduc/json-now

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/ngduc/json-now)