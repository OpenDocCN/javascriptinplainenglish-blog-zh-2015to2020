# React 应用程序的依赖注入设置

> 原文：<https://javascript.plainenglish.io/a-dependency-injection-setup-for-react-applications-af9c67fa5e9c?source=collection_archive---------7----------------------->

## 如何配置 React 应用程序使用 InversifyJS 在组件中注入服务

![](img/afa1b036033a78a4388aede312f12a7e.png)

Photo by [Erik Eastman](https://unsplash.com/@erikeae?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我想如果你在这里，你知道什么是依赖注入。在某些情况下，我对前端开发感到困惑。如果技术上不需要，为什么要使用依赖注入？我可以在组件中导入“服务”，瞧，它可以调用 API 或在存储上写入。客户端是“平凡的”，它只显示数据、灯光和颜色:)

对于小型应用程序来说，这可能是一个很有价值的方法，但是当你的应用程序需要增长很多年的时候，当你试图测试你的代码的时候，问题就来了。在某一点上，您将开始这样做，因为大型代码库需要测试来减轻问题和手工 QA 部门的压力(如果有的话)，以实现连续交付或在发布前的 UAT 中。引入依赖注入允许你分离代码，提高可测试性，当然，增加组件的组织，使你能够更好地扩展或改变它们。

React 不像 Angular 那样提供现成的依赖注入。于是，我自问如何让 React 能够实现依赖注入。

# 应用程序设置

让我们从使用 [create-react-app](https://create-react-app.dev/) 创建一个新的 React 应用程序开始:

```
npx create-react-app **my-app** --template **typescript**oryarn create react-app **my-app** --template **typescript**cd **my-app**
```

现在，我们需要安装 [InversifyJS](http://inversify.io/) :

```
npm install **inversify reflect-metadata**oryarn add **inversify reflect-metadata**
```

之后，只需编辑 *tsconfig.json* 使其适应 inversify。只需在*编译器选项*中添加这两行:

```
"**experimentalDecorators**": true,
"**emitDecoratorMetadata**": true
```

InversifyJS 使用 decorators 工作，我们只需要告诉 TypeScript 识别并与代码一起发出这些信息。

要检查我们的设置是否有效，只需使用以下命令启动应用程序:

```
npm startoryarn start
```

# 依赖注入设置

如果应用程序像预期的那样工作，并且应该如此，我们就可以开始将注意力转移到依赖注入上。

要知道如何使用 Inversify，请查看它的文档。对于我们的目的，我们只需要一个简单的配置。

Inversify 使用容器来注册服务。之后，可以使用同一个容器对象创建或获取服务。与其他 DI 工具没有什么不同。所以，让我们添加一个简单的服务。转到 *src* 文件夹，创建一个新的 TS 文件 *date-string.service.ts* :

```
**export class** DateStringService {
  **constructor**(
    **private** prefix: **string** ) {}

  getDateString() {
    **return** `${**this**.prefix}: ${(**new** Date()).toISOString()}`;
  }
}
```

**DateStringService** 只是在构造函数中接受一个前缀，并返回当前日期，其前缀是在创建过程中指定的字符串。

让我们设置容器。在 *src/index.ts* 中，您应该有类似于以下代码的内容:

```
**import** React **from** 'react';
**import** ReactDOM **from** 'react-dom';
**import** './index.css';
**import** *App* **from** './App';
**import** *reportWebVitals* **from** './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
*reportWebVitals*();
```

只需添加一个容器配置:

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import *App* from './App';
import { DateStringService } from './date-string.service';
import *reportWebVitals* from './reportWebVitals';
**import { Container } from 'inversify';**

**const container = new Container();
container.bind(DateStringService)
   .toConstantValue(new DateStringService('today is'));**

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
*reportWebVitals*();
```

完成了。现在，您的容器能够创建一个新的 *DateStringService* 并在需要时提供它。

# 解决服务问题

我们需要做的最后一步是建立在组件内部解析服务的能力。为此，我们创建了一个特定的绑定。让我们创建一个名为 *react-binding.tsx* 的新文件，其中包含以下代码:

```
**import** { Container, interfaces } **from** 'inversify';
**import** React, { *useContext* } **from** 'react';

// Defines the context to support the container.
**interface** ContainerContext {
  container: Container | **null**;
}

*/**
 * Defines the context to use Inversify
 * container to resolve dependencies.
 */* **const** InversifyContext = React.*createContext*<ContainerContext>(
  { container: **null** },
);

// Defines props for the provider.
**interface** ContainerProviderProps {
  container: Container;
}

*/**
 * Defines the React component to provide
 * the container to the child components.
 */* **export const** *ContainerProvider*: React.FC<ContainerProviderProps> = (
  { container, children },
) => (
  <InversifyContext.Provider value={{ container }}>
    {children}
  </InversifyContext.Provider>
);

*/**
 * Defines the hook used to resolve a dependency in a component.
 */* **export function** *useInjection*<T>(identifier: interfaces.ServiceIdentifier<T>): T {
  **const** { container } = *useContext*(InversifyContext);
  **if** (!container) {
    **throw new** Error('The container should not be null');
  }
  **try** {
    **return** container.get<T>(identifier);
  } **catch** (e) {
    **return** container.resolve<T>(identifier **as** interfaces.Newable<T>);
  }
}
```

这一小段代码是注射过程的核心。

*   **InversifyContext** 是将通过组件层次结构传递的上下文，以便容器本身在任何地方都可用。
*   **ContainerProvider** 是一个 UI 容器，它将被用作应用程序组件的包装器，以便为任何子组件提供上下文。
*   **useInjection** 是一个 an hoc，它将被用来获取上下文(因此反转容器)以获取或解析指定为参数的服务(关于*获取*和*解析*之间的区别，请参见 [InversifyJS](https://github.com/inversify/InversifyJS/blob/master/wiki/container_api.md#containerresolveconstructor-newable) )。

现在我们必须再次编辑 *index.tsx* 来用全新的 ContainerProvider 包装应用程序组件:

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import *App* from './App';
import { DateStringService } from './date-string.service';
import { *ContainerProvider* } from './react-binding';
import *reportWebVitals* from './reportWebVitals';
import { Container } from 'inversify';

const container = new Container();
container.bind(DateStringService).toConstantValue(new DateStringService('today is'));

ReactDOM.render(
  <React.StrictMode>
 **<ContainerProvider container={container}>
      <App />
    </ContainerProvider>**
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
*reportWebVitals*();
```

所以，现在在应用程序组件中:

```
import React from 'react';
**import { DateStringService } from './date-string.service';**
import logo from './logo.svg';
import './App.css';
**import { *useInjection* } from './react-binding';**

function *App*() {
 **// Gets the service.
  const dateStringService = *useInjection*(DateStringService);**

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.tsx</code> and save to reload.
        </p>
 **<p>
          {dateStringService.getDateString()}
        </p>**
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default *App*;
```

您可以通过启动应用程序来检查是否一切都如我们所期望的那样工作，您应该会看到一个新的行，显示从服务获得的字符串。

# 结论

我们设置了一个简单的依赖注入，即使我们没有关注 InversifyJS 特性。无论如何，这个简单的方法允许您在您的反应应用程序中有一个可靠的 DI 层。通过研究文档，您可以得到一个非常稳定和可扩展的设计来处理 InversifyJS 容器配置。在我的项目中，我通常为一个模块创建模块和注册服务，以增加代码解耦。也许，在我未来的一个故事中，我将深入探讨这种设置的模块化结构。

**请记住，这种方法不属于 Inversify，您可以将其与其他库一起使用，例如 typedi。**