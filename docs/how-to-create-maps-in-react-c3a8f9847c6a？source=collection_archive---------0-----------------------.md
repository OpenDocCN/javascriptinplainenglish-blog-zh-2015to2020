# 如何在 React 中创建地图

> 原文：<https://javascript.plainenglish.io/how-to-create-maps-in-react-c3a8f9847c6a?source=collection_archive---------0----------------------->

## 了解如何使用 React 和传单创建地图。

![](img/3c23ad128cdbaf1425ac4047cd0bc5e8.png)

Photo by [Jake Davies](https://unsplash.com/@jvkedavies?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

读者你好！

今天在这篇文章中，我们将看到如何使用 React，vanilla.js 和传单创建一个地图。在进入正题之前，我们先来看看关于传单——它是构建地图的强大工具。我们可以使用这个工具创建不同种类的地图。它支持所有的网络浏览器和移动平台。

在这个项目中，我们将代表非医疗火灾事件的地点。消防部门在地图上回应。我们只标记火灾发生的地点，并显示一些细节。

让我们创建一个简单的例子。现在我们可以设置传单地图，使用标记和弹出窗口。

# 我们开始吧

第一步，让我们在我们的**/项目**文件夹中创建**index.html**和 **app.js** 文件，并链接到我们的**index.html**文件。

通过使用传单，我们必须在头部标签中链接传单 CSS 和传单 JS。始终将活页 CSS 放在活页 JS 之前。

添加一个容器来保存我们到 index.html 的地图:

```
<div id="mapid"></div>
```

在此之前，让我们设置容器的高度:

```
#mapid { height: 1000px; }
```

现在在 Script 标签中创建新的 JavaScript 文件，并确保在调用 **L.map('mapid')** 之前将**<div id = " mapid ">**添加到 dom 中。

# 创建地图

为了初始化地图，我们必须将 div 传递给带有一些选项的 L.map() 。

```
const myMap = L.map('mapid', {
 center: [37.7749, -122.4194],
  zoom: 13
})
```

第一步，我们必须使用传单 API 的 Map 类在页面上创建一个地图。然后我们将传递两个参数给这个类:

*   我们向 DOM ID 传递了一个字符串变量。
*   地图选项的可选对象。

我们可以将许多选项传递给我们的班级。主要的两个选项是居中和缩放。该中心是最初的地理中心。缩放指定了初始地图缩放级别。

使用 **setView()** 初始化地图。坐标和一个整数被分配到缩放级别的数组中。

```
const myMap = L.map('map').setView([37.7749, -122.4194], 13);
```

默认情况下，地图上的所有鼠标和触摸控件都处于启用状态。

# 创建层

现在，我们可以添加一个切片层到我们的地图，即地图框街道切片层。我们可以通过实例化 TileLayer 类来添加不同类型的平铺层。

要创建切片图层，首先我们必须设置切片图像的 URL 模板、属性文本和图层的最大缩放级别。URL 模板将允许服务提供商访问切片图层。然后我们使用 [Mapbox 的静态图块 API](https://docs.mapbox.com/api/maps/#static-tiles) ，所以我们需要[请求一个访问令牌](https://www.mapbox.com/studio/account/tokens/)。

```
L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', { 
attribution: 'Map data © <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
maxZoom: 18, 
id: 'mapbox/streets-v11', 
accessToken: 'your.mapbox.access.token' }).addTo(mymap);
```

现在当我们打开我们的**index.html**时，我们可以看到一张地图。

# 标记和圆圈

现在我们有了地图和图层，但它没有指向任何特定的东西。为了指向地图上的特定位置，我们必须为传单提供一个标记。

要锁定一个特定的位置，首先我们必须使用 marker 类实例化标记，然后传递坐标并将其添加到地图中。

```
const marker = L.marker([37.7544, -122.4477]).addTo(mymap);
```

现在我们用一个圆来绘制一个圆类。我们可以传递一些可选的选项，比如半径，颜色等等

```
const circle = L.circle([37.8157, -122.5295], {
 color: 'gold',
 fillColor: '#f03',
 fillOpacity: 0.5,
 radius: 200
}).addTo(mymap);
```

# 弹出窗口

如果我们想知道更多关于地点的信息。我们可以通过使用 popup 来实现这一点。

```
circle.bindPopup("I am pointing to Point Bonita Lighthouse");

marker.bindPopup("I am pointing to Twin Peaks");
```

Popup 方法为您提供了一个指定的 HTML 内容，并将其附加到标记中。当你点击标记时，弹出窗口出现。

# 反应传单

现在我们要创建一个地图，使用传单和普通 JavaScript 添加标记。让我们看看如何用 React 来构建。

## 访问 API 密钥

*   创建一个帐户并登录。
*   点击右下角的管理按钮。
*   创建新的 API 键并分配一个名称。
*   复制您的密钥 ID 和密钥。这可以用来访问数据。

现在，我们正在使用反应传单组件的传单地图。让我们创建我们的 react 应用程序。

```
npx create-react-app react-fire-incidents
cd react-fire-incidents
```

让我们在我们的终端中通过以下命令安装 react-leaflet 和 leaflet:

```
npm install react-leaflet leaflet
```

# APP。射流研究…

让我们在 src 中创建一个文件夹/组件。在 components 文件夹中创建一个名为 **Map.js** 的文件。现在编辑 **App.js** 文件，从 react-leaflet 导入模块。然后新建一个 **Map.js** 文件**。**

```
import React, { Component, Fragment } from 'react';
import axios from 'axios';
import Map from './components/Map'
```

在我们的 App 类中，我们将在状态中定义一个名为 incidents 的数组。当页面加载时，它会将我们的数据推入这个数组。

```
class App extends Component {
 state = {
   incidents: [],
 }
 render() {
   return (
     <div> </div>
   );
 }
}
export default App;
```

然后，当组件做出反应时，我们将发出 GET 请求。一旦我们得到数据，就把它传给我们的州:

```
async componentDidMount() {
   const res = await axios.get('https://data.sfgov.org/resource/wr8u-xric.json', {
     params: {
       "$limit": 500,
       "$$app_token": YOUR_APP_TOKEN
     }
   })
   const incidents = res.data;
   this.setState({incidents: incidents });
 };
```

现在我们的 **app.js** 文件将会是这样的:

```
class App extends Component {
state = {
  incidents: [],
}

async componentDidMount() {
 const res = await axios.get('https://data.sfgov.org/resource/wr8u-xric.json', {
   params: {
     "$limit": 500,
     "$$app_token": YOUR_APP_TOKEN
   }
 })
 const incidents = res.data;
 this.setState({incidents: incidents });
};
render() {
 return (
<Map incidents={this.state.incidents}/>
 );
}
}
export default App;
```

# 地图。射流研究…

我们已经知道如何创建一个传单地图。现在，我们将从 react-leaflet 导入地图、标记、弹出窗口、切片层组件。

```
import React, { Component } from 'react'
import { Map, TileLayer, Marker, Popup } from 'react-leaflet'
```

在这个例子中，我们需要坐标和缩放级别来初始化地图。在我们的 Map 类中，我们通过 lat、lng 和 zoom 变量来定义它们。

```
export default class Map extends Component {
   state = {
       lat: 37.7749,
       lng: -122.4194,
       zoom: 13,
   }
   render() {
       return (
     <div></div>
        )
    }
}
```

在我们的 react-leaflet 的地图组件中，我们传递了中心坐标和带有某种样式的缩放级别。在我们的切片层组件中，我们传递一个属性和 URL。

```
render() {
       return (
          this.props.incidents ?
              <Map 
                 center={[this.state.lat, this.state.lng]} 
                 zoom={this.state.zoom} 
                 style={{ width: '100%', height: '900px'}}
              >
              <TileLayer
                attribution='&copy <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
                url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
               />
             </Map>
               :
               'Data is loading...'
       )
   }
}
```

然后，我们将循环遍历我们的 **props.incident** ，并将每个事件的坐标传递给标记组件。在标记组件内部，我们传递一个弹出组件。

```
<Map 
    center={[this.state.lat, this.state.lng]} 
    zoom={this.state.zoom} 
    style={{ width: '100%', height: '900px'}}>
       <TileLayer
          attribution='&copy <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        />
        {
          this.props.incidents.map(incident => {
               const point = [incident['point']['coordinates'][1],                 incident['point']['coordinates'][0]]

return (
    <Marker position={point} key={incident['incident_number']} >
         <Popup>
            <span>ADDRESS: {incident['address']}, {incident['city']} - {incident['zip_code']}</span>
          <br/>
            <span>BATTALION: {incident['battalion']}</span><br/>
         </Popup>
     </Marker>
  )
 })
}
</Map>
```

现在，如果我们运行我们的应用程序，我们可以看到一张地图。如果我们单击任何标记，将会出现一个弹出窗口，显示有关该位置的更多信息。

# 结论

我希望你喜欢很多，并知道如何使用 React 和传单创建一个地图。在这个应用程序中，我们只是实现了基本的功能。如果你愿意，你可以给它添加更多的功能，使它更具互动性。

感谢阅读！

## 简单英语的 JavaScript

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！