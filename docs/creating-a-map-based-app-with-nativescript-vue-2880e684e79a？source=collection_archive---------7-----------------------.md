# 使用 NativeScript Vue 创建基于地图的应用程序

> 原文：<https://javascript.plainenglish.io/creating-a-map-based-app-with-nativescript-vue-2880e684e79a?source=collection_archive---------7----------------------->

随着移动设备和通信网络的发展，其最受欢迎的用途之一是基于位置的应用。Nativescript 提供了一些插件，这些插件提供了创建现代地图应用程序所需的地图和位置发现工具。在这篇文章中，我将讨论如何使用地图、定位服务和 API 调用来创建一个 Nativescript Vue 应用程序，该应用程序将显示当前用户位置的地图，并允许用户搜索附近的地方。

# 入门指南

首先，我们将使用 CLI 创建一个名为 ns6maps 的新应用程序。当被询问时，选择一个 Vue。开始编码的 JS 空白应用程序模板。一旦完成，我们将添加[谷歌地图插件](https://github.com/dapriett/nativescript-google-maps-sdk)，并在模拟器中运行，以确保一切正常。

```
tns create ns6maps 
cd ns6maps 
tns doctor 
tns plugin add nativescript-google-maps-sdk 
tns run ios
```

我们需要一些 API 密钥来在 iOS 和 Android 上使用谷歌地图，所以登录[谷歌云平台网站](https://cloud.google.com/console/google/maps-apis/overview)并为你的应用程序创建一个新项目。

![](img/d819e18f89804da8b89b78a0cf21f1d2.png)

在页面顶部选择该项目，您将看到一个屏幕，其中显示了可用于您的项目的所有 API

![](img/8b7897d04d32110274a1e42654cf2ad8.png)

首先通过点击那个框来启用 iOS 的地图 SDK，它将带你到那个 API 的概述。点击凭证，然后点击顶部的链接，转到主 Google APIs & Services 凭证区域，然后点击按钮创建凭证并选择一个 API 密钥。您将看到一个弹出窗口，显示新创建的密钥，以及一个限制该密钥的链接。

![](img/1805e712fce3e70831225c65999ef5e0.png)

点击`Restrict Key`链接，选择应用限制下的 iOS 应用。

![](img/b0492fc39ac30e529521f562c0673d52.png)

这将需要您的应用捆绑包标识符，因此让我们确保它与您的`package.json`中的标识符相匹配。

![](img/04b7be42819e6c279d77bcd829f9f064.png)

保存设置，并记下 iOS 密钥以备后用。现在，创建另一个 API 键，按照限制链接，将这个键限制为 Android 应用程序。对于 Android，你需要为用于开发应用的调试键和生产键添加 SHA1 指纹。如果您还没有一个产品密钥库，请按照 [Google 文档](https://developer.android.com/studio/publish/app-signing#generate-key)创建一个。对于 Linux/OSX 系统，可以通过使用`keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android`来获取调试指纹，按键限制屏幕的右侧显示了说明，显示了如何获取应用程序调试和生产版本的指纹。

![](img/6ca806859ef056aeeede9749ed90ede7.png)

现在我们有了 API 键，让我们将它们添加到我们的应用程序中。对于 Android，我们将从复制模板配置文件开始，使用:

```
cp -r node_modules/nativescript-google-maps-sdk/platforms/android/res/values app/App_Resources/Android/src/main/res
```

然后编辑`app/App_Resources/Android/src/main/res/values/nativescript_google_maps_api.xml`,在用 Android Google Maps API 键替换占位符后取消对字符串的注释。最后，编辑`app/App_Resources/Android/src/main/AndroidManifest.xml`并将以下内容添加到`application`标签中:

```
<meta-data android:name="com.google.android.geo.API_KEY" android:value="@string/nativescript_google_maps_api_key" />
```

对于 iOS，我们将编辑/app/app.js，并在其他导入之后将以下代码添加到页面顶部，使用之前生成的 iOS Google Maps API 密钥。我们还将添加代码来注册与 Nativescript Vue 一起使用的插件，并将该插件作为 MapView 加载，以便在该应用程序中使用。

```
import { isAndroid, isIOS } from "tns-core-modules/platform";
if (isIOS) {
    GMSServices.provideAPIKey('REPLACE_WITH_IOS_KEY');
}
Vue.registerElement('MapView', () => require('nativescript-google-maps-sdk').MapView);
```

编辑`Home.vue`并将内容替换为:

```
<template>
    <Page actionBarHidden="true" backgroundSpanUnderStatusBar="false">
        <StackLayout height="100%" width="100%" >
            <MapView iosOverflowSafeArea="true" :latitude="latitude" :longitude="longitude" :zoom="zoom" :bearing="bearing" :tilt="tilt" height="100%" @mapReady="onMapReady" @markerSelect="onMarkerSelect" @markerInfoWindowTapped="onMarkerInfoWindowTapped"></MapView>
        </StackLayout>
    </Page>
</template>

<script>
export default {};
</script>

<style scoped>
</style>
```

注意，我们已经将 Map 组件包含在 StackLayout 中，宽度和高度都设置为 100%。这样做是为了避免当地图是更复杂的页面布局的一部分时的潜在问题，并确保地图组件全屏显示。我们还将组件的`iosOverflowSafeArea`属性设置为 true，这样地图将在更大的缺口设备上完全填充屏幕，而不是被限制在安全的矩形屏幕区域。使用`tns run ios`运行应用程序，如果一切正常，您应该会看到一幅完整的地图:

![](img/cb57064d52abfb2ec89ae7d1978b758f.png)

# 使用 Nativescript 进行地理定位

如果您查看 MapView 组件，您会看到我们已经定义了参数，我们将使用这些参数来配置地图的位置和显示。我们现在将添加另一个插件，允许 Nativescript 请求当前设备位置，然后将这些位置设置为地图的中心坐标。我们还将在地图上显示当前用户的位置以供参考。首先安装[地理定位插件](https://github.com/NativeScript/nativescript-geolocation):

`tns plugin add nativescript-geolocation`

为了访问设备位置，我们需要额外的权限，所以让我们先解决这些问题。由于我们将使用地图，因此我们需要为该应用程序请求精确的位置权限。编辑`app/App_Resources/Android/src/main/AndroidManifest.xml`并添加以下权限:

```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

编辑`app/App_Resources/iOS/Info.plist`并添加以下字符串，以在请求位置许可时显示给用户:

```
<key>NSLocationWhenInUseUsageDescription</key>
	<string>Find your current location to see places nearby.</string>
<key>NSLocationUsageDescription</key>
	<string>Find your current location to see places nearby.</string>
```

接下来，我们将用 map 组件使用的变量填充 exports default data 部分。我们还将添加`mounted()`部分，在那里我们将请求位置许可，然后请求当前位置(如果我们有它们的话)。如果设备返回一个位置，我们将使用用户的当前坐标更新数据变量，这也将使地图重新以该位置为中心。将脚本部分修改为:

```
const geolocation = require("nativescript-geolocation");
export default {
    data() {
        return {
            latitude: '',
            longitude: '',
            zoom: '',
            bearing: '',
            tilt: '',
            mapView:null,
        }
    },
    mounted() {
        let that = this
        geolocation.enableLocationRequest(true, true).then(() => {
            geolocation.isEnabled().then(value => {
                if (!value) {
                    console.log("NO permissions!");
                    return false;
                } else {
                    console.log("Have location permissions");
                    geolocation
                        .getCurrentLocation({
                            timeout: 20000
                        })
                        .then(location => {
                            if (!location) {
                                console.log("Failed to get location!");
                            } else {
                                that.latitude = location.latitude
                                that.longitude = location.longitude
                                that.zoom = 14
                                that.bearing = 0
                                that.altitude = 0
                            }

                        });
                    return true;
                }
            });
        })

    },
    methods: {},
}
```

现在在 iOS 模拟器上运行应用程序，允许位置权限，如果您的模拟器设置了默认的旧金山位置，您应该会看到如下内容。

![](img/8d7fb886193f19571f5e8c43ef60b637.png)

让我们通过显示当前用户位置来改进我们的地图。我们将通过添加一个新函数来配置 MapView，一旦地图组件被加载并准备好使用，就会调用这个新函数。将以下函数添加到方法部分:

```
 onMapReady(args) {
            this.mapView = args.object;
            this.mapView.myLocationEnabled = true;
            this.mapView.zoomGesturesEnabled = true;
            var gMap = this.mapView.gMap;
            if (isAndroid) {
                uiSettings = gMap.getUiSettings();
                uiSettings.setMyLocationButtonEnabled(true);
                gMap.setMyLocationEnabled(true);
            }
            if (isIOS) {
                gMap.myLocationEnabled = true;
                gMap.settings.myLocationButton = true;
                this.mapView.on("myLocationTapped", event => {
                    geolocation.isEnabled().then(enabled => {
                        if (enabled) {
                            geolocation.getCurrentLocation({
                                maximumAge: 5000,
                                timeout: 20000
                            }).then(location => {
                                gMap.animateToLocation(location);
                            });
                        }
                    });
                });
            }
        },
	onMarkerSelect() {},
        onMarkerInfoWindowTapped() {},
```

一旦 map 组件初始化，就会运行`onMapReady`函数。首先，我们存储一个对 map 组件的引用，供以后其他函数使用。然后，我们配置地图以显示当前用户位置，并在每个平台上启用一个浮动位置按钮，用于将地图置于当前用户位置的中心。最后，我们启用指南针按钮的显示以将地图指向北方(仅在用户旋转地图时显示)并允许缩放手势。你会注意到，对于 iOS，我们已经添加了一个小的解决方法来修复这个插件的一个错误，这个错误会影响较新的 iOS 版本，并在点击按钮时阻止地图居中。在模拟器上运行该应用程序，您现在应该会看到一个蓝色的脉冲点，显示您设备的当前位置，以及用于将地图置于该位置中心的浮动按钮。

![](img/23533246507bf54f7f3242749b4ef6b9.png)

立即在 Android 模拟器上运行该应用程序。你可能会遇到一些问题，这些问题会使地图插件挂起或崩溃。为了避免此插件在 Android 上的一个常见问题，您应该在`app/App_Resources/Android/before-plugins.gradle`文件中设置 Google Play 服务的版本(如果该文件不存在，只需创建它):

```
android { 
    project.ext { googlePlayServicesVersion = "16.+" } 
}
```

一旦准备好了，你会在 Android 上遇到另一个问题，当你试图显示当前用户的位置，而在`onMapReady()`中没有一个位置的许可时。为了避免这种情况，我们将使用一个在`mounted()`中解决权限状态后设置的标志`isMounted`，并且只有在用户授予位置权限并且 MapView 已经初始化后才启用 Android 上的当前位置设置。这种方法可能会导致一种竞争情况，因为我们期望地图插件在`mounted()`中的权限检查之前完成加载，否则，按钮可能不会被启用和显示。为了避免这种情况，我们还将检查`mapView`变量是否已经在`mounted()`中赋值，并且在这里启用该设置之前权限检查是否已经完成。在一个更现实的应用程序中，你可能会用一个加载器或`v-if`隐藏地图屏幕，直到其他一切准备就绪，地图可以开始初始化。更新`default`部分，使其包含以下更改:

```
 data() {
        return {
            latitude: '',
            longitude: '',
            zoom: '',
            bearing: '',
            tilt: '',
            mapView: null,
            isMounted: false
        }
    },
    mounted() {
        let that = this
        geolocation.isEnabled().then(function(isEnabled) {
            if (!isEnabled) {
                geolocation.enableLocationRequest(true, true).then(() => {
                    that.isMounted = true
                    if (isAndroid && that.mapView) {
                        let uiSettings = that.mapView.gMap.getUiSettings();
                        uiSettings.setMyLocationButtonEnabled(true);
                        that.mapView.gMap.setMyLocationEnabled(true);
                    }
                    geolocation
                        .getCurrentLocation({
                            timeout: 20000
                        })
                        .then(location => {
                            if (!location) {
                                console.log("Failed to get location!");
                            } else {
                                that.latitude = location.latitude
                                that.longitude = location.longitude
                                that.zoom = 14
                                that.bearing = 0
                                that.altitude = 0
                            }

                        });
                }, (e) => {
                    console.log("Error: " + (e.message || e));
                }).catch(ex => {
                    console.log("Unable to Enable Location", ex);
                });
            } else {
                that.isMounted = true
                if (isAndroid && that.mapView) {
                    let uiSettings = that.mapView.gMap.getUiSettings();
                    uiSettings.setMyLocationButtonEnabled(true);
                    that.mapView.gMap.setMyLocationEnabled(true);
                }
                geolocation
                    .getCurrentLocation({
                        timeout: 20000
                    })
                    .then(location => {
                        if (!location) {
                            console.log("Failed to get location!");
                        } else {
                            that.latitude = location.latitude
                            that.longitude = location.longitude
                            that.zoom = 14
                            that.bearing = 0
                            that.altitude = 0
                        }

                    });

            }
        }, function(e) {
            console.log("Error: " + (e.message || e));
        });

    },
    methods: {
        onMapReady(args) {
            this.mapView = args.object;
            var gMap = this.mapView.gMap;
            this.mapView.settings.myLocationEnabled = true;
            this.mapView.settings.myLocationButtonEnabled = true
            this.mapView.settings.compassEnabled = true
            this.mapView.settings.zoomGesturesEnabled = true;
            if (isAndroid && this.isMounted && geolocation.isEnabled()) {
                let uiSettings = gMap.getUiSettings();
                uiSettings.setMyLocationButtonEnabled(true);
                gMap.setMyLocationEnabled(true);
            }
            if (isIOS) {
                gMap.myLocationEnabled = true;
                gMap.settings.myLocationButton = true;
                this.mapView.on("myLocationTapped", event => {
                    geolocation.isEnabled().then(enabled => {
                        if (enabled) {
                            geolocation.getCurrentLocation({
                                maximumAge: 5000,
                                timeout: 20000
                            }).then(location => {
                                gMap.animateToLocation(location);
                            });
                        }
                    });
                });
            }
        },
        onMarkerSelect() {},
        onMarkerInfoWindowTapped() {},
    },
```

在 Android 模拟器上运行该应用程序，您应该会看到类似于以下内容的内容:

![](img/c56d8b2b07be115d362c8cb2c6eb4458.png)

# 添加地图标记

我们已经有了一张显示我们当前位置的地图，但现在让我们为感兴趣的位置添加标记来使它变得有用。让我们将模拟器中的当前位置设置为美国加利福尼亚州山景城圣安东尼奥路 645 号的坐标。我们将使用示例地图标记，通过在 Google Maps 中搜索杂货店找到该位置附近的几个地方，并记下名称和位置信息。将以下导入添加到脚本部分的顶部:

`const mapsModule = require("nativescript-google-maps-sdk");`

然后添加一个新的数据变量来存储要由我们的应用程序呈现的标记数组:

```
 markers: [{
                    name: 'Whole Foods',
                    address: '4800 El Camino Real',
                    city: 'Los Altos',
                    state: 'CA',
                    zip: '94022',
                    type: 'Grocery Store',
                    latitude: '37.398933',
                    longitude: '-122.110570',
                },
                {
                    name: 'Trader Joe\'s',
                    address: '590 Showers Dr',
                    city: 'Mountain View',
                    state: 'CA',
                    zip: '94040',
                    type: 'Grocery Store',
                    latitude: '37.402180',
                    longitude: '-122.110888',
                },
                {
                    name: 'Safeway',
                    address: '645 San Antonio Rd',
                    city: 'Mountain View',
                    state: 'CA',
                    zip: '94040',
                    type: 'Grocery Store',
                    latitude: '37.402079',
                    longitude: '-122.111946',
                },
                {
                    name: 'Walmart',
                    address: '600 Showers Dr',
                    city: 'Mountain View',
                    state: 'CA',
                    zip: '94040',
                    type: 'Grocery Store',
                    latitude: '37.400774',
                    longitude: '-122.109642',
                },
            ]
```

现在我们将使用添加到`onMapReady`末尾的以下代码添加标记:

```
 this.markers.forEach(element => {
                var marker = new mapsModule.Marker();
                marker.position = mapsModule.Position.positionFromLatLng(
                    element.latitude,
                    element.longitude
                );
                marker.title = element.name;
                this.mapView.addMarker(marker);
            })
```

现在，如果您运行该应用程序，您应该会看到类似以下内容:

![](img/9044156f3e9969865c2106aa145e79c9.png)

点击标记时，地图会以该标记为中心，并显示该标记的标题。我们可以通过弹出一个包含更多信息的窗口来做得更好，就像你通过他们的应用程序或网站使用谷歌地图时得到的那些信息一样。将刚刚添加的代码块替换为:

```
 this.mapView.infoWindowTemplate = `<StackLayout orientation="vertical" width="240" height="140"  >
        	    <Label text="{{title}}"  marginTop="26" marginLeft="20"    textWrap="true" color="black"  fontSize="18" />
                <Label text="{{type}}"  marginLeft="20"    textWrap="true" color="gray"  fontSize="12" />
                <Label text="{{address}}"  marginLeft="20"    textWrap="true" color="gray"  fontSize="14" />
            </StackLayout>`;
            this.markers.forEach(element => {
                var marker = new mapsModule.Marker();
                marker.position = mapsModule.Position.positionFromLatLng(
                    element.latitude,
                    element.longitude
                );
                marker.title = element.name;
                marker.type = element.type
                marker.address = element.address + ' ' + element.city + ',' + element.state + ' ' + element.zip
                this.mapView.addMarker(marker);
            })
```

我们添加了一个用于弹出窗口的 XML 模板，它引用了我们添加到每个标记对象的属性。您可以在这些弹出窗口中尝试样式化和使用本地图像，如果您想要执行诸如导航到该位置的详细信息页面之类的操作，还可以实现标记点击处理程序。

![](img/2439c67a94ec00964226adb6488c31ff.png)

# 带有 Google Places 的搜索框

让我们使这个地图更具交互性，并添加一个连接到 Google Places 的搜索框来查找结果并根据这些结果渲染标记。我们将需要为 Google Places 获取另一个 API 密钥，这将需要与 Google Maps 密钥相同的步骤，除了这一次我们将只生成一个由 API 限制的仅用于 Google Places 的密钥，并将其用于两个平台。一旦获得了这个键，就将下面的内容添加到 XML 部分的`<StackLayout>`内和`MapView`组件之上:

```
<SearchBar hint="What are you looking for?" v-model="searchPhrase" @submit="onSubmit" textFieldHintColor="gray" marginTop="20" />
```

这将在页面顶部呈现一个[搜索栏组件](https://nativescript-vue.org/en/docs/elements/components/search-bar/)，并将搜索查询绑定到一个新的本地变量`searchPhrase`，我们将把它作为`searchPhrase: '',`添加到数据导出部分。在脚本部分的顶部添加一个新的导入，这样我们就可以进行 HTTP 调用:`import * as http from "http";`。我们需要建立一个到 API 服务器的 URL 请求，其中包含当前用户位置、1000 米结果的最大距离、用户提交的搜索查询和受限的 Google Places API 密钥。在首先移除所有旧标记后，返回的任何结果都将被解析并作为新标记添加到地图上。这将发生在添加到方法部分的新的`onSubmit`函数中:

```
 onSubmit() {
            let that = this
            this.mapView.removeAllMarkers()
            let searchurl = "https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=" + this.latitude + "," + this.longitude + "&radius=1000&keyword=" + encodeURI(this.searchPhrase) + "&key=YOUR_API_KEY"
            http
                .request({
                    url: searchurl,
                    method: "GET",
                    timeout: 10000,
                    headers: { "Content-Type": "application/json" }
                })
                .then(function(data) {
                    if (data.statusCode == 200) {
                        let result = JSON.parse(data.content);
                        let results = result.results
                        results.forEach(element => {
                            var marker = new mapsModule.Marker();
                            marker.position = mapsModule.Position.positionFromLatLng(
                                element.geometry.location.lat,
                                element.geometry.location.lng
                            );
                            marker.title = element.name;
                            marker.type = element.types[0]
                            marker.address = element.vicinity
                            that.mapView.addMarker(marker);
                        })

                    } else {
                        console.log("Error requesting youtube metadata! Returned: ");
                        console.dir(data);
                    }
                })
                .catch(e => {
                    console.log("Error retreiving youtube metadata");
                    console.error(e);
                });
        },
```

这个函数首先使用当前用户位置、1 km 的搜索半径、搜索查询和 API 键为 Google Places API 调用构建查询字符串。您可以在此阅读更多关于[如何使用和解释此次通话结果的信息。](https://developers.google.com/places/web-service/search)调用完成后，我们将检查有效的状态代码，然后通过创建新标记并将它们添加到地图来处理返回的任何结果。从数据段中删除示例`markers`变量数组，并删除在`onMapReady`中添加这些示例标记的代码。现在运行代码并搜索“杂货店”,您应该会看到类似这样的内容:

![](img/d5d846ca0855247cadb8c270aa8fec2f.png)

到目前为止，这是可行的，但是如果用户四处移动，那么我们在加载应用程序时获得的初始位置将不再准确。为了确保我们发送当前位置的搜索结果，我们可以在发出查询之前从设备请求当前位置，确保我们将获得当前位置的结果，而不是过去的结果。将`onSubmit`功能修改为:

```
 onSubmit() {
            let that = this
            geolocation
                .getCurrentLocation({
                    timeout: 20000
                })
                .then(location => {
                    if (!location) {
                        console.log("Failed to get location!");
                    } else {
                        that.latitude = location.latitude
                        that.longitude = location.longitude
                        that.mapView.removeAllMarkers()
                        let searchurl = "https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=" + that.latitude + "," + that.longitude + "&radius=1000&keyword=" + encodeURI(that.searchPhrase) + "&key=AIzaSyCnnXo6rqBp4ItB__nC80OyoLDYjxWcxBQ"
                        http
                            .request({
                                url: searchurl,
                                method: "GET",
                                timeout: 10000,
                                headers: { "Content-Type": "application/json" }
                            })
                            .then(function(data) {
                                if (data.statusCode == 200) {
                                    let result = JSON.parse(data.content);
                                    let results = result.results
                                    results.forEach(element => {
                                        var marker = new mapsModule.Marker();
                                        marker.position = mapsModule.Position.positionFromLatLng(
                                            element.geometry.location.lat,
                                            element.geometry.location.lng
                                        );
                                        marker.title = element.name;
                                        marker.type = element.types[0]
                                        marker.address = element.vicinity
                                        that.mapView.addMarker(marker);
                                    })

                                } else {
                                    console.log("Error getting google places data");
                                    console.dir(data);
                                }
                            })
                            .catch(e => {
                                console.log("Error getting google places data");
                                console.error(e);
                            });
                    }

                });
        },
```

虽然这确实可行，但更好的替代方法是使用地理定位插件功能`watchLocation`设置一个位置监视器，并在设备位置发生变化时在后台更新应用程序的当前用户位置。

# 给地图加边界

您可能想要实现的另一个功能是将地图缩放并居中，以便它适合从搜索结果返回的所有标记。这将需要特定于每个平台的本地调用来从所有标记创建一个新的边界框，然后将其应用到`MapView`以将其置于该框的中心。对于 Android，一旦所有标记都添加到地图上，我们就可以计算边界框。对于 iOS，我们需要在添加所有标记之前、期间和之后执行操作。更新`onSubmit`到:

```
onSubmit() {
            let that = this
            geolocation
                .getCurrentLocation({
                    timeout: 20000
                })
                .then(location => {
                    if (!location) {
                        console.log("Failed to get location!");
                    } else {
                        that.latitude = location.latitude
                        that.longitude = location.longitude
                        that.mapView.removeAllMarkers()
                        let searchurl = "https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=" + that.latitude + "," + that.longitude + "&radius=1000&keyword=" + encodeURI(that.searchPhrase) + "&key=AIzaSyCnnXo6rqBp4ItB__nC80OyoLDYjxWcxBQ"
                        http
                            .request({
                                url: searchurl,
                                method: "GET",
                                timeout: 10000,
                                headers: { "Content-Type": "application/json" }
                            })
                            .then(function(data) {
                                if (data.statusCode == 200) {
                                    let result = JSON.parse(data.content);
                                    let results = result.results
                                    var bounds
                                    let padding = 100
                                    if (isIOS) {
                                        bounds = GMSCoordinateBounds.alloc().init();
                                    }
                                    results.forEach(element => {
                                        var marker = new mapsModule.Marker();
                                        marker.position = mapsModule.Position.positionFromLatLng(
                                            element.geometry.location.lat,
                                            element.geometry.location.lng
                                        );
                                        if (isIOS) bounds = bounds.includingCoordinate(marker.position);
                                        marker.title = element.name;
                                        marker.type = element.types[0]
                                        marker.address = element.vicinity
                                        that.mapView.addMarker(marker);
                                    })
                                    if (isAndroid) {
                                        var builder = new com.google.android.gms.maps.model.LatLngBounds.Builder();
                                        that.mapView.findMarker(function(marker) { builder.include(marker.android.getPosition()); });
                                        bounds = builder.build();
                                        var cu = com.google.android.gms.maps.CameraUpdateFactory.newLatLngBounds(bounds, padding);
                                        that.mapView.gMap.animateCamera(cu);
                                    }
                                    if (isIOS) {
                                        var update = GMSCameraUpdate.fitBoundsWithPadding(bounds, padding);
                                        that.mapView.gMap.animateWithCameraUpdate(update);
                                    }
                                } else {
                                    console.log("Error getting google places data");
                                    console.dir(data);
                                }
                            })
                            .catch(e => {
                                console.log("Error getting google places data");
                                console.error(e);
                            });
                    }

                });
        },
```

# 自定义标记图标

除了为每个标记定制弹出窗口，我们还可以更进一步，定制标记本身的外观。我们可以做的最简单的改变是修改标记图标的颜色。为此，我们可以使用`marker.color="COLOR"`简单地给每个标记分配一种颜色。让我们将标记添加到地图的循环中，通过将`marker.color="green"`添加到`onSubmit`来将所有标记更改为绿色而不是红色。现在你的地图和标记应该看起来像:

![](img/4924963e1723950b7240971a6db4b5bd.png)

使用图像几乎同样简单，尽管你必须小心使用的图像大小以及它们在 iOS 和 Android 平台上的呈现方式。让我们注释掉标记颜色分配，并添加以下行:

```
// marker.color = "green"
let imageSource = ImageSource.fromFileSync("~/images/foodicon.png");
const icon = new Image();
icon.imageSource = imageSource;
marker.icon = icon;
that.mapView.addMarker(marker);
```

我们已经下载了一个 48x48 的图标图像，并将其放在`/app/images/`目录中。我们还需要一些 Nativescript 图像库，因此将这些导入添加到脚本部分:

```
import { ImageSource } from "tns-core-modules/image-source";  
import { Image } from "tns-core-modules/ui/image";
```

运行应用程序，执行搜索，您现在应该有带此图标的标记，而不是默认的大头针标记:

![](img/2caf320443cf79312971ce96c2f2712c.png)

# 完成的

这个帖子到此为止。如果你想下载最终的源文件，你可以在 [Github](https://github.com/drangelod/ns6maps) 上找到。

*原载于 2020 年 1 月 16 日 https://blog.angelengineering.com*[](https://blog.angelengineering.com/nativescript-maps/)**。**