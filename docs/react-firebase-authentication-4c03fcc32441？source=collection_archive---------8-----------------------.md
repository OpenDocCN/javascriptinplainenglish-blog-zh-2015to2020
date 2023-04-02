# React Firebase 认证

> 原文：<https://javascript.plainenglish.io/react-firebase-authentication-4c03fcc32441?source=collection_archive---------8----------------------->

**Github:-**【https://github.com/swati1707/React-firebase-authentication】T2

这篇博客解释了如何使用 Firebase 来验证 React 应用程序中的用户。 **Firebase** 是一个后端即服务(Baas)应用开发平台，提供托管后端服务，如实时数据库、云存储、身份验证、崩溃报告、机器学习、远程配置和静态文件托管。

让我们开始吧。

## **第一步。创建一个 React 应用程序**

运行以下命令:-

```
npx create-react-app social-login
cd social-login
npm install firebase react-firebaseui
```

## **第二步。创建一个新的 Firebase 项目**

转到[https://console.firebase.google.com/u/0/](https://console.firebase.google.com/u/0/)并创建一个新项目。

![](img/393f8113dbe1be2b2e0dde59ead51a76.png)

然后转到验证选项卡，

![](img/01bf40bd1d87e13e51337832f191140e.png)

在这里，我们可以看到我们可以用来登录的所有提供者。我们将实现谷歌，脸书，Twitter，Github 和电子邮件登录。

## **第三步。启用脸书登录**

前往[https://developers.facebook.com/](https://developers.facebook.com/)并创建一个新的应用程序。你将获得一个应用 id 和应用密码。在这里补充一下。

![](img/4ce8dd3b742dbf0499b1f7799cd31d59.png)

## **第四步。启用 Twitter 登录**

前往[https://apps.twitter.com/](https://apps.twitter.com/)。创建新应用程序。

![](img/9fc68b02c1f2c05527c4d8a9cbc85a31.png)

在此添加 API 密钥和 API 秘密。

## **步骤五。启用 Github 登录**

登录 GitHub 并创建一个新的 OAuth 应用程序。你会得到一个客户 Id 和客户秘密。这里补充一下。

![](img/f57739ec6696b062e0cacfb942c19703.png)

## **第六步。启用谷歌登录**

![](img/9c71272e7ea6a01f90903f5e31c8039e.png)

## **第七步。配置登录方式**

**App.js**

```
import React from 'react';import './App.css';import firebase from "firebase";import StyledFirebaseAuth from "react-firebaseui/StyledFirebaseAuth";firebase.initializeApp({apiKey:"AIzaSyDtEF31ZsJu7GDfGr2BLttr-QkUBZBz3gg",authDomain:"fir-react-auth-eb41e.firebaseapp.com"})class App extends React.Component {state = {isSignedIn: false}uiConfig = {signInFlow: "popup",signInOptions: [firebase.auth.GoogleAuthProvider.PROVIDER_ID,firebase.auth.FacebookAuthProvider.PROVIDER_ID,firebase.auth.TwitterAuthProvider.PROVIDER_ID,firebase.auth.GithubAuthProvider.PROVIDER_ID,firebase.auth.EmailAuthProvider.PROVIDER_ID],callbacks: {signInSuccess: () => false}}componentDidMount = () => {firebase.auth().onAuthStateChanged(user => {this.setState({ isSignedIn: !!user })})}render() {return (<div className="data">{this.state.isSignedIn ?<div><h6>Signed in !</h6><button className="btn btn-primary logout" onClick={() => firebase.auth().signOut()}>Logout</button><h4>Welcome {firebase.auth().currentUser.displayName}</h4></div>:<StyledFirebaseAuthuiConfig={this.uiConfig}firebaseAuth={firebase.auth()}/>}</div>);}}export default App;
```

这里，我们通过添加以下命令，在应用程序中包含了 firebase 和 firebaseui

```
import firebase from "firebase";import StyledFirebaseAuth from "react-firebaseui/StyledFirebaseAuth";
```

导入后，我们已经初始化了 firebase。

接下来，我们配置了登录选项:

```
uiConfig = {signInFlow: "popup",signInOptions: [firebase.auth.GoogleAuthProvider.PROVIDER_ID,firebase.auth.FacebookAuthProvider.PROVIDER_ID,firebase.auth.TwitterAuthProvider.PROVIDER_ID,firebase.auth.GithubAuthProvider.PROVIDER_ID,firebase.auth.EmailAuthProvider.PROVIDER_ID],callbacks: {signInSuccess: () => false}}
```

根据登录状态，状态数据会发生变化，显示用户是否登录。

![](img/fdd531a64ab7dbb744077b20d49ad5f3.png)![](img/3815c0909c9d9266575aa16526663062.png)