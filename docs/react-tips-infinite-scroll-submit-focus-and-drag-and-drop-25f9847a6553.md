# 反应提示—无限滚动、提交、聚焦和拖放

> 原文：<https://javascript.plainenglish.io/react-tips-infinite-scroll-submit-focus-and-drag-and-drop-25f9847a6553?source=collection_archive---------6----------------------->

![](img/d194118f34107d111eea3d3d5dc41d98.png)

Photo by [Herbert Goetsch](https://unsplash.com/@hg_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 在反应中提交

要在 React 应用程序中运行提交处理程序，我们应该调用`preventDefault`来阻止默认的提交行为，即提交给服务器。

例如，我们写道:

```
class App extends React.Component { submit(e){
    e.preventDefault();
    alert('submitted');
  } render() {
    return (
      <form onSubmit={this.submit}>
        <button type='submit'>click me</button>
      </form>
    );
  }
});
```

我们用`submit`方法调用了`e.preventDefault()`，并将其作为`onSubmit`属性的值传递。

# 对 Render 上调用的 onClick 作出反应

我们必须传入对函数的引用，而不是调用它。

例如，我们写道:

```
class Create extends Component {
  constructor(props) {
    super(props);
  } render() {
    const playlist = this.renderPlaylists(this.props.playlists);
    return (
      <div>
        {playlist}
      </div>
    )
  } renderPlaylists(playlists) {
    const activatePlaylist = this.activatePlaylist.bind(this, playlist.id);
    return playlists.map(playlist => {
      return (
        <div key={playlist.id} onClick{activatePlaylist}>
          {playlist.name}
        </div>
      );
    })
}
```

我们有:

```
this.activatePlaylist.bind(this, playlist.id)
```

它返回一个函数，将`this`的值更改为当前组件的值。

此外，它将`playlist.id`作为参数传递给`this.activatePlaylist`方法。

# 使 React 组件或元素可拖动

要轻松创建一个可拖动的组件，请听一下`mousemove`、`mousedown`和`mouseup` 事件

例如，我们可以写:

```
import React, { useRef, useState, useEffect } from 'react'const styles = {
  width: "200px",
  height: "200px",
  background: "green",
  display: "flex",
  justifyContent: "center",
  alignItems: "center"
}const DraggableComponent = () => {
  const [pressed, setPressed] = useState(false)
  const [position, setPosition] = useState({x: 0, y: 0})
  const ref = useRef() useEffect(() => {
    if (ref.current) {
      ref.current.style.transform = `translate(${position.x}px, ${position.y}px)`
    }
  }, [position]) const onMouseMove = (event) => {
    if (pressed) {
      setPosition({
        x: position.x + event.movementX,
        y: position.y + event.movementY
      })
    }
  } return (
    <div
      ref={ref}
      style={styles}
      onMouseMove={ onMouseMove }
      onMouseDown={() => setPressed(true)}
      onMouseUp={() => setPressed(false)}>
      <p>drag me</p>
    </div>
  )
}
```

我们有带一些道具的`Draggable`组件。

我们监听`mousedown`和`mouseup`事件，将`pressed`状态分别设置为`false`和`true`。

如果`pressed`状态是`true`，也就是我们正在拖动的时候，这将让我们被拖动。

然后，我们通过将`onMouseMove`函数传递给`onMouseMove`属性来为`mousemove`事件添加一个监听器。

然后我们在`onMouseMove`功能中设置位置。

如果`pressed`是`true`，我们通过改变 div 的`x`和`y`坐标来设置位置。

# 带 React 的无限滚动

要使用 React 轻松添加无限滚动，我们可以使用 react-infinite-scroller 包。

要安装它，我们运行:

```
npm install react-infinite-scroller
```

然后我们可以通过写来使用它:

```
import React, { Component } from 'react';
import InfiniteScroll from 'react-infinite-scroller';class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      listData: [],
      hasMoreItems: true,
      nextHref: null
    };
    this.fetchData = this.fetchData.bind(this);
  } async fetchData(){
    const listData  = await getJobsData();
    this.setState({ listData });
  } componentDidMount() {
     this.fetchData();
  } render() {
    const loader = <div className="loader">Loading ...</div>;
    const JobItems = this.state.listData.map(job => {
      return (<div>{job.name}</div>);
    });    
    return (
      <div className="Jobs">
         <h2>Jobs List</h2>
         <InfiniteScroll
           pageStart={0}
           loadMore={this.fetchData.bind(this)}
           hasMore={this.state.hasMoreItems}
           loader={loader}
         >
            {JobItems}
         </InfiniteScroll>
      </div>
    );
  }
}
```

我们使用`InfiniteScroll`组件为我们的应用程序添加无限滚动。

`pageStart`是起始页码。

`loadMore`是加载更多数据的功能。

`hasMore`就是看我们有没有更多数据的状态。

`loader`为装载机部件。

每次加载并滚动到页面底部时，我们都会得到新的数据。

# 选择输入中的所有文本，并在聚焦时做出反应

我们可以在输入上调用`select`方法来聚焦它。

例如，我们可以写:

```
const Input = (props) => {
  const handleFocus = (event) => event.target.select(); return <input type="text" value="something" onFocus={handleFocus} />
}
```

我们有`handleFocus`函数，它调用输入元素上的`select`方法来选择聚焦时的输入值。

有了类组件，我们可以编写:

```
class Input extends React.Component {
  constructor(){
    super();
    this.handleFocus = this.handleFocus.bind(this);
  }      handleFocus(event){
    event.target.select();
  } render() {
    return (
      <input type="text" value="something" onFocus={this.handleFocus} />
        );
    }
}
```

当我们聚焦输入时，我们有`handleFocus`方法调用`select`来选择输入值。

![](img/75c0482f5ba98d70efbd74cbad79f960.png)

Photo by [Alexandru Sofronie](https://unsplash.com/@alexsofronie?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用一个软件包轻松添加无限滚动。

此外，我们可以选择输入的值。

我们可以在没有库的情况下将可拖动的项目添加到我们的组件中。

我们必须调用`preventDefault`来停止默认的提交行为。

## 简单英语中的 JavaScript

喜欢这篇文章吗？如果是这样的话，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获得更多类似的内容吧！**