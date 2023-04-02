# 了解 JavaScript 代理

> 原文：<https://javascript.plainenglish.io/understanding-javascript-proxies-69d07f812666?source=collection_archive---------6----------------------->

## JavaScript ES6 代理和可撤销代理的快速介绍

![](img/6b9057b11f2353f6948638498c4c16b1.png)

代理是 ES6 包含的特性之一，虽然不太为人所知，但它非常强大。代理是我们创建的一种特殊的对象，它“包装”了另一个对象。我们可以在代理对象上注册特殊的处理程序，当针对代理执行各种操作时会调用这些处理程序。

代理允许您进行元编程操作，例如拦截检查或更改对象属性的调用。简单地说，代理就像超级维生素化的“getters”和“setters ”,它们拦截对属性的调用并执行动作。

多亏了代理，我们可以做这样的事情:

*   拦截对属性的调用
*   动态验证
*   …

代理构造函数接受两个参数:源对象和充当源对象处理程序的对象。处理程序对象包含称为陷阱的方法。

例如，traps([Complete list](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Proxy#M%C3%A9todos_del_objeto_handler))“get”和“set”分别拦截对属性的调用以获取和设置值，允许我们在这个过程之前和过程中设置逻辑。

让我们看一个没有使用代理和使用代理的例子，其中我们将获得用大写字母格式化的对象的属性值，并且我们将验证电子邮件到用户对象的分配。

没有代理:

```
const user = {
 _name : "Templeton",
 _email : "[faceman@gmail.com](mailto:faceman@gmail.com)",get name() {
    return this._name.toUpperCase();
 },get email() {
  return this._email.toUpperCase();;
 },set name(newName) {
    //Apply some validations
    //...
    console.log("new name: "   
    +newName);
    this._name= newName;
  },set email(newEmail) {
    console.log("new email: " 
    + newEmail);    
    if(!newEmail.includes('@')){
      console.log("Incorrect mail.  
      Nothing has been assigned!");    
    }else{   
      this._email = newEmail;
    }
  }
}//GETTERS
user.name; //TEMPLETON
user.email; //[FACEMAN@GMAIL.COM](mailto:FACEMAN@GMAIL.COM)
user.otherProperty; //undefined !!//SETTERS
user.email ="bademail#gmail.com"
// Incorrect mail. Nothing has been assigned!user.email; //[FACEMAN@GMAIL.COM](mailto:FACEMAN@GMAIL.COM)
user.email = "[ateam@gmail.com](mailto:ateam@gmail.com)"
user.email //[ATEAM@GMAIL.COM](mailto:ATEAM@GMAIL.COM)
```

现在，我们将看到相同的使用情形，但使用代理:

```
const user = {
  name :"Templeton",
  email : "[faceman@gmail.com](mailto:faceman@gmail.com)"
}const handler = {
  get: (target, name) => {
    return name in target ?
      target[name].toUpperCase() :
      console.log('The property not exists');
  },set: (target, name, newValue) => {
    if(name ==='email'){
      if(!newValue.includes('@')){  
       console.log("Incorrect mail. Nothing has been assigned!");
      }else{target[name] = newValue}
    }else if(name != email){
      target[name] = newValue;
    }    
  }
}const userProxy = new Proxy(user, handler);//GETTERS
userProxy.name; //TEMPLETON
userProxy.email; //[FACEMAN@GMAIL.COM](mailto:FACEMAN@GMAIL.COM)
userProxy.otherProperty; //The property not exist//SETTERS
userProxy.email ="bademail#gmail.com"
//Incorrect mail. Nothing has been assigned!userProxy.email; //[FACEMAN@GMAIL.COM](mailto:FACEMAN@GMAIL.COM)
userProxy.email ="[ateam@gmail.com](mailto:ateam@gmail.com)"
userProxy.email //[ATEAM@GMAIL.COM](mailto:ATEAM@GMAIL.COM)
```

正如我们在最后一个使用代理的例子中看到的，我们不需要为用户对象中的每个属性定义一个 getter 或 setter，如果属性不存在，我们也不会获得 undefined。

## 可撤销的代理

常规代理在创建后不能修改；但是，在某些情况下，我们可能希望创建一个可以被禁用的代理。为此，我们可以创建一个可撤销的代理:

我们可以用 Proxy.revocable(..)，这是一个常规函数，而不是 Proxy 之类的构造函数(..).采用与常规“代理”相同的两个参数，并返回一个具有两个属性的对象:proxy 和 revoke。

```
const user = {
  name :"Templeton",
  email : "[faceman@gmail.com](mailto:faceman@gmail.com)"
}const handler = {

 get: (target, name) => {
    return name in target ?
      target[name].toUpperCase() :
      console.log('The property not exists');
 },set: (target, name, newValue) => {
    if(name ==='email'){
      if(!newValue.includes('@')){  
       console.log("Incorrect mail.  
       Nothing has been assigned!");
      }else{target[name] = newValue}
    }else if(name != email){
      target[name] = newValue;
    }    
  }
}const revocableUserProxy = Proxy.revocable(user, handler);//GETTERS
user.name; //TEMPLETON
revocableUserProxy.revoke();
user.name; //Templeton
```

在前面的示例中，当我们访问 user.name 属性时，代理被调用，我们获得大写字母的属性值，然后，如果我们调用代理的 revoke 方法并再次访问 user.name 属性，我们获得 user.name 属性的原始值。

## 结论

代理的能力可能不明显，但是它们提供了强大的元编程机会。所有当前的浏览器和节点都支持代理特性，因此我们可以在开发中使用它来获得更干净、更简洁的代码。

考虑到并非所有的浏览器都支持所有的陷阱，这一点也很重要。我们可以在 MDN 代理页面上查看 trap [兼容性表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Browser_compatibility)。另一个缺点是，由于 ES5 的限制，代理不能被传输(例如，用[巴别塔](https://babeljs.io/))或聚合填充。

非常感谢您阅读这篇文章——希望您觉得有用！

## **用简单英语写的 JavaScript 笔记**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到[**submissions@javascriptinplainenglish.com**](mailto:submissions@javascriptinplainenglish.com)给我们，我们会把你添加为作者。

我们还推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[](https://medium.com/ai-in-plain-english)**[**UX**](https://medium.com/ux-in-plain-english)[**Python**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****