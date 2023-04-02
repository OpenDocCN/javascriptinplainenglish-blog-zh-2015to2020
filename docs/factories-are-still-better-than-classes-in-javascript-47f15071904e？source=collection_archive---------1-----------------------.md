# 工厂仍然比 JavaScript 中的类要好

> 原文：<https://javascript.plainenglish.io/factories-are-still-better-than-classes-in-javascript-47f15071904e?source=collection_archive---------1----------------------->

## 更新了为什么在 JavaScript 中应该使用工厂而不是类的想法

![](img/f6a4ccc1dc749a7c79bd36780db18bf3.png)

Photo by [**Pixabay**](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/adolescent-adult-beauty-blur-459971/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

写完 JS 中的[为什么工厂比类好，我得到了大量的反馈和指正。](https://medium.com/javascript-in-plain-english/why-factories-are-better-than-classes-in-javascript-1248b600b6d4)

让我们借此机会深入研究一下这些建议，以我自己的知识为基础，同时也让任何不熟悉 JavaScript 的人受益。

就当这是对原帖的反驳和延伸吧。

# 类实例的公共属性带来了安全问题(以及如何避免)

[代码笔 1](https://codepen.io/chris898/pen/eYdONbe) 。

我最初提出 JS 类实例容易受到外部属性改变的影响，并认为这是一个缺点。

例如，给定一个类`Car`，我们用`new`创建一个实例，并调用`drive`方法。

```
class Car {
  constructor(maxSpeed){
    this.maxSpeed = maxSpeed;
  }
  drive(){
    console.log(`driving ${this.maxSpeed} mph!`)
  }
  honk(){
    console.log(`honk!!!`)
  }
}const car1 = new Car(120)
car1.drive()
#=> "driving 120 mph!"
```

是`“driving 120 mph!”`。到目前为止一切顺利。

## 公共属性和函数可以在初始化后更改

不仅可以更改对象属性，这些更改还会影响方法的输出。

```
car1.maxSpeed = 300
car1.drive()
#=> "driving 300 mph!"
```

甚至整个方法都可以被覆盖。

```
car1.drive = function(){console.log('parked')}
car1.drive()
#=> "parked"
```

在大多数情况下，从外部编辑属性和方法似乎是不受欢迎的行为。

公平地说，这就是 JavaScript 的本意。但话虽如此，有意的行为并不总是等同于“伟大”或理想的行为。

## 有防止改变属性的策略

[代码笔 2](https://codepen.io/chris898/pen/BaLBoOZ?editors=1112) 。

一个建议是通过使用“getter”函数来防止改变值。

```
class Car {
  constructor(maxSpeed){
    this.defMaxSpeed = maxSpeed;
  }
 **get maxSpeed(){
    return this.defMaxSpeed
  }**

  drive(){
    console.log(`driving ${this.maxSpeed} mph!`)
  }
  honk(){
    console.log(`honk!!!`)
  }
}
```

注意我们已经添加了`get maxSpeed()`。对象现在将用一个`this.defMaxSpeed`属性初始化，而不是用一个`this.maxSpeed`属性。

这可以防止改变的`maxSpeed`影响`drive()`方法。

```
const car1 = new Car(120)
car1.drive()
#=> "driving 120 mph!"car1.maxSpeed = 300
car1.drive()
#=> "driving 120 mph!"
```

也就是说，我们可以通过从外部更新`defMaxSpeed`来解决这个问题。

```
car1.defMaxSpeed = 300
car1.drive()
#=> "driving 300 mph!"
```

我想我们可以想出一个复杂的不可猜测的属性名，但这也会使编码变得不那么有趣。

# 类实例遭受与“这”相关的范围混淆(以及如何避免它)

[代码笔 3](https://codepen.io/chris898/pen/LYRPRaj?editors=1111) 。

对于 JavaScript 新手来说,“this”关键字可能会让人感到困惑。

下面是一个调用`$(‘button’).click(car1.drive)`返回的例子:

```
=> “driving undefined mph!” 
```

而不是预期的`“driving 120 mph!”`。

*原因是* `*this.maxSpeed*` *中的* `*this*` *不再指实例本身。*

```
**//////////
// html //
//////////**<button>Drive</button>**/////////
// css //
/////////**button {
 cursor: pointer;
 appearance: none;
 border-radius: 4px;
 font-size: 1.25rem;
 padding: 0.75rem 1rem;
 border: 1px solid navy;
 background-color: dodgerblue;
 color: white;
}**////////
// js //
////////** class Car {
  constructor(maxSpeed){
    this.maxSpeed = maxSpeed;
  }
  drive(){
    console.log(`driving ${this.maxSpeed} mph!`)
  }
  honk(){
    console.log(`honk!!!`)
  }
}
const car1 = new Car(120)
```

在点击按钮触发`drive()`的三种不同方式中，只有最后一种感觉简单明了。

```
$('button').click(car1.drive)
$('button').click(car1.drive.bind(car1))
$('button').click(_ => car1.drive())#=> "driving undefined mph!"
#=> "driving 120 mph!"
#=> "driving 120 mph!"
```

我最初的想法是使用工厂来完全跳过“this ”,但事实证明有一种简单的方法来解决“this”和类的范围问题。

## **这可以通过箭头函数**解决

[密码笔 4](https://codepen.io/chris898/pen/BaLBjNY?editors=1111) 。

```
class Car {
  constructor(maxSpeed){
    this.maxSpeed = maxSpeed;
  }
  drive = () => {
    console.log(`driving ${this.maxSpeed} mph!`)
  }
}const car1 = new Car(120)
$('button').click(car1.drive)
#=> "driving 120 mph!"
```

注意这个问题是如何通过把`drive(){...}`换成`drive = () => {...}`来解决的。按钮点击现在与`$(‘button’).click(car1.drive)`一起工作。

在我看来，这似乎是一个很好的解决方案。

# 工厂有额外的内存开销

这是无可辩驳的。

类实例占用的内存更少。这是因为每个唯一的方法只存在一次——在类原型上。

相比之下，工厂在每个实例上创建每个方法的副本。当我们看一家工厂时，这一点变得很明显。

```
const Car = (ms) => {
  const maxSpeed = ms

  return {
    drive: () => console.log(`driving ${maxSpeed} mph!`),
  } 
}
```

这里工厂返回一个内部有函数的对象。每个对象内部都有相同的复制函数。

如果实例化 10k 以上的对象，这可能会占用大量内存(我希望有一种简单的方法来测试这一点)。

也就是说，我怀疑在日常开发中，我们需要在浏览器中生成 10k 个相同对象的频率。

# 工厂和类不是苹果之间的比较

我比较了班级和工厂，但这并不完全公平。

它们是两种不同的“东西”，来自两种不同的编码范例。

工厂遵循函数式编程范式，纯粹是为了实例化对象(实例或类)。

类遵循面向对象的编程范式，实际上是对象的模板。

我已经提到的另一个论点是，所有与类有关的“问题”都只是语言的工作方式。

# 结论

作为一个 JavaScript 新手，我确实发现工厂更简单，不容易产生副作用。

虽然这可能是由于缺乏理解和经验，但我一直认为我们应该在简单性方面比性能更胜一筹。为什么编码不应该是初学者友好的？

如果你不同意或可以在这里补充任何东西，我洗耳恭听的反馈！

一如既往，继续编码！