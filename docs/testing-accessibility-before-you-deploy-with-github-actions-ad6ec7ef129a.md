# 在使用 GitHub 操作部署之前测试可访问性

> 原文：<https://javascript.plainenglish.io/testing-accessibility-before-you-deploy-with-github-actions-ad6ec7ef129a?source=collection_archive---------6----------------------->

## 如何为你的 Jekyll 网站编写测试

![](img/82f44a2edd25f17b49541f8290bb728e.png)

Photo by [Paul Green](https://unsplash.com/@pgreen1983?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在离开前端开发几个月后，我发现了一个需要解决的问题。如果你读过我关于测试的另一个故事，你会知道这是我在工作中经常思考的事情。编写 Web 组件在部署代码之前创建要运行的测试是很简单的。有人问我，在部署你的网站之前，如何为你的 Jekyll 网站编写可访问性测试。幸运的是，我们可以使用 GitHub 动作来执行您的测试，并在您失败时暂停构建。

![](img/1bd28da647b05c828914cbf81603f056.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## GitHub 操作

什么是 GitHub 动作？来自 GitHub 网站

> GitHub Actions 现在拥有世界一流的 CI/CD，可以轻松实现所有软件工作流程的自动化。直接从 GitHub 构建、测试和部署您的代码。按照您想要的方式进行代码审查、分支管理和问题分类。

GitHub Actions 是你的 CI/CD。操作使用 YAML 文件来定义 CI/CD 管道的步骤。在我们的 YAML 文件中，我们将定义我们的管道。我们想做的步骤是

*   下载代码
*   将我们下载代码的权限更改为 777
*   NPM 安装
*   建立一个 Jekyll 网站
*   进行我们的测试。
*   继续我们的 CI/CD

我已经创建了一个 [GitHub 回购](https://github.com/jcdalton2201/jekyll-accessibility-test)来展示这将如何工作。YAML 文件中的步骤目录下的 blank.yml。github/workflows/blank.yml。

```
steps:# Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it- uses: actions/checkout@v2- name: update permissionsrun: |cd ..sudo chmod -R 777 .# NPM installs- name: NPM installsrun: npm install# start up a jekyll server- name: Build the stackrun: docker-compose up -d# Test and show the container running- name: Show running containersrun: docker ps -a#Sleep for 30s to give Jekyll a chance to spin up- name: Sleep for 30 secondsuses: jakejarvis/wait-action@masterwith:time: '30s'#start the tests- name: Test is siterun: npm run test
```

第一行是我们对 Jekyll 网站的检查。下一步是更新我们的签出目录的权限。我们必须更改权限，因为我们的 Jekyll 版本在构建时会更新目录。下一步是安装 NPM。在这一点上，一切都应该是直截了当的。下一步是事情变得有趣的地方。

## 码头工人

![](img/24b789183d9acf278f283e3b50dd0a1a.png)

Photo by [Jonas Smith](https://unsplash.com/@jonassmith?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

GitHub Actions 在 Docker 容器中运行。他们使用的 Docker 容器上安装了 Docker。它允许我们在 GitHub 内部运行一个单独的 Docker 容器。当我们运行测试时，我们需要能够这样做来运行我们自己的 Jekyll 服务器。

我用启动时启动的 Jekyll 服务器创建了一个 Docker 映像。我会推荐建立你的哲基尔形象，但这里有一个是我做的[jcdalton 2201/Jekyll-ci-CD](https://hub.docker.com/r/jcdalton2201/jekyll-ci-cd)。我们现在可以使用 YAML 文件中的 docker-compose 命令启动容器，并使用-d 属性在后台运行。如果需要的话，我们也可以对一个简单的 web 服务器这样做。

## 操纵木偶的人

![](img/fc834f2b56c7d22bcefbfc04c6b8cb5d.png)

Photo by [pixpoetry](https://unsplash.com/@blackpoetry?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最后一步是运行我们的测试。我使用 Jasmine、木偶师和 Axe-Core 进行测试。如果你看我的 GitHub repo，你会在 spec 目录中找到我的测试。如果你过去用过木偶戏，这些测试是基本的。您创建了您的木偶页面，并在容器中访问您的网站。

```
await page.goto(‘[http://localhost:4000/',
  {
    waitUntil](http://localhost:4000/',{waitUntil): ‘networkidle0’
  }
);
```

networkidle0 将一直等到页面停止加载。然后我用下面的代码运行 Axe。

```
const results = await new AxePuppeteer(page).include(‘body’).analyze();expect(axeUtils.isValid(results)).toBeTruthy();
```

我们刚刚在车身上运行了 Axe 进行可访问性测试。

现在，我们可以在将静态网站部署到环境中之前测试它的可访问性。我希望这个例子能帮助团队在测试中增加可访问性，并在它影响到任何环境之前完成。