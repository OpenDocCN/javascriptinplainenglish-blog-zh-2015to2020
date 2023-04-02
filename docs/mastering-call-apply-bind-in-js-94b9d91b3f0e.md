# 掌握 JavaScript 中的调用/应用/绑定

> 原文：<https://javascript.plainenglish.io/mastering-call-apply-bind-in-js-94b9d91b3f0e?source=collection_archive---------8----------------------->

## 真正理解三种被误解的 JS 方法的实用指南

JavaScript 的一个最重要的方面是许多开发人员都不确定的，在 JavaScript 开发人员进入更高级的概念之前，需要明确这一点。

这个主题在某种程度上是必不可少的，对于 JavaScript 开发人员来说，理解如何调用函数、“this”关键字和上下文范例、你正在玩的区域几乎是必须的。在讨论这些问题时，我决定提供尽可能多的例子，因为文献缺少一些方面和用例。

![](img/dffbdcdfdc744bb37bc778c4851f9d67.png)

Photo by [Irvan Smith](https://unsplash.com/@mr_vero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# “这个”是什么？

在我们进入主要问题之前，最好先谈一谈“这个”关键词。“This”关键字指的是上下文，它取决于在哪里操作。

当“this”单独使用或在函数中使用时，它自动引用全局对象，即函数的所有者。但是当模式为“严格”时，它是未定义的。

```
console.log(this);
*// logs global Object* function test() {
    console.log(this)
}test(); 
*// logs global Object*
```

如果在对象文本中操作，方法和实际上下文的所有者就是对象本身。

```
var obj = {
    test: function() {
        console.log(this)
    }
}

obj.test();
*// logs obj*
```

# 函数调用

在 JavaScript 中有多种执行函数的方法。定义了函数之后，最好的方法就是在任何时候用它的名字调用它。

```
function test() {
    console.log(this)
}

*// invoke the function as a function* 
test();
*// logs global object* *// or use a named function* 
var test = function() {
    console.log("test")
}

test();
```

调用函数的另一种方法是在对象文字中定义函数，从而将函数表示为对象方法。

```
*// object literal notation
var obj = {
    a: 1,
    b: 2,
    test: function() {
        console.log(this)
    }
}

// invoke the function as a method
obj.test();
// logs obj {a: 1, b: 2, test: ƒ}*
```

当您想使用构造函数逻辑和“new”操作符来调用一个函数时，您实际上是在创建一个新的对象，而不是创建一个新的函数，因为函数是 JavaScript 中的对象。

```
*// invoked with a function constructor*
function test() {
}

*// use new operator to create a new object*
var obj = new test();
```

还有另一种通过 DOM 事件处理程序调用函数的方法。当您向 DOM 元素注册事件侦听器时，从现在起上下文就是元素本身。

```
var button = document.createElement('button');
button.addEventListener(function(){
    console.log(this)
});
*// logs button*
```

如您所见，上下文隐式绑定在所有者上，在某种程度上，它主要与调用者相关。换句话说，“这个”的价值是由其语境决定的。但是如果您想使用另一个指定的上下文呢？您可能希望使用与另一个上下文相关的函数或方法。当你想改变上下文时该怎么做？但是首先，我们为什么要改变背景呢？

# 致电/申请/绑定的动机

有时我们想使用属于另一个对象或上下文的方法或函数。为了避免重复或耦合，我们可能想要调用或使用另一个没有复制的方法。

让我们用一个例子来讨论这个问题:

```
function test(param) {
    *console*.log(this.name + "-" + param)
}
```

该函数接受参数“param ”,并记录“上下文”的“名称”属性以及附加的“param”值。不管内部逻辑如何，想象一下，这个函数是基于某种通用目的编写的，需要被其他对象或上下文使用。如果像下面这样经常使用这个函数，

```
test("test");

*// logs "-test"*
```

输出会很奇怪。而是根据上面提供的信息进行思考；由于函数是在全局上下文中定义的(此时属于窗口)，因此 this.name 将引用 window.name，此时 window . name 为空字符串。

另一方面，认为这个效用函数将被另一个上下文使用，比如一个对象。该对象还有一个名称属性:

```
var human = {"name":"javascript", "surname": "developer"}
```

但问题是如何将这个函数和对象相互关联，以便函数内部的内部逻辑绑定到显式提供的对象。

这就是 **Function.prototoype.call** 和 **Function.prototype.apply** 方法出现的地方。这些方法调用一个提供了“this”值和某种意义上的参数的函数。主要区别在于提供参数的方式。Call()接受列表形式的参数(用逗号分隔), Apply()接受参数数组。

```
func.call([thisArg[, arg1, arg2, ...argN]])
*// an optional thisArg for the bound context
// an optional list of arguments followed*

func.apply(thisArg, [ argsArray])
*// thisArg for the bound context
// an optional array-like object as argsArray*
```

这些方法允许为要展示的函数/方法定义“this”的新值。通过这些函数，我们只需编写一次方法，就可以提供不同的对象，而无需重写方法。

```
var myMethod = function () {
    console.log(this.location)
}var place1 = {location: 'Istanbul'}
var place2 = {location: 'Izmir'} myMethod()
*// logs window.location* myMethod.call()
*// logs window.location since no this value is bound* myMethod.call(place1)
*// logs Istanbul* myMethod.apply(place2)
*// logs Izmir*
```

调用和应用在很多情况下都得到了最好的实践。

## 链接构造函数

```
function Animal(name, type) {
    this.name = name
    this.type = type
} function Pet(name, type, skill) {
    *//this.name = name*
    *//this.type = type*

    *// instead of replicating the same properties*
    Animal.call(this, name, type)
    *// or Animal.apply(this, [name, type])*
    this.skill = skill
} var animal = new Animal('lion', 'wild')
*// {name: "lion", type: "wild"}* var bird = new Pet('parrot', 'domestic', 'speak')
*// {name: "parrot", type: "domestic", skill: "speak"}*
```

## 调用内置函数

```
let values = [1,2,3,4,5] Object.prototype.toString.call(values)
*// "[object Array]"
// a good method to find the type of a variable !* Math.max.apply(null, values)
*// 5* Array.prototype.splice.call(values,0,1)
*// returns [1]
// and values array becomes -> [2, 3, 4, 5]*
```

## 调用匿名函数

```
(function(){
    console.log(this.name)
})()
*// self invoking function with no parameters or context supplied
// logs window.name in browser*  (function(){
    console.log(this.name)
}).call({name:'test'})
*// logs "test"
// calling an anonymous function with a bound "this"*  (function(param){
    console.log(this.name + param)
}).apply({name: 'test'},[3])
*// logs test3
// calling an anonymous function with bound object and a param*
```

一个简单的工作室:

```
const myMethod = function (param = "district") {
    console.log(`${this.location} ${param}`)
}
let place1 = {
    location: 'Istanbul',
    myMethod: myMethod
}
let place2 = {
    location: 'Izmir',
    myMethod: myMethod
}

myMethod.apply(place1, ["Bayrampasa"])
*// Istanbul Bayrampasa*

myMethod.call(place1)
*// Istanbul district*

place2.myMethod()
*// Izmir district*

place1.myMethod.call(place2, "Gaziemir")
*// Izmir Gaziemir*
```

至于 **Function.prototype.bind** ，它创建了一个新函数，其“this”与所提供的对象以及所提供的参数列表绑定在一起。一旦使用了带有绑定属性的“bind ”,就有了一个新的函数可以调用。

```
newFunc = func.bind(thisArg[, arg1[, arg2[, ...argN]]])
```

请注意，参数是按列表顺序排列的，就像提供给“call”函数的参数一样。

```
const myMethod = function (param = "district") {
    console.log(`${this.location} ${param}`)
}
let place1 = {
    location: 'Istanbul'
}
let place2 = {
    location: 'Izmir'
}

*// create a new function*
let yourMethod = myMethod.bind(place1)

yourMethod()              // Istanbul district
yourMethod('Bayrampasa')  // Istanbul Bayrampasa

let theirMethod = myMethod.bind(place2,'Gaziemir')
theirMethod()             // Izmir Gaziemir
theirMethod('Buca')       // Izmir Gaziemir
theirMethod.call(place1)  // Izmir Gaziemir
// ! note that once the corresponding values are bound,
// Remaining arguments are ignored.
```

绑定函数的一些用法如下:

## 绑定函数创建

绑定函数主要用于生成绑定函数，将函数打包成较短的格式，以备将来使用。同样，函数的快捷方式也可以用这种方法产生。

另一方面，当需要使用时，可以通过产生较短的语法来实现变量、对象文字或方法的缓存。有一个在许多开发人员中广泛传播的简单错误，即当稍后调用时，应该使用方法的原始“this”。

这个问题导致“This”丢失，并可能导致不寻常的后果。解决方案是在绑定函数时提供原始上下文。

```
const circle = {
    value: Math.PI,
    calculate: function(r) {
        return 2*this.value*r
    }
}

let perimeter = circle.calculate(2)
*// 12.56*

const perimeterFinder = circle.calculate
perimeterFinder(2)
*// logs NaN, guess what we lost THIS !!!
// it returns 2*undefined*2 -> NaN*

const truePerimeterFinder = circle.calculate.bind(circle)
truePerimeterFinder(2)
*// logs 12.56*

*// if you are to use 3 as PI ?*
const newPerimeterFinder = circle.calculate.bind({value:3})
newPerimeterFinder(2)
*// logs 12*
```

## 具有初始设置参数的绑定函数

当函数被打包时，一些参数或上下文可以容易地被绑定或设置，覆盖任何可能的参数。

```
const calculations = {
    accumulatorForAdd: 0,
    accumulatorForMult: 1,
    add: function(a, b) {
        return this.accumulatorForAdd + a + b
    },
    multiply: function(a, b) {
        return this.accumulatorForMult * a * b
    }
}

calculations.add(1,2)  *// returns 3*

let calc1 = calculations.add.bind(calculations)
calc1(2,3)   *// returns 5*

let calc2 = calculations.add.bind(calculations, 1)
*// the first parameter (a) is occupied and set to 1.
// if extra arguments are supplied, will be evaluated as followers*
calc2(4,9)  *// returns 5, second parameter (b) will be 4.*

let calc3 = calculations.multiply.bind({accumulatorForMult: 2})
calc3(3,4)  *// returns 2*3*4 = 24 since accumulatorForMult is bound*

let calc4 = calculations.multiply.bind(calculations, 3)
calc4(5,6)  *// returns 1*3*5 = 15 where second parameter is ignored*
```

对于 JS 开发人员来说，跟上框架和库开发中取得的毁灭性进步每天都变得越来越具有挑战性。但是，如果您深入研究这些结构的实际实现，您会发现熟悉的 JavaScript 方法正在承担这一负担。调用/应用/绑定三部曲显然是这些代码段背后最重要的负担之一。

# 用简单英语写的便条

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**