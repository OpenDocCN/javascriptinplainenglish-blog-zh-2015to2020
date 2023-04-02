# 顶级 React 库—编辑器、翻译和代码分割

> 原文：<https://javascript.plainenglish.io/top-react-libraries-editor-translation-and-code-splitting-1f8015a727fd?source=collection_archive---------15----------------------->

![](img/7f55bc4edfe96a9b255ec9b6c26ad92c.png)

Photo by [Mae Mu](https://unsplash.com/@picoftasty?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 草稿-js

Draft.js 是一个包，它允许我们向 React 应用程序添加一个富文本编辑器。

我们可以通过运行以下命令来安装它:

```
npm i draft-js
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Editor, EditorState } from "draft-js";export default function App() {
  const [editorState, setEditorState] = React.useState(
    EditorState.createEmpty()
  ); const editor = React.useRef(null); function focusEditor() {
    editor.current.focus();
  } React.useEffect(() => {
    focusEditor();
  }, []); return (
    <div onClick={focusEditor}>
      <Editor
        ref={editor}
        editorState={editorState}
        onChange={editorState => setEditorState(editorState)}
      />
    </div>
  );
}
```

我们添加了`Editor`组件来添加富文本编辑器。

它只会显示一个光标，让我们输入文本。

我们使用`setEditorState`函数在输入时设置编辑器的状态。

# react-i18 接下来

react-i18next 包允许我们向 react 应用程序添加翻译。

要安装它，我们运行:

```
npm i react-i18next i18next-browser-languagedetector react-i18next
```

然后我们可以通过写来使用它:

```
import React from "react";
import i18n from "i18next";
import { useTranslation, Trans } from "react-i18next";import LanguageDetector from "i18next-browser-languagedetector";
import { initReactI18next } from "react-i18next";i18n
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    // we init with resources
    resources: {
      en: {
        translations: {
          "To get started, edit <1>src/App.js</1> and save to reload.":
            "To get started, edit <1>src/App.js</1> and save to reload.",
          "Welcome to React": "Welcome to React and react-i18next",
          welcome: "Hello <br/> <strong>World</strong>"
        }
      },
      de: {
        translations: {
          "To get started, edit <1>src/App.js</1> and save to reload.":
            "Starte in dem du, <1>src/App.js</1> editierst und speicherst.",
          "Welcome to React": "Willkommen bei React und react-i18next"
        }
      }
    },
    fallbackLng: "en",
    debug: true,
    ns: ["translations"],
    defaultNS: "translations",
    keySeparator: false,
    interpolation: {
      escapeValue: false
    }
  });export default function App() {
  const { t, i18n } = useTranslation(); const changeLanguage = lng => {
    i18n.changeLanguage(lng);
  }; const index = 11; return (
    <div className="App">
      <div className="App-header">
        <h2>{t("Welcome to React")}</h2>
        <button onClick={() => changeLanguage("de")}>de</button>
        <button onClick={() => changeLanguage("en")}>en</button>
      </div>
      <div className="App-intro">
        <Trans>
          To get started, edit <code>src/App.js</code> and save to reload.
        </Trans>
        <Trans i18nKey="welcome">trans</Trans>
        <Trans>{index + 1}</Trans>
      </div>
    </div>
  );
}
```

我们在`init`方法中添加了翻译。

此外，我们使用`fallbackLng`选项将备用语言设置为英语。

`ns`设置翻译的名称空间。

`defaultNS`是默认的名称空间。

`interpolation`有插值选项。

`escapeValue`设置为`true`时，对数值进行转义。

然后在`App`中，我们用`changeLanguage`函数通过`i18n.changeLanguage`方法调用来改变语言。

最后，我们用`Trans`组件来添加翻译。

# 可反应加载的

react-loadable 是一个包，让我们可以添加代码分割到我们的应用程序。

要安装它，我们运行:

```
npm i react-loadable
```

然后我们可以通过写来使用它:

`Foo.js`

```
import React from "react";export default () => {
  return <div>foo</div>;
};
```

`App.js`

```
import React from "react";
import Loadable from "react-loadable";const LoadableComponent = Loadable({
  loader: () => import("./Foo"),
  loading: () => <p>loading</p>
});export default function App() {
  return <LoadableComponent />;
}
```

我们有一个`Foo`组件来显示一些东西。

然后在`App.js`中，我们通过从`Foo.js`导入`Foo`组件来创建`LoadableComponent`。

当`Foo.js`正在加载时，`loading`属性有一个组件要显示。

然后我们可以在`App`中使用`LoadableComponent`。

![](img/e2937cf81e057fe613639f357ec1f027.png)

Photo by [Ola Mishchenko](https://unsplash.com/@olamishchenko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

draft-js 让我们创建一个可定制的文本编辑器。

react-loadable 允许我们通过动态加载组件来进行代码拆分。

react-i18next 是一个用于在我们的应用程序中添加翻译的包。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)