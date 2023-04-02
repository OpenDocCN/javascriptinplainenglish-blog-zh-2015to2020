# 使用谷歌地图 JavaScript API 添加自定义标记

> 原文：<https://javascript.plainenglish.io/add-custom-markers-with-the-google-maps-javascript-api-43e8b83f4f7d?source=collection_archive---------0----------------------->

## 2020 年更新的解决方案，关于如何使用 JavaScript 将谷歌地图添加到您的网站，并根据客户标记进行调整

![](img/10f92bdca60abe817cd0619d1c64fa31.png)

Photo by [Mackenzie Weber](https://unsplash.com/@m_weber?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/maps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有很多关于如何使用 Google Maps API 在你的网站上实现地图的教程，但是大部分都是旧的，没有很好的整合在一起。在本文中，我将介绍如何创建地图，添加标记，如何为标记添加自定义图标，以及如何添加弹出叠加图。

在我们开始之前，您需要有一个 Google API 密钥，因此要做到这一点，我们必须转到 [Google API 控制台](https://console.developers.google.com/apis)并创建一个项目(您创建的这个项目不只是用于 Google Maps。它支持很多 API)。创建项目后，您需要转到 Maps JavaScript 并启用它。在“凭据”选项卡中，您可以创建 API 密钥。

*还可以查看*[*API layer*](https://apilayer.com/?utm_source=plainenglish&utm_medium=Leads%20Acquisition&utm_content=apilayer)*，这是一套提升生产力的 Web APIs 和基于云的微服务应用。*

现在一切都准备好了，让我们开始吧。

# 设置地图

首先，创建一个 HTML 文件，并从[谷歌地图文档](https://developers.google.com/maps/documentation/javascript/tutorial)中复制脚本

```
<div id="map"></div>
<script src="https://maps.googleapis.com/maps/api/js?key=**YOUR_API_KEY**&callback=initMap" async defer></script>
```

现在请注意，它有一个名为`callback`的参数，该参数被设置为`initMap`。所以它将加载一个名为`initMap`的函数。我们要做的是将`function initMap`放入我们的 JavaScript 文件中。在`initMap`中，我们必须设置一个名为 map 的变量，并设置一个新的 Google Map 对象。

> 出于开发目的，您可以通过添加`js?v=3.exp`来添加没有 API 键的地图

```
function initMap() {
   var options = {
      zoom:10,
      center: { lat:40.730610, lng:-73.935242} //Coordinates of New York 
   }var map = new google.maps.Map(document.getElementById('map'), options);}
```

现在如果你保存并重新加载，你将不会在网站上看到任何东西。那是因为我们还没有为我们的地图设置高度和宽度。因此，您必须将这几行代码添加到 CSS 文件中

```
#map {
   height: 400px;
   width: 100%;
}
```

现在我们有了一张以纽约市为中心的地图。

# 添加自定义标记

要添加自定义标记，您需要创建一个变量并将其设置为`google.maps.Markers()`,并给出放置标记的对象位置

```
var marker = new google.maps.Marker({
   position:{lat: 40.6782, lng: -73.9442}, // Brooklyn Coordinates
   map:map //Map that we need to add
   icon:'[https://img.icons8.com/fluent/48/000000/marker-storm.png](https://img.icons8.com/fluent/48/000000/marker-storm.png)',
   // adding custom icons (Here I used a Flash logo marker)
   draggarble: false// If set to true you can drag the marker
});
```

让我们继续添加一个包含一些信息或内容的弹出窗口，为此我们要做的是在`marker`下创建一个名为`infomation`的变量，并将其设置为`google.maps.InfoWindow`，该变量将接收一个包含我们想要的内容的对象。

```
var information = new google.maps.InfoWindow({
   content: '<h4>The marker is at Brooklyn</h4>'
});
```

这本身不会做任何事情，所以我们需要添加一个事件监听器来监听那个信息窗口

```
marker.addListener('click', function() {
   information.open(map, marker);
});
```

现在让我们看看我们到目前为止完成的所有代码是什么样子的，

[https://jsfiddle.net/c6shrnbw/6/](https://jsfiddle.net/c6shrnbw/6/)

这就是如何添加带有自定义图标和信息窗口的单个标记。

# 添加多个标记

当您需要添加多个标记时，使用一个函数而不硬编码坐标是非常有效的。让我们看看如何做到这一点，

```
function addMarker(coordinates) {
   var marker = new google.maps.Marker({
      position: coordinates, // Passing the coordinates
      map:map, //Map that we need to add
      draggarble: false// If set to true you can drag the marker
   });
}addMarker({lat: 40.6782, lng: -73.9442}); // Brooklyn Coordinates
addMarker({lat: 40.7831, lng: -73.9712}); // Manhattan Coordinates
```

您可以更改传递属性，并为每个标记设置不同的自定义图标。但是当您为一些标记添加自定义图标时，保持默认标记图标不变并不是最佳选择。传递这些标记的内容信息也是如此。

```
function addMarker(prop) {
   var marker = new google.maps.Marker({
      position: prop.coordinates, // Passing the coordinates
      map:map, //Map that we need to add
      draggarble: false// If set to true you can drag the marker
   }); if(prop.iconImage) { marker.setIcon(prop.iconImage); } if(prop.content) { 
      var information = new google.maps.InfoWindow({
      content: prop.content
      });

      marker.addListener('click', function() {
      information.open(map, marker);
      });
   }
}addMarker({
   coordinates:{lat: 40.6782, lng: -73.9442},
   iconImage:'[https://img.icons8.com/fluent/48/000000/marker-storm.png](https://img.icons8.com/fluent/48/000000/marker-storm.png)',
   content:'<h4>Brooklyn Marker</h4>'
});
addMarker({
   coordinates:{lat: 40.7831, lng: -73.9712} // Manhattan Marker
}); 
```

让我们看看使用 JSFiddle 的结果，

[JSFiddle](https://jsfiddle.net/c6shrnbw/9/)

通过将所有标记属性添加到一个数组中，可以进一步优化这一点。

# 结论

这些是你可以用谷歌地图 JavaScript API 做的一些事情。文章到此为止。谢谢，注意安全！

*更多内容看*[***plain English . io***](http://plainenglish.io)

## 资源

*   [谷歌地图文档](https://developers.google.com/maps/documentation/javascript/tutorial)
*   [JSFiddle](https://jsfiddle.net/)