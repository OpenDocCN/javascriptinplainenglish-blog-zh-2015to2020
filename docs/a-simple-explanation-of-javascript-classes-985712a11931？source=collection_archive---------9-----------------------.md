# JavaScript 类的简单解释

> 原文：<https://javascript.plainenglish.io/a-simple-explanation-of-javascript-classes-985712a11931?source=collection_archive---------9----------------------->

![](img/0e74a776ed8011c44b2934fefe999721.png)

您可以将 JavaScript 类想象成包含变量和函数房子。我们使用变量存储数字的原因和我们使用类存储函数的原因是一样的；我们希望能够创建一种模式，允许我们以有效的方式重用信息。

但是类是特殊的，因为它们就像一个蓝图，供其他人使用和构建很酷的东西。例如，我可以开设一个名为战士的课程。

我可以通过编写以下代码来“实例化”或创建该类的对象:

```
let bruceLee = Warrior.create(“Bruce Lee”);
bruceLee.kick();// Don’t worry. We’ll get to how to make a class after this quick example.
```

可以把一个实例化的对象想象成一种产品，比如从传送带上喷出来的可乐。每个对象都有通常的基本属性，如高度和宽度。但是有些东西可能是独特的，比如可乐的味道。

使用 Warrior 类，我可以创建一个全新的对象，而不必重写 kick 函数，因为它已经在一个类中定义了。

```
let chuckNorris = Warrior.create(“Chuck Norris”);
chuckNorris.kick(); 
```

但是我们首先如何创建一个类呢？在 JavaScript 中有几种创建类的方法。我们可以把它们分为旧方法和新方法。一些纯粹主义者更喜欢老方法，但我们会通过那座桥，当我们到达那里。

在 JavaScript 中创建类最流行的方法是使用 function 关键字，后跟一个大写的函数名:

```
 function Warrior(){
}
```

但是我们也可以通过使用对象文字来创建一个类:

```
 let Warrior = new Class({
}); 
```

或者我们可以通过使用自调用函数来创建一个类:

```
let Warrior = (function(){
})()
```

在 JavaScript 中构建类的新方法是像其他语言一样使用 class 关键字:

```
 class Warrior{
 constructor(){
 }
}
```

无论您选择如何创建一个类，类的组件通常都是相同的。对于本教程，我们将使用第一种方法，但是我将向您展示如何使用新的“class”方法来做同样的事情。

当构建一个类时，你首先要考虑的是这个类的基本特征应该是什么样的。比如你想做一个健身 app，你想大概知道用户的姓名，年龄，身高，体重。我们创建的每个新对象(或用户)都应该有这些描述。

我们将这些信息放在我们称之为构造函数的地方:

```
function User(){
//constructor
 this.name;
 this.height;
 this.age;
 this.weight;
} 
```

**注意**:我们使用“this”关键字，因为当创建一个新的用户对象时，我们希望获得实例化对象的值，而不是类本身。

让我们更进一步，允许某人通过向类添加一个参数来给类赋值:

```
function User(object){
//constructor
 this.name = object.name;
 this.height = object.height;
 this.age = object.age;
 this.weight = object.weight;
}
```

这种类型的类构建是一种很好的实践，因为它允许任何调用类的人能够像 build-a-bear 一样构建他们的对象，而不必担心破坏任何东西或以特定的顺序排列任何东西。例如:

```
 let bob = User({ height:”7'2", age: 30, name:”Mike”, weight: “200 Ibs”}) 
```

假设这个类是完整的，即使它是无序的，这也是可行的，因为我们可以从对象字面量进行调用。

请注意，上面的代码无法正常工作，因为我们还没有讨论 JavaScript 类的下一个方面。

## 私人与公共

类中的数据被组织成私有和公共数据。有些东西是你不希望外人在类内访问的，所以你把它们保持为私有。例如，这可能是一个计算人的身体质量指数的算法。这应该永远保持不变，所以你不想让人们改变它。但是我们确实希望能够改变名称或重量，不是吗？所以数据应该公开。

下面是我们如何在 JavaScript 中将公共数据与私有数据分开。注意，经典类构造中没有关键字。

```
 function User(construct){
//constructor
 this.name = construct.name; 
 this.height = construct.height;
 this.age = construct.age;
 this.weight = construct.weight;
//private
 let bmiMultiple = 703;
 let bmi = ()=>{
 return (this.weight / (this.height * this.height)) * bmiMultiple;
 }
//public
 return {
 name,
 height,
 age,
 weight,
 bmi
 }
} 
```

如果我们试图访问 bmiMultiple 变量，我们将无法这样做。请注意，由“this”关键字前置的变量是自动“可获取”和“可设置”的。这就是构造函数的要点——允许某人通过设置值立即构建一个对象。

**注意**:我在 return 语句中列出变量，没有像纯 ES5 中那样添加冒号和值，这是作弊。ES6 允许这种简写。请随意混合新旧

一旦你实例化了你的类…

```
 let bob = User({ height:72, age: 30, name:”Mike”, weight: 200}) // 72 is in inches and 200 is in pounds 
```

…然后您可以获取信息和设置信息:

```
 bob.name; // Mike
bob.name = “Joe”; // Joe
bob.weight; // 200 
bob.weight = 300;
bob.weight; // 300 Ibs 
```

你应该意识到新的方法(ES6 类)不允许你如此容易地分离私有和公共变量/函数。您必须使用一个不规则的下划线来提醒其他开发人员，或者在变量名或函数名前使用一个标签。

