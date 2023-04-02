# 角度动态脚本加载+ bootstrap-select，ajaxSelectPicker

> 原文：<https://javascript.plainenglish.io/angular-7-dynamic-script-load-bootstrap-select-ajaxselectpicker-f5677e1eff8d?source=collection_archive---------0----------------------->

![](img/90795c8dc91302df641741227fc35b8f.png)

# 简介:

如果你正在寻找如何在 Angular 应用程序中动态加载 javascript 脚本，那么你就找对了地方，请阅读下一页。

因此，加载脚本的通常方式是通过 *angular.json* 文件，但是它在应用程序启动时加载每个脚本。你可能有一些懒惰加载的模块，当它被加载时需要一些特定的脚本。

# 实施:

在这里你可以找到服务。

# 用法:

添加这个服务后，您可以将它注入到您的组件、效果(如果使用 ngrx store 等)中，并简单地像这样调用它的方法 load

```
**this**.dynamicScriptLoader.load(code);
```

在这个服务中，我使用 ScriptStore 常量来存储 [bootstrap-select](https://github.com/snapappointments/bootstrap-select) 和 [ajaxSelectPicker](https://github.com/truckingsim/Ajax-Bootstrap-Select) 的脚本。

**作者:** [**米罗斯拉夫**](https://medium.com/u/3cf54a8924de?source=post_page-----f5677e1eff8d--------------------------------)