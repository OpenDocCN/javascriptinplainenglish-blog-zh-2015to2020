# JavaScript 项目中的 DIY 多语言

> 原文：<https://javascript.plainenglish.io/diy-multilanguage-in-javascript-project-65f8aa91a506?source=collection_archive---------5----------------------->

## java 描述语言

## 让我们用这个简单的方法来国际化我们的 webapp

![](img/60d75768882e9585ec09177ca3f0a6c8.png)

Photo by [Artem Beliaikin](https://unsplash.com/@belart84?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们开发一个想要在全球范围内发布的 webapp 时，我们必须考虑到很多人不会说我们的语言。如果他们不理解我们，我们可能会失去一大块用户。

在我从事 JavaScript 项目的经验中，我尽可能以最简单的方式改进了管理多种语言的过程，在这种方式下，我可以在更短的时间内添加新的语言或编辑现有的字符串，并且错误更少。

# 如何管理不同的语言

在本教程中，我们将学习如何创建一个插件，带来一个语言文件，解析它，并返回所需的字符串在用户的语言。

语言文件(在我们的例子中称为 *strings.js* )是一个默认导出的 JavaScript 对象。该对象可能具有与具有相同结构的其他对象相同的属性。这是我们 *strings.js* 的一个例子:

```
**import** months **from** "./months";**export default** {
  **account**: {
    en: "Account",
    es: "Usuario",
    it: "Utente"
  },
  **appName**: "My Project",
  **cancel**: {
    en: "Cancel",
    es: "Cancelar",
    it: "Annulla"
  },
  **months**
};
```

而这就是导入文件 *months.js* 的内容:

```
**export default** {
  **january**: {
    en: "January",
    es: "Enero",
    it: "Gennaio"
  },
  **february**: {
    en: "February",
    es: "Febrero",
    it: "Febbraio"
  },
  **march**: {
    en: "March",
    es: "Marzo",
    it: "Marzo"
  },
  **april**: {
    en: "April",
    es: "Abril",
    it: "Aprile"
  },
  **may**: {
    en: "May",
    es: "Mayo",
    it: "Maggio"
  },
  **june**: {
    en: "June",
    es: "Junio",
    it: "Giugno"
  },
  **july**: {
    en: "July",
    es: "Julio",
    it: "Luglio"
  },
  **august**: {
    en: "August",
    es: "Agosto",
    it: "Agosto"
  },
  **september**: {
    en: "September",
    es: "Septiembre",
    it: "Settembre"
  },
  **october**: {
    en: "October",
    es: "Octubre",
    it: "Ottobre"
  },
  **november**: {
    en: "November",
    es: "Noviembre",
    it: "Novembre"
  },
  **december**: {
    en: "December",
    es: "Diciembre",
    it: "Dicembre"
  }
};
```

# lang 插件

因此，为了将我们的字符串以所选语言交付给用户，我们必须在一个新文件 *lang.js* 中创建一个函数，我们称之为 *getStrings* 。该函数将解析之前声明的 *strings.js* 对象，并返回一个只包含所选语言字符串的对象。例如，如果我选择“it”(意大利语)，返回的对象将是:

```
{
  **account**: "Utente"
  **appName**: "My Project",
  **cancel**: "Annulla"
  **months**: {
    **january**: "Gennaio",
    **february**: "Febbraio",
    **march**: "Marzo",
    **april**: "Aprile",
    **may**: "Maggio",
    **june**: "Giugno",
    **july**: "Luglio",
    **august**: "Agosto",
    **september**: "Settembre"
    **october**: "Ottobre",
    **november**: "Novembre",
    **december**: "Dicembre"
  }
}
```

这是 *getStrings* 函数，它需要语言对象和所选语言作为输入参数:

```
**const** getStrings = (*string*, *lang*) => {
  **let** res = {}; **Object**.entries(string).forEach(([*key*, *value*]) =>
    res[*key*] = !value[*lang*] && **typeof** value === **"object"**
      ? getStrings(value, lang)
      : value[*lang*] || value
  ); **return** res;
};
```

我们遍历对象属性，如果值是一个字符串或包含所选语言作为属性的对象，我们就返回它。

否则，我们调用 *getStrings* 函数(递归地)将值作为输入传递，当然也是所选的语言。

# 检索当前用户语言并在项目中使用

大多数时候，用户不会手动选择他们的语言，因为我们可以从浏览器设置中获得。为此，我们将下面几行添加到 *lang.js* 中:

```
**const** langs = [
  "en",
  "es",
  "it"
];**let** curLang = **navigator.language**.split(**"-"**)[0];
!langs.includes(curLang) && (curLang = **"en"**);
```

使用*navigator . language*native API，我们可以获得主要用户语言，如果没有找到，我们默认为“en”。可用的语言在 ISO 639–1 标准中定义。

然后，在文件的结尾，我们导出返回的对象:

```
**import** strings **from** "./strings";// previous code**export default** getStrings(strings, curLang);
```

现在，您已经准备好在您的项目中使用它了:只需导入 *lang.js* 文件，并以如下方式选择字符串:

```
**import** _s **from** "./lang";document.write(_s.cancel); // prints Annulla
document.write(_s.month.september); // prints Settembre
```

*lang.js* 的完整代码:

```
**import** strings **from** "./strings";**const** langs = [
  "en",
  "es",
  "it"
];**let** curLang = **navigator.language**.split(**"-"**)[0];
!langs.includes(curLang) && (curLang = **"en"**);**const** getStrings = (*string*, *lang*) => {
  **let** res = {}; **Object**.entries(string).forEach(([*key*, *value*]) =>
    res[*key*] = !value[*lang*] && **typeof** value === **"object"**
      ? getStrings(value, lang)
      : value[*lang*] || value
  ); **return** res;
};**export default** getStrings(strings, curLang);
```

# 提示:向字符串添加变量

如果我们想在字符串中添加变量，我们可以创建一个新函数并导出它。例如，我们称之为*格式字符串*:

```
**export const** formatString = (*s*, *variables*) => {
  **const** regex = new RegExp(
    '{(' + Object.keys(variables).join('|') + ')}',
    'g'
  ); **return** s.replace(*regex*, (*m*, *i*) => variables[i] || m);
};
```

要使用它，我们必须执行以下步骤:

1.  将字符串添加到 *strings.js* 中；例如`greetings: "Hi, {username}"`
2.  导入 *formatString* 函数，使用方法如下:`formatString(_s.greetings, {username: "Andrea"})`

# 结论

如果你对这篇文章有任何问题，或者你想提出修改建议，欢迎写评论或者直接联系我。

一如既往，我为我的英语道歉，但这不是我的母语。

谢谢大家，关注我更多文章！

**以下是我的一些其他文章:**

[](https://medium.com/javascript-in-plain-english/how-to-create-a-router-in-svelte-ce66c10275fe) [## 如何创建一个苗条的路由器

### 我们可以避免不必要的包装

medium.com](https://medium.com/javascript-in-plain-english/how-to-create-a-router-in-svelte-ce66c10275fe) [](https://medium.com/javascript-in-plain-english/create-reusable-svelte-components-for-your-page-layouts-9593b1a60b08) [## 为你的页面布局创建可重用的苗条组件

### 了解如何构建一个可以跨页面使用的组件

medium.com](https://medium.com/javascript-in-plain-english/create-reusable-svelte-components-for-your-page-layouts-9593b1a60b08) 

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**