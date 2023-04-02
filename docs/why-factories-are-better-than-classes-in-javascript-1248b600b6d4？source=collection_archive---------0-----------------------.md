# 为什么 JavaScript 中工厂比类更好

> 原文：<https://javascript.plainenglish.io/why-factories-are-better-than-classes-in-javascript-1248b600b6d4?source=collection_archive---------0----------------------->

## 通过一个例子了解为什么应该使用 JavaScript 工厂而不是类

![](img/34c32b55bb8d663ca9030b840329b4c4.png)

Photo by [**Pixabay**](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/rocket-factory-256297/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

你知道为什么我们经常用工厂而不是类来创建对象吗？

我没有。作为一名被迫在 fullstack 自由职业期间编写 JS 的后端开发人员，我编写工厂是因为“其他代码看起来就是这样”。

这是个糟糕的理由。区别很简单，基本的理解会让你省心。

简单来说，**一个** **工厂就是一个返回对象**的函数，而**一个类就是一个对象**的模板。

但是让我们通过一个`RocketShip`类的例子来理解行为是如何不同的。

# 建造一艘基于职业的火箭船

完成[码笔](https://codepen.io/chris898/pen/WNxgONj)。

让我们写一个实例化火箭船的类。它将能够“飞行”和“着陆”。很刺激吧。

```
class RocketShip {
  constructor(color){
    this.color = color;
  }
  fly(){
    console.log(`The ${this.color} rocketship is flying!`)
  }
  land(){
    console.log(`The ${this.color} rocketship has landed.`)
  }
}
```

然后实例化该类的一个实例。一艘蓝色的火箭船。

```
const r1 = new RocketShip('blue');
```

驾驶它，着陆它，获得它的颜色。

```
r1.fly()
r1.land()
console.log(r1.color)
#=> The blue rocketship is flying!
#=> The blue rocketship has landed.
#=> blue
```

厉害！

## 问题 1:公共属性

牛逼，不牛逼。我们可以创建一个火箭船实例，但是它的颜色可以从外面改变。

```
r1.color = 'sad'
console.log(r1.color)
r1.fly()
#=> sad
#=>The sad rocketship is flying!
```

现在，我们驾驶的不是一艘“蓝色”的火箭飞船，而是一艘“悲伤”的火箭飞船。这可不好。

使用类，可以从对象外部编辑属性。你甚至可以像这样编辑方法。

## 问题 2:“this”关键字

添加 html 和 jquery，这样我们只需点击一个按钮就可以发射火箭。

```
**// html**
<h1>Launch Rocket Ship from Class</h1>
<button>Launch</button> **// css**
h2 {
 font-size: 2rem;
 font-weight: bolder;
 margin-bottom: 1rem;
}button {
 cursor: pointer;
 appearance: none;
 border-radius: 4px;
 font-size: 1.25rem;
 padding: 0.75rem 1rem;
 border: 1px solid navy;
 background-color: dodgerblue;
 color: white;
}**// JS**
$('button').click(r1.fly)
```

![](img/0253357119fb7c41ffe9cf77486c62c9.png)

现在点击按钮

```
#=> The undefined rocketship is flying!
```

哦不。这是怎么回事？“颜色”未定义！

这是因为“This”关键字的作用范围不是对象本身，而是指向 DOM 中的另一个对象。

## 这可以通过以下任一方法解决:

1.  `$('button').click(r1.fly.bind(r1))`
2.  `$('button').click(_ => r1.fly())`

但是它们给我一种轻微的代码味道…

也许我们可以用工厂来解决这个问题。

# 建造一艘基于工厂的火箭船

完成[码笔](https://codepen.io/chris898/pen/KKMxWbw)。

让我们允许实例化同一个对象，但是使用工厂。

```
const RocketShipFactory = (c) => {
  const color = c

  return {
    fly: () => console.log(`The ${color} rocketship has launched.`),
    land: () => console.log(`The ${color} rocketship has landed.`)
  } 
}
```

现在从工厂创建一个火箭船的实例。

```
const r2 = RocketShipFactory('pink')
```

注意这里我们甚至不需要`new`关键字，就像我们从类中实例化一个实例时一样。

让我们调用它的方法。

```
r2.fly()
r2.land()
#=> "The pink rocketship has launched."
#=> "The pink rocketship has landed."
```

我们能从外面改变它的颜色吗？

```
r2.color = 'purple'
r2.fly()
#=> "The pink rocketship has launched."
```

是的。酪它将不再影响使用该颜色的方法。原始颜色现在被硬编码到这些方法返回的内容中。

*感谢*[*Artem Streltsov*](https://medium.com/u/71af155f6204?source=post_page-----1248b600b6d4--------------------------------)*帮忙解释一下。*

现在让我们把它附在我们的按钮上。

```
$('button').click(r2.fly)
$('button').click(r2.land)
```

然后点击按钮。

```
#=> "The pink rocketship has launched."
#=> "The pink rocketship has landed."
```

放松点皮兹。这里一点代码味都没有。

# 结论

你可能已经用过工厂了。现在你知道你为什么要写它们了。

虽然差异可能看起来微不足道，但由于避免了`this`关键字，工厂对 bug 更加健壮。你应该在几乎所有的情况下使用它们。

类的实例化稍微快一些，但是我们说的是几分之一毫秒。通常，在 99.99%的情况下，选择可读性胜过速度。

我错过了什么重要的区别吗？请在评论中告诉我。

*跟进帖:* [*工厂还是比 JavaScript 里的类*](https://medium.com/javascript-in-plain-english/factories-are-still-better-than-classes-in-javascript-47f15071904e) *。*