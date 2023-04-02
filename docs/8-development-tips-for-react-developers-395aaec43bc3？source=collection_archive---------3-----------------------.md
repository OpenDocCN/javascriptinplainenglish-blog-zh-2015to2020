# React 开发人员的 8 个开发技巧

> 原文：<https://javascript.plainenglish.io/8-development-tips-for-react-developers-395aaec43bc3?source=collection_archive---------3----------------------->

![](img/91ec6f46d7cb9ab884afe036e81734bd.png)

Photo by [Form](https://unsplash.com/@theformfitness?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.用 React 中的 setState 更新对象

## >例如

鉴于这种情况，

```
this.state = {jasper: {name: 'jasper', age:28}}
```

您可以使用两种方法更新对象

1.  `Object.assign`
2.  传播算子

## **1 > Object.assign**

```
this.setState(prevState => { let jasper = Object.assign({}, prevState.jasper) jasper.name = 'someothername' return { jasper }})
```

## 2 >扩展运算符

```
this.setState(prevState => ({ jasper: { ...prevState.jasper, name: 'something' }}))
```

# 2.在 React 中更新嵌套对象

*   `Object.assign` &传播算子只能创建浅层拷贝

## >例如

给定嵌套对象，

```
this.state = { food: { sandwich: { capsicum: true, crackers: true, mayonnaise: true }, pizza: { jalapeno: true, extraCheese: false } }}
```

*   更新披萨对象的 **extraCheese** (使用扩展运算符)

## >嵌套对象的扩展运算符

```
this.setState(prevState => ({ food: { ...prevState.food, pizza: { ...prevState.food.pizza, extraCheese: true } }}))
```

# 3.在子目录中部署应用程序

*   如果您想在子目录中部署应用程序，例如

```
https://myapp.com/directory-name
```

您可以使用两种方法在子目录中部署应用程序

1.  设定基地名称`basename`
2.  在`package.json`中设置 app `homepage`

## 1 >设置基本名称

```
<Router basename={'/directory-name'}> <Route path='/' component {Home} /> ...</Router>
```

*   在`<Router />`组件上设置`basename`属性告诉 React Router 将从子目录中提供应用程序

## 2 >在 package.json 中设置 app 主页

*   在`package.json`文件中，设置属性`homepage`
*   `npm run build`命令来自`creact-react-app`并且`npm run build`使用`homepage`属性确保生产构建指向正确的位置
*   默认情况下，CRA (Create React App)会生成一个版本，假设您的应用程序托管在服务器根目录
*   要覆盖它，请在您的`packge.json`中指定`homepage`

```
"homepage" : "http://mywebsite.com/relativepath"
```

# 4.渲染嵌套对象

*   有时，您会发现嵌套对象类型中的数据
*   与 JavaScript 中的数组相比，对象不太容易迭代
*   所以我建议你使用`Object.keys()`方法将对象转换成数组

给定嵌套对象，

```
const fruits = { apple: { price: 10, weight: 2.5, color: 'red', }, banana: { price: 5, weight: 1.2, color: 'yellow', },}
```

## > Object.keys()并使用键来映射

```
render() { Object.keys(fruits).map(function(fruit_name){ return( <tr key={fruit_name}> <tr> <td> <b> Template: {fruit_name} </b> </td> </tr> {fruits[fruit_name].items.map(function(item){ return( <tr key={item.id}> <td>{item}</td> </tr> ) })} </tr> ) })}
```

# 5.简单嵌套映射

```
const mscList = classroomSummary.map((classRoom, ind) => ( classRoom.map(classData => ( <MSCItem classData={classData} /> )) ))
```

# 6.检测路线变化

*   很多时候，您会发现需要检测路由更改并采取措施(即，每当有页面刷新或路由更改时，将状态“加载”更改为`false`)
*   要检测 React 中的路由更改，您需要使用`history`属性中的`listen`方法

举个例子，

*   每当有路线改变时，我想将我的状态从`expanded`改变到`false`

## >历史属性中的 listen 方法

```
constructor(props){ super(props); this.props.history.listen((location, action) => { **// console.log('history changed')** this.routeChange(); })}**// routeChange function**routeChange = () => { const { BaseActions } = this.props BaseActions.sideBar({expanded: false})}
```

# 7.历史推送而不是窗口。位置

*   `history.push`和`window.location`的主要区别在于

```
**// CLIENT SIDE**
history.push('/')**// SERVER SIDE** window.location('/')
```

*   使用历史 API `history.push('/')`刷新页面以获得更好的 UX

# 8.查询字符串

*   查询字符串通常用于在客户端和服务器端传递参数(数据)
*   安装— `npm install query-string`
*   我使用查询字符串通过选择的选项过滤数据
*   例如，如果用户选择`Europe`，我将`Europe`作为查询字符串`...URL...?Region=Europe`传递，并使用 URL 中`Region`的值过滤数据

## >查询字符串

*   使用 **history.push** 方法将**状态**或**属性**推送到 URL

```
resetBtn = () => { const { history, schoolid, 
          selectedAgeValue, selectedDomain } = this.props history.push(`/reports/disadmin/${schoolid}
    ?age=${selectedAgeValue}&domain=${selectedDomain}`)}
```

*   然后使用 queryString(例如`queryString.parse`方法)来使用它
*   在这个例子中，我获取了键**年龄** & **域**的值并更新 Redux

```
import queryString from 'query-string' const params = queryString.parse(window.location.search)const param_selectedAgeValue = params.ageconst param_selectedDomain = params.domainreturn state.set('param_selectedAgeValue', param_selectedAgeValue)
```

![](img/5be249a9cc6f78772cf391a39f24242b.png)

Photo by [Tyler Nix](https://unsplash.com/@jtylernix?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 谢谢大家！~