# JavaScript 中地理定位 API 的介绍

> 原文：<https://javascript.plainenglish.io/an-introduction-to-the-geolocation-api-in-javascript-cc7fa1260c33?source=collection_archive---------5----------------------->

## 通过实例学习使用 JavaScript 中的地理定位 API

![](img/8cb3eb1f1aaba7d057d94a30014b65b1.png)

Photo by [henry perks](https://unsplash.com/@hjkp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

**地理定位 API** 用于获取用户的地理位置。这是一个简单的方法来获得用户在你的网站上的经度和纬度。但是，由于这可能会损害隐私，除非用户批准，否则该职位不可用。

在本文中，我们将学习 JavaScript 中的地理定位 API 以及如何使用它。让我们开始吧。

# 使用地理定位 API

地理定位 API 在所有浏览器中都受支持，并且只能在 HTTPS 这样的安全环境下工作。如果您的网站托管在不安全的来源上(如 HTTP)，获取用户位置的请求将不再起作用。

方法`**getCurrentPosition()**`用于返回用户的位置。

这里有一个例子:

```
function getLocation() {// Check if geolocation is supported.
if (navigator.geolocation) {
  navigator.geolocation.**getCurrentPosition**(showPosition);// If not supported.
} else {
  console.log("Geolocation is not supported by this browser.");
}
}function **showPosition(position)** {
  const x = **position.coords.latitude**;
  const y = **position.coords.longitude**;
  console.log(x); // Returns your latitude.
  console.log(y); // Returns your longitude.
}
```

正如您在上面看到的，第一个函数将检查地理位置是否受支持。如果是，它将运行函数`getCurrentPosition`，否则，它将向用户显示一条消息。

如果方法`getCurrentPosition`成功，它返回一个坐标对象给参数`showPosition`中指定的函数。函数`showPosition()`输出纬度和经度。

现在，如果您愿意，可以在网页上显示用户的位置。

# 在地图中显示结果

您还可以使用地理定位 API 在地图上显示结果。你只需要访问一个地图服务，比如谷歌地图。

在下面的示例中，返回的纬度和经度用于在 Google 地图中显示位置(使用静态图像):

```
function showPosition(position) {
  var latlon = position.coords.latitude + "," +position.coords.longitude;

  var img_url = "https://maps.googleapis.com/maps/api/staticmap?center=
  "+**latlon**+"&zoom=14&size=400x300&sensor=false&key=YOUR_KEY";

  const map = document.getElementById("mapholder");
  map.innerHTML = "<img src='"+img_url+"'>";
}
```

# 结论

这只是对一个很酷的 JavaScript API 的介绍，我鼓励你通过访问 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) 文档来探索关于这个 API 的更多信息。

感谢您阅读本文，希望您觉得有用。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-amazing-things-pure-css-can-do-20b8fe3738b) [## 纯 CSS 能做的 5 件惊人的事情

### 你可以用纯 CSS 创造出很棒的东西

medium.com](https://medium.com/javascript-in-plain-english/5-amazing-things-pure-css-can-do-20b8fe3738b)