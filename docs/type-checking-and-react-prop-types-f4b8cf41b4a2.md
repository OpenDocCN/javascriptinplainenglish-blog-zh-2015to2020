# 类型检查和反应正确类型

> 原文：<https://javascript.plainenglish.io/type-checking-and-react-prop-types-f4b8cf41b4a2?source=collection_archive---------3----------------------->

## 什么是类型检查？

![](img/27fe54ba5be561846fb1f0385d67b2b9.png)

类型检查是在运行时(动态)或编译时(静态)验证数据类型的过程。这是为了确保程序能够成功执行。

*静态类型检查*是在编译时已知变量的类型。这意味着一些语言需要用数据类型初始化变量。

## 静态类型语言:

*   C
*   C++
*   C#
*   Java 语言(一种计算机语言，尤用于创建网站)

更快地捕捉错误要容易得多，因为你的程序将无法编译。

*动态类型检查*是在运行时知道变量的类型。

## 动态类型语言:

*   java 描述语言
*   计算机编程语言
*   红宝石
*   左上臂

错误是在运行时发现的，这意味着你的程序可以编译，但可能无法正常执行。

## 什么是反应类型？

在 React Native 中，可以使用 PropTypes 进行类型检查。您可以通过为传递到组件中的属性分配一个数据类型来使用它们。

使用 PropTypes 的示例:

```
import * as React from ‘react’;
 import { Text, View } from ‘react-native’;
 import PropTypes from ‘prop-types’;

 export default class HelloWorld extends React.Component {
    render() {
       return (
          <View>
             <Text>
             {this.props.helloWorld}
             </Text>
          </View>
      );
    }
 }

 HelloWorld.propTypes = {
    helloWorld: PropTypes.string,
 }
```

HelloWorld PropType 设置为 string。如果一个错误的属性被传递到组件中，它会给出一个错误或警告。

## 我如何使用它们？

第一步是安装 React 提供的 prop-types 包。

`npm install — save prop-types`

**或**

`yarn add prop-types`

一旦将包添加到您的项目中，我们就可以将 PropTypes 导入到您的组件中。`js`文件。文件顶部的导入`prop-types`如下所示。

`import { PropTypes } from ‘prop-types’;`

现在是时候向组件添加 PropTypes 了。

```
ComponentName.propTypes = { 
   propName: PropTypes, // Just a template
   name: PropTypes.string,  // String PropType
   age: PropTypes.number,   // Number
   isWorking: PropTypes.bool // Boolean
}
```

差不多就是这么多了。还有很多你可以使用的 prop 类型，我没有列出来，但是官方的 React [文档](https://reactjs.org/docs/typechecking-with-proptypes.html)有。

## 结论

属性类型可以方便地防止错误的数据类型作为属性传入。我数不清在一个对象中传递了多少次，而这个对象应该是一个数组，反之亦然。添加 PropTypes 将有助于更快、更早地发现这些错误。