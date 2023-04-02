# 用 5 个步骤学会在 React 中使用本地化

> 原文：<https://javascript.plainenglish.io/learn-to-use-localization-in-react-in-5-steps-fa65c2313083?source=collection_archive---------8----------------------->

## 本地化反应仅需 5 分钟

![](img/2d4ffcaffb3846cd699516c21f950df1.png)

**第一步**

安装以下节点包:-

```
npm install --save i18next-browser-languagedetector i18next react-i18next
```

**第二步**

在 src 文件夹的根目录下添加 i81n.js 文件。

```
import i18n from 'i18next';
import LanguageDetector from 'i18next-browser-languagedetector';i18n.use(LanguageDetector).init({
 // we init with resources
 resources: {
  // here english is our base language
  en: {
   translations: {
    text1: 'text1', // english:english
    text2: 'text2',
   },
  },
  jap: {
   translations: {
    text1: '前書き', // english:chinese
    text2: 'ムワークです',
   },
  },
 },
 fallbackLng: 'en',
 debug: true,// have a common namespace used around the full app
 ns: ['translations'],
 defaultNS: 'translations',keySeparator: false, // we use content as keysinterpolation: {
  escapeValue: false, // not needed for react!!
  formatSeparator: ',',
 },react: {
  wait: true,
 },
});export default i18n;
```

**第三步**

在您的根 index.js 文件中导入以下模块

```
import { I18nextProvider } from "react-i18next";
import i18n from "./i18n";
..... rest of file
```

**第四步**

现在添加一个提供者包装或者只是将 i81n 作为道具传递给你的组件，比如:-

```
<Landing *i18n*={i18n}/>
```

或者

```
<I18nextProvider i18n={i18n}>
<Landing/>
<I18nextProvider/>
```

**第五步**

现在我们在组件中有了 props，它有“t”方法，所以只需像这样使用它:-

```
{this.props.i18n.t("Introduction")} // just pass the translation property
```

要更改应用程序中的语言，我们只需要使用

```
{props.i18n.changeLanguage('jap');} // just pass the language name
```

恭喜你已经学会了如何在 5 个步骤中使用本地化。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！为我们的新出版物献上一点爱心吧，请跟随他们:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****