```
 // private property
 this.#weightCounter= 0;// private method (can only be called within the class)
 #bmi() {
 this.#weightCounter++;
 } 
```

## 功能

如果您研究过我的[函数](https://www.howtocodejs.com/javascript-functions/)文章，您会注意到我使用了一个匿名函数，以便“bmi”函数可以无缝地融入 return 语句。您可以使用普通的函数声明，但是您应该知道您选择的类结构会改变您调用函数的方式。最好尽可能坚持使用匿名函数，因为它们更简洁、可读性更强。

我们可以这样调用我们的函数:

```
 bob.bmi(); // 27.12 
```

## 遗产

我们应该简单讨论一下继承。最终，你会发现自己需要开设更多针对特定个人的课程。例如，您可能希望创建一个专门针对举重运动员的课程和另一个专门针对跑步运动员的课程。这些类有不同的特征，但是它们仍然和普通用户类有共同点，比如名字，身高等等。

您可以让 CardioUSer 从 USer 类继承，而不只是重写这些变量。有旧的继承方式和新的继承方式。如果你已经用旧的但仍然有效的方法构建了一个类，你会想要做一个“原型继承”我已经写了一篇关于原型的[文章](https://www.howtocodejs.com/javascript-prototype-chain/)。当您查看那篇文章时，我将给出一个继承的旧形式的例子。

```
//constructor
function User(construct){
 this.name = construct.name;
 this.height = construct.height;
 this.age = construct.age;
 this.weight = construct.weight;
}
//prototype
User.prototype.bmi = ()=>{
 return (this.weight / (this.height * this.height)) * 703;
}

//inheriting constructor
function CardioUser(object){
 User.call(this, object)
}
//prototypical inheritance
CardioUser.prototype = User.prototype;let andy = new CardioUser({name:”Joe”, height:72, age: 30, weight: 200});

andy.bmi();
```

有很多事情需要解决。我将 bmi 函数从类中移除，并将其分配给 User 的原型，然后可以将其传递给 CardioUser。这是用 JavaScript 创建类的最实用的方法。函数本身只作为一个构造函数，而原型包含你想要创建的任何动态函数。

接下来，我通过使用 User 上的 call 函数继承了构造函数。

根据 Mozilla 的说法，“使用‘call()’,你可以写一次方法，然后在另一个对象中继承它，而不必为新对象重写方法。”第一个参数接受“this”关键字和用于赋值的附加参数。

在我们的例子中，我们使用了一个对象，所以我们只需要给 object 赋值，这将应用到我们继承的构造函数中。

之后我把功能从 User 分配到 CardioUSer 和 wala！CardioUser 现在继承了构造函数和函数。

新的做事方式要简单得多:

```
 class User {
 constructor(construct){
 this.name = construct.name;
 this.height = construct.height;
 this.age = construct.age;
 this.weight = construct.weight;
 }bmi(){
 return (this.weight / (this.height * this.height)) * 703;
 }
}class CardioUSer extends User {
 constructor(object){
 super(object)
 }
}let bob = new CardioUSer({name:”Joe”, height:72, age: 30, weight: 200});bob.bmi();
```

你可能仅仅通过阅读代码就知道发生了什么。我将用户类别扩展到了有氧运动。我没有使用 call，而是使用了 super，它只需要接受我想赋给我的继承变量的任何内容。在这种情况下，我们使用一个对象文字。

## Getters 和 Setters

如果那些原型的东西让你害怕，你可能最好使用新的 ES6 类。不可否认，它更干净，而且你实际上可以做其他脚本语言中可用的事情，比如指定 getter 和 setter 方法。正如您之前看到的，getter 和 setter 是隐含的，但是使用 Es6 类我们可以显式地设置它们。

```
 set name (name) { 
this.name = name; 
} 

get name () { 
return this.name; 
}
```

这种类型的编码是很好的实践，尽管它看起来可能是多余的。做这件事的老方法是写…

```
 let setName = (name)=> { 
this.name = name; 
} 

let getName = (name)=> { 
return this.name; 
} 
```

## 结论

我们可以没完没了地谈论课程，但这应该足够了。你知道有基本的构建模块需要去做功能性的应用。但是你可能会问，哪种方法更好呢？旧的还是新的？我发现老方法用起来真的很舒服，只是因为我觉得我没有被 ES6 类设置的参数所束缚。然而，我确实发现 ES6 类是值得一看的，它们来自 Ruby 背景，在那里类是紧凑和可读的。

我认为人们必须认识到，JavaScript 一直是一种功能工具，用于为应用程序添加一些奇特的属性。所以创建大型类结构的想法并不存在。语言设计者只是边走边构建，没有预料到 JavaScript 会被用于后端。

但是现在 JavaScript 已经在越来越多的领域使用，ES6 类似乎是为了给来自其他脚本语言的程序员提供安慰而设计的。碰巧的是，新来者可能会发现新的 ES6 类也更容易理解。相比之下，旧的做事方式似乎有点过时。

最后，你应该选择最适合你的。当您习惯于用 JavaScript 编程时，您可能会发现使用旧方法更快。或者你可能不会。这就是语言的美妙之处。各有所好。

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**

[](https://medium.com/the-open-manuel/learn-code-like-a-chef-not-a-diner-45f38cb758af) [## 像厨师而不是用餐者一样学习代码

### 六年前，我想建立自己的网站。所以，我做了一些研究，发现 HTML/CSS3、JavaScript 和…

medium.com](https://medium.com/the-open-manuel/learn-code-like-a-chef-not-a-diner-45f38cb758af)