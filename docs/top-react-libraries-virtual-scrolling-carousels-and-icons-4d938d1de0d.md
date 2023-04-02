# 顶级反应库——虚拟滚动、旋转木马和图标

> 原文：<https://javascript.plainenglish.io/top-react-libraries-virtual-scrolling-carousels-and-icons-4d938d1de0d?source=collection_archive---------5----------------------->

![](img/92da4b58559e8ab0d8bd2a881113fb27.png)

Photo by [Pel](https://unsplash.com/@cha_pel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应虚拟化

react-virtualized 包让我们显示一个虚拟化列表。

要安装它，我们运行:

```
npm i react-virtualized
```

然后我们可以通过写来使用它:

```
import React from "react";
import { List } from "react-virtualized";const list = Array(1000)
  .fill()
  .map((_, i) => i);function rowRenderer({ key, index, isScrolling, isVisible, style }) {
  return (
    <div key={key} style={style}>
      {list[index]}
    </div>
  );
}export default function App() {
  return (
    <div className="App">
      <List
        width={300}
        height={300}
        rowCount={list.length}
        rowHeight={20}
        rowRenderer={rowRenderer}
      />
    </div>
  );
}
```

我们用`rowRenderer`来显示列表项。

它需要以下道具。

`key`是行数组中的唯一键。

`index`是集合中行的索引。

`isScrolling`是当前正在滚动的列表。

`isVisible`表示该行是否可见。

`sytle`是用于定位行的样式对象。

我们也可以用它来显示一个网格。

例如，我们可以写:

```
import React from "react";
import { Grid } from "react-virtualized";
import * as _ from "lodash";const list = _.chunk(
  Array(1000)
    .fill()
    .map((_, i) => i),
  3
);function cellRenderer({ columnIndex, key, rowIndex, style }) {
  return (
    <div key={key} style={style}>
      {list[rowIndex][columnIndex]}
    </div>
  );
}export default function App() {
  return (
    <div className="App">
      <Grid
        cellRenderer={cellRenderer}
        columnCount={list[0].length}
        columnWidth={100}
        height={300}
        rowCount={list.length}
        rowHeight={30}
        width={300}
      />
    </div>
  );
}
```

我们用 Lodash `chunk`方法创建了一个嵌套数组，每个条目中有 3 个条目。

然后我们创建了`cellRenderer`来渲染单元格。

`columnIndex`有列的索引。

`key`有条目的唯一键。

`rowIndex`有该条目的行索引。

`style`具有定位项目的样式。

然后在`App`中，我们使用`Grid`组件，该组件使用`cellRenderer`并传入行和列计数。

高度和宽度也已设定。

这个包还附带了桌子和砖石网格的组件。

# 反应图标

react-icons 包允许我们向 react 应用程序添加图标。

要安装它，我们可以运行:

```
npm i react-icons
```

然后我们可以导入图标并使用它:

```
import React from "react";
import { FaBeer } from "react-icons/fa";export default function App() {
  return (
    <div className="App">
      <h1>
        time for a <FaBeer />
      </h1>
    </div>
  );
}
```

它带有许多库的图标，包括 Bootstrap、Font Awesome、Github 等等。

# 反应-滑头

react-slick 包允许我们向 react 应用程序添加一个旋转木马。

要安装它，我们可以运行:

```
npm i react-slick slick-carousel
```

然后我们可以通过写来使用它:

```
import React from "react";
import "slick-carousel/slick/slick.css";
import "slick-carousel/slick/slick-theme.css";
import Slider from "react-slick";const settings = {
  dots: true,
  infinite: true,
  speed: 500,
  slidesToShow: 1,
  slidesToScroll: 1
};export default function App() {
  return (
    <div className="App">
      <Slider {...settings}>
        <div>
          <h3>1</h3>
        </div>
        <div>
          <h3>2</h3>
        </div>
        <div>
          <h3>3</h3>
        </div>
        <div>
          <h3>4</h3>
        </div>
        <div>
          <h3>5</h3>
        </div>
        <div>
          <h3>6</h3>
        </div>
      </Slider>
    </div>
  );
}
```

我们使用`Slider`组件向我们的应用程序添加了一些幻灯片。

`settings`对象有我们传入来配置转盘的道具。

`dots`让我们选择是否显示导航点。

`infinite`设置到达最后一张幻灯片后是否返回第一张幻灯片。

`speed`是滑动的速度。

`slidesToShow`是一次显示的幻灯片数量。

`slidesToScroll`是我们导航时滚动的幻灯片数量。

还有许多其他的道具可以用来设计圆点、幻灯片和转盘的其他部分。

![](img/4d9dd1aac2070ed323dac7ded432c423.png)

Photo by [Leilani Angel](https://unsplash.com/@leilaniangel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

react-virtualized 包允许我们创建虚拟滚动列表、表格或砖石网格。

react-icons 允许我们从许多来源添加图标。

react-slick 是一个库，可以让我们在应用程序中添加旋转木马。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)