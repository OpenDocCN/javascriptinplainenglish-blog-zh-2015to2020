# 在 React 本机应用程序中检查互联网连接

> 原文：<https://javascript.plainenglish.io/check-internet-connectivity-easily-on-react-native-application-b9f0de162dd9?source=collection_archive---------1----------------------->

## 安装—实施—测试

![](img/a0e7bb4a7171f1ee5d79aff4b9d03e7f.png)

Photo by [Oleg Magni](https://unsplash.com/@olegmagni?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/internet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 装置

**安装**[react-native-netInfo](https://github.com/react-native-community/react-native-netinfo)

```
yarn add @react-native-community/netinfocd ios && pod install && cd .. # iOS pod installation
```

**android/build.gradle** 的变化

```
buildscript {
  ext {
    buildToolsVersion = "28.0.3"
    minSdkVersion = 16
    compileSdkVersion = 28
    targetSdkVersion = 28
    # Only using Android Support libraries
    supportLibVersion = "28.0.0"
  } 
```

# 履行

创建文件名 **NetworkUtils.js**

```
import NetInfo from "@react-native-community/netinfo";export default class NetworkUtils { static async isNetworkAvailable() {
    const response = await NetInfo.fetch();
    return response.isConnected;
}}
```

现在在必要的地方调用它

```
const isConnected = await NetworkUtils.isNetworkAvailable()
```

# 测试

在 **package.json** 文件中添加

```
"jest": {
 "preset": "react-native",
 "setupFiles": [
   "<path>/jestSetupFile.js"
],
```

创建新文件**jessetupfile . js**并添加这个

```
import mockRNCNetInfo from '@react-native-community/netinfo/jest/netinfo-mock.js';

jest.mock('@react-native-community/netinfo', () => mockRNCNetInfo);
```

现在 jest 可以测试了。

NetworkUtils Class

测试案例

NetworkUtils Test Cases

# 感谢阅读！