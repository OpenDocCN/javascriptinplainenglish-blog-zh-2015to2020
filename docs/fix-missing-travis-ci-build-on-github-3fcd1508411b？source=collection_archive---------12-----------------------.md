# 如何修复 GitHub 上的“丢失 Travis CI 构建”

> 原文：<https://javascript.plainenglish.io/fix-missing-travis-ci-build-on-github-3fcd1508411b?source=collection_archive---------12----------------------->

![](img/386905f87e74ac2ef4b9848723c1345f.png)

# 背景

[Travis CI](https://travis-ci.org/) 不再出现在我的 [GitHub](https://github.com/) PR(拉取请求)中。我所做的是重新设置 Travis CI 应用程序对我的 GitHub 帐户的访问，以使构建再次显示出来。

# 撤销访问权限

打开 [**设置**](https://github.com/settings/profile) :

![](img/cfc492b156c90a2bb6e1f7b2b801738c.png)

然后进入 [**申请**](https://github.com/settings/applications) > **授权 OAuth 申请**:

![](img/49f61e4aa39dce7f7ac939aa26ea77ed.png)

从 **Travis CI for Open Source** 下拉列表中点击 **Revoke** :

![](img/15fa46c6c48d39f7d577efa33f7e2019.png)

# 重新授权申请

注销 [Travis CI](https://travis-ci.org/) ，重新登录，并重新授权应用程序访问 GitHub 帐户:

![](img/9b8fb4eed2b425c1c170455787bd58a1.png)

打开一个新的 PR 后，我可以看到 Travis CI 构建在**合并拉取请求**按钮旁边。

[*本文原载于《remarkablemark.org》2020 年 11 月 14 日。*](https://b.remarkabl.org/32N8CkC)