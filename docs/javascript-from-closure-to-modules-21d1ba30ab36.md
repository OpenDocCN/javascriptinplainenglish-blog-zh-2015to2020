# 闭包如何导致 JavaScript 中的模块

> 原文：<https://javascript.plainenglish.io/javascript-from-closure-to-modules-21d1ba30ab36?source=collection_archive---------6----------------------->

![](img/90fc654f27978cdcfb69e2ac31118870.png)

在这篇博客中，我们将看到闭包在 JavaScript 中是如何操作的，以及它如何导致最强大的代码组织模式之一，模块。如果你不熟悉 JavaScript 的词法范围机制，我建议你先在这里阅读一下[。](https://medium.com/javascript-in-plain-english/javascript-my-favorite-compiled-language-a-blog-about-scope-2012071aac86)

闭包是计算机科学有史以来发明的最重要的思想之一，让我们看看为什么。

**闭包定义:**当一个函数能够**记住** **并且** **访问**它的词法作用域时，即使该函数在不同的作用域中执行。

*一个* ***会认为*** *当你返回一个函数，或者执行它的时候，它最初被定义的作用域已经消失了。但是结果是，那个作用域根本没有消失，无论你在哪里传递那个函数，它都可以继续访问那个作用域。*

无论在哪里传递该值/执行该功能，对其定义的作用域的保留/链接都被理解为闭包。

两个简单的例子:

```
********** Example 1 ************function ask(question){
    return function encloseQuestion(){
        console.log("My question is: ", question)
    }
}var logMyQuestion = ask("What is closure ?")logMyQuestion() // My question is:  What is closure ?********** Example 2 ************function outerFunction(){
    var number = 1
    return function innerFunction(){
        number ++
        console.log(number)
    }
}
var logNumber = outerFunction()
logNumber() // 2
logNumber() // 3
logNumber() // 4
```

我认为这里最值得注意的是，我们没有将变量传入内部函数，但是我们仍然可以访问它们。我们保留对变量的访问。

据说`encloseQuestion`和`innerFunction`函数**分别关闭`question`和`number`变量**。

非常重要:我们**不关闭值**，我们**关闭变量**。这是**而不是**的快照！我们可以访问当时的变量值，而不是它在创作时的值！

# 模块模式

现在我们理解了闭包是如何工作的，我们可以开始深入研究**模块模式**。

模块模式是将一组**行为(**功能)和**数据**组合成一个逻辑单元。

先说什么**不是**一个模块。

对功能和行为进行分组的最简单方法是创建一个对象，并将您的数据和行为放入其中。这就是所谓的**名称空间**模式。

```
var nameSpaceExample = {
    name: "Mark",
    logName: function (){
        console.log(this.name);
    }
}
```

那就是**不是**一个模块，因为模块模式需要**封装**的概念。

**封装**是对模块用户隐藏**数据和行为的想法。**

**模块模式因此**既需要**绑定**我们的数据和行为，**又需要**和**封装。**

因此，一个模块允许向你的代码的用户提供一些数据和行为，而拒绝其他数据和行为。

“经典”或“揭示”模块模式如下:“模块将数据和行为(方法)封装在一起。模块的状态(数据)由模块的方法通过**闭包**保存。

让我们来看一个例子并进行分解:

```
function PetShopModule(animal){
    var publicAPI = {animalName, };
    return publicAPI;

   function animalName(name){
       console.log(`${name} is a ${animal}`)
   }
}

var animal = PetShopModule("Dog")
animal.animalName("Kim")   // "Kim is a Dog"
```

*   我们定义了一个`PetShopModule`，将第一个字母大写以区别于传统函数。`PetShopModule`将负责保存**数据**和**功能**，通过确定返回什么，我们能够**允许** / **限制**访问**数据**和**功能**。
*   在`PetShopModule`函数中，我们定义了一个函数`animalName`，它将关闭作用域内的变量，在本例中是`animal`(作为参数传递给`PetShopModule`)和`publicAPI`。
*   我们将返回一个对象`var publicAPI`，它将保存我们选择提供给模块用户的功能。
*   当我们声明`var petShop = PetShopModule("Dog")`时，我们实际上是在创建我们的`PetShopModule`的一个实例，并传入`"Dog"`来设置为我们的`animalName`函数可以访问的`var name`。
*   由于调用`PetShopModule("Dog")`的`return`值是一个带有键`animalName`的`Object`，我们的`animal variable`现在持有`{animalName: animalName}`，这里的值是对我们的`animalName`函数的引用。
*   这意味着我们可以调用`petShop.animalName("Kim")`，其中`animalName` **可以访问**到当我们声明`var petShop = PetShopModule("Dog")`时传入的`var animal`。

这就是我们如何捆绑数据和行为。

这种模式允许的另一个非常重要的特性是**跟踪状态随时间的变化**。

例如，我们可以将`var animal`的值改为`"Cat"`。

```
animal = PetShopModule("Cat")
animal.animalName("Kim") // "Kim is a Cat"
```

现在让我们看看如何拒绝行为的访问。

```
function PetShopModule(animal){
    var publicAPI = {animalName, };
    return publicAPI;

   function animalName(name){
       logDate.call(this)
       console.log(`${name} is a ${animal}`)
   }

    function logDate(){
        var date = new Date();
        console.log("Today's date is: ", date)
    }
}

var petShop = PetShopModule("Dog")
petShop.animalName("Kim")
```

我们可以看到我们的`logDate`函数是如何对我们模块中的函数可用的，但是**没有形状或形式**对我们的用户可用。那就是**封装。**

# ES6 模块

添加 ES6 模块是因为模块被证明是主要的代码组织模式。

ES6 带来了基于文件的模块；你打开一个文件，开始创建变量和函数。

您可以想象这个文件被包装在一个函数中，就像我们在前面的例子中看到的那样。

因为这个文件是作为一个模块加载的，所以它被认为是私有的。

公开的方法是使用`export`关键字。

就拿这个`PetShop.js`模块来说吧。

```
var animal = "Dog";
​
export default function petName(name){
    console.log(`${name} is a ${animal}`)
}
```

值得注意的是，这些基于文件的模块也是**单例的**，这意味着即使它们被多次导入到应用程序中，**它们也只运行一次**。

在 ES6 中有两种主要的导入和使用模块的方式，选择哪种方式是个人喜好的问题。

```
// named import syntax
import petName from 'PetShop.mjs';
​
petName("Kim");
​
// Or
​
// Namespaced import syntax
import * as PetShop from 'PetShop.mjs'
​
var pet = PetShop.petName("Kim");
```

我希望这有所帮助。

博客灵感来自[前端大师](https://frontendmasters.com/) / [前端大师](https://medium.com/u/1b199ed2dfd?source=post_page-----21d1ba30ab36--------------------------------)，深度 JavaScript 基础，v3，由 [Kyle](https://medium.com/u/5dccb9bb4625?source=post_page-----21d1ba30ab36--------------------------------) Simpson 教授。对于那些想大幅增加他们的前端编程知识的人来说，这是一个惊人的资源。

感谢阅读！

爱，光，代码❤️

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****