# 你应该从今天开始使用的 21 个 React UI 组件库

> 原文：<https://javascript.plainenglish.io/21-react-ui-component-libraries-you-should-start-using-from-today-6249758d188?source=collection_archive---------1----------------------->

## 不要浪费你的时间去自己创造一切

![](img/a404d267f5f3c18a2846ed46c0c47307.png)

Photo by [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

UI 是最难的部分。在某种程度上，这比后端的逻辑更难。事实上，用户并不关心后端是什么，因为他们看到的只是前端部分。所以，在用户第一眼看到的时候抓住他们是很重要的。

如果你有时间，你可以从头开始编写 UI 代码，玩得开心。但是如果时间是一种奢侈，你应该从下面的 21 个库中选择现成的 UI 组件:

# 1.反应套件

[React Suite](https://rsuitejs.com/en/guide/introduction) 包含工具提示、加载器、图标、按钮等很多组件。它为企业系统产品提供了一个友好的用户界面。

安装:

```
# npm
npm i rsuite# yarn
yarn add rsuite
```

使用示例:

```
import { Button } from ‘rsuite’;
import ‘rsuite/lib/styles/index.less’; // or ‘rsuite/dist/styles/rsuite-default.css’export default function SignInButton() {
  return (
    <Button>Sign In</Button>
  );
}
```

# 2.碎片会起反应

[碎片反应](https://designrevision.com/docs/shards-react/getting-started)让你的用户界面尽可能的高质量，并通过提供大量的组件列表来节省你的大量时间。你可以用这个库构建几乎所有类型的 UI。

安装:

```
# npm
npm i shards-react# yarn
yarn add shards-react
```

使用示例:

```
import { Badge } from “shards-react”;export default function VipBadge() {
  return (
    <Badge theme=’info’>VIP</Badge>
  );
};
```

# 3.PrimeReact

[PrimeReact](https://primefaces.org/primereact/) 给你各种有独特主题的组件供你选择。您还可以定制这些主题和组件来满足自己的需求。

安装:

```
npm i primereact
npm i primeicons
```

使用示例:

```
import { InputText } from ‘primereact/inputtext’;export default function PasswordInput() {
  return (
    <InputText
      value={‘value’}
      onChange={(event) => onPasswordChanged(‘value’)}
    />
  );
};
```

# 4.材料-用户界面

[Material-UI](https://material-ui.com) 基于 Google Material 设计原理实现。如果你想按照这个原则创建你的网站，不要跳过这个。

安装:

```
# npm
npm i @material-ui/core# yarn
yarn add @material-ui/core
```

使用示例:

```
import Button from ‘@material-ui/core/Button’;export default function CustomButton() {
  return (
    <Button color=’primary’>Custom Button</Button>
  );
};
```

# 5.温泉 UI

材料和平面设计在[温泉 UI](https://onsen.io/v2/guide/react/) 中融为一体。我喜欢这个库的一点是，它让你的 web 应用看起来像一个本地应用。所以，如果你想移动优先，就选这个吧。

安装:

```
npm i onsenui react-onsenui
```

使用示例:

```
import { Page, Button } from “react-onsenui”export default function CustomButton() {
  return (
    <Page>
      <Button>Custom Button</Button>
    </Page>
  );
};
```

# 6.阿特拉斯基特

Atlaskit 是一个 UI 组件库，帮助实现 Atlassian 设计指南。

安装(只需添加您想要使用的组件，[参见此处的组件列表](https://atlaskit.atlassian.com/packages)):

```
# npm
npm i @atlaskit/button# yarn
yarn add @atlaskit/button
```

使用示例:

```
import Button from '@atlaskit/button';export default function ConfirmButton() {
  return (
    <Button theme='dark'>Confirm</Button>
  );
};
```

# 7.碳成分

[Carbon Components](https://react.carbondesignsystem.com/?path=/story/accordion--default) 是代表 IBM 碳设计体系的一个。这些组件旨在创建设计的一致性，并确保设计师和开发人员相处融洽。

安装:

```
# npm
npm i carbon-components-react carbon-components carbon-icons# yarn
yarn add carbon-components-react carbon-components carbon-icons
```

使用示例:

```
import { Button } from “carbon-components-react”;export default function ViewDetailButton() {
  return (
    <Button>View Detail</Button>
  );
};
```

# 8.蓝图

在[蓝图](https://blueprintjs.com)中，组件由 TypeScript 构成，并使用 Sass 进行样式化，以加速开发。这个库专注于在现代浏览器上运行的桌面应用程序的数据表示。

安装:

```
yarn add @blueprintjs/core react react-dom
```

使用示例:

```
import { Button } from “@blueprintjs/core”;export default function PlaceOrderButton() {
  return (
    <Button text=’Order’ onclick={order} />
  );
};
```

# 9.反应阱

简而言之， [Reactstrap](https://reactstrap.github.io) 是自举 4 的 React 组件。如果您熟悉 bootstrap，就不需要任何进一步的解释。

安装:

```
npm i reactstrap react react-dom
```

使用示例:

```
import { Button } from ‘reactstrap’;export default function RegisterButton() {
  return (
    <Button>Register</Button>
  );
};
```

# 10.React 工具箱

[React 工具箱](http://react-toolbox.io/#/)的组件遵循谷歌材质设计。这个库建立在一些最流行的提议之上，比如 CSS 模块、ES6 和 Webpack。

安装:

```
npm i react-toolbox
```

使用示例:

```
import Dialog from ‘react-toolbox/lib/dialog’;export default function CustomDialog() {
  return (
    <Dialog title=’Custom Dialog’>Content</Dialog>
  );
};
```

# 11.美女

Belle 提供了广泛的组件，这些组件经过优化，可以在桌面和移动设备上很好地工作。您可以在基础级别为所有组件或每个组件单独定制这些组件。

安装:

```
npm i belle
```

使用示例:

```
import { Button } from ‘belle’;export default function CloseButton() {
  return (
    <Button>Close</Button>
  );
};
```

# 12.React 桌面

[React Desktop](http://reactdesktop.js.org) 利用 macOS 和 Windows 的用户界面将原生桌面应用程序带到网络上。

安装:

```
npm i react-desktop
```

使用示例:

```
import { Text } from ‘react-desktop/macOs’;export default function CustomText() {
  return (
    <Text size=’18’>content</Text>
  );
};
```

# 13.索环

[索环](https://v2.grommet.io)是基于 React 的框架。它主要关注可访问性、响应性和主题化。

安装:

```
npm i grommet
```

使用示例:

```
import { Grommet, Heading } from “grommet”export default function CustomHeading() {
  return (
    <Grommet theme={theme}>
      <Heading level=’3’>Custom Heading</Heading>
    </Grommet>
  );
};
```

# 14.雷巴斯

[Rebass](https://rebassjs.org) 带有一些基础组件。这些组件是可扩展的，这使您能够创建一组很好的 UI 元素。

安装:

```
npm i rebass
```

使用示例:

```
import { Label } from ‘@rebass/forms’;export default function CustomLabel() {
  return (
    <Label>Username:</Label>
  );
};
```

# 15.元素用户界面

Elemental UI 包含了基本的组件，但是你仍然可以单独使用一个组件或者混合使用它们来创建一个美妙的 UI。

安装:

```
npm i elemental
```

使用示例:

```
import { Button } from ‘elemental’;export default function LogoutButton() {
  return (
    <Button size=’sm’ type=’primary’>Logout</Button>
  );
};
```

# 16.反应引导

[React bootstrap](https://react-bootstrap.github.io) 是一种使用 Bootstrap 的方式，不是原来的 Bootstrap 方式，而是 React 方式。

安装:

```
npm i react-bootstrap
```

使用示例:

```
import Button from “react-bootstrap/Button”;export default function CustomButton() {
  return (
    <Button>Custom Button</Button>
  );
};
```

# 17.KendoReact

到目前为止，我们已经走过了免费的图书馆，现在是到了高级图书馆的时候了。说到数据可视化，你应该看看这个家伙。售价 899 美元起，物有所值。事实上，一些大公司正在使用它，如微软，IBM，美国宇航局，索尼等。

安装:

```
npm i — save @progress/kendo-react-grid @progress/kendo-data-query @progress/kendo-react-inputs @progress/kendo-react-intl @progress/kendo-react-dropdowns @progress/kendo-react-dateinputs @progress/kendo-drawing @progress/kendo-react-data-tools
```

使用示例:

```
import ‘@progress/kendo-theme-default/dist/all.css’;
import { Grid, GridColumn } from ‘@progress/kendo-react-grid’;export default function CustomGrid() {
  return (
    <Grid data={products}>
      <GridColumn field=’ProductName’ />
      <GridColumn field=’ProductPrice’ />
      <GridColumn field=’ProductDiscount’ />
    </Grid>
  );
};
```

# 18.完全形态

Pinterest 开发了[格式塔](https://github.com/pinterest/gestalt)遵循他们的内部设计方针，让开发人员和设计人员使用相同的语言。现在，您可以利用它与您的设计师朋友聊天。

安装:

```
# npm
npm i gestalt# yarn
yarn add gestalt
```

用法示例:

```
import { Avatar } from ‘gestalt’;
import ‘gestalt/dist/gestalt.css’;export default function CustomAvatar() {
  return (
    <Avatar
      size=’sm’
      src=’image source’
      name=’Amy’
    />
  );
};
```

# 19.对虚拟化做出反应

[反应虚拟化](https://github.com/bvaughn/react-virtualized)专注于展示大型数据集。它是渲染大型表格、列表和网格的完美库。

安装:

```
npm i react-virtualized
```

用法示例:

```
import ‘react-virtualized/styles.css’;
import {Collection} from ‘react-virtualized’;export default function UsersList() {
  function cellRenderer({index, key, style}) {
    return (
      <div key={key} style={style}>
        {list[index].name}
      </div>
    );
  } return (
    <Collection
      cellCount={100}
      cellRender={cellRenderer}
      width={256}
      height={256}
    />
  );
}.
```

# 20.常绿树

[常青](https://evergreen.segment.com)含有大量柔韧可组合的成分。它包括几乎所有你需要开始为网站构建用户界面的东西。

安装:

```
#npm
npm i evergreen-ui#yarn
yarn add evergreen-ui
```

用法示例:

```
import { Button } from ‘evergreen-ui’;export default function CancelButton() {
  return (
    <Button>Cancel</Button>
  );
};
```

# 21.FluentUI

这个 [FluentUI](https://github.com/microsoft/fluentui) 来自一个大家伙，微软。你知道办公室，对吗？如果您想创建类似 Office 的用户界面，请选择 FluentUI。

安装:

```
# npm
npm i @fluentui/react# yarn
yarn add @fluentui/react
```

用法示例:

```
import { Button } from ‘@fluentui/react/lib/Button’;export default function DeleteButton() {
  return (
    <Button>Delete</Button>
  );
};
```

嗯，我想就这样了。上面的 21 个反应用户界面库现在就在你的手中。你应该一个接一个地测试它们，并选择你认为对你下一个项目最有用的人。

如果你已经和他们有过一些经历，请告诉我你最喜欢哪一个，为什么？

进一步阅读:

[](https://medium.com/javascript-in-plain-english/7-simple-ways-to-conditionally-render-components-in-react-a3170d0cd9e0) [## 7 种在反应中有条件渲染组件的简单方法

### 显示红色用户名(用户类型 A)，蓝色用户名(用户类型 B)，或仅显示登录用户的仪表板…

medium.com](https://medium.com/javascript-in-plain-english/7-simple-ways-to-conditionally-render-components-in-react-a3170d0cd9e0)