# JavaScript 中的继承与组合

> 原文：<https://javascript.plainenglish.io/inheritance-is-a-vs-composition-has-a-in-javascript-98fb96dfa0e6?source=collection_archive---------2----------------------->

## 一个是“是”,另一个是“有”——但你知道区别吗？

![](img/868f37a5af8a1c7792a1ac5229fcea33.png)

A man with a laptop

面向对象编程中的组合优先于继承的原则是，类通过它们的组合而不是继承来实现多态行为和代码重用。使用组合模式，您可以根据模型的功能来设计模型，而使用继承模式，您可以根据模型的功能来创建模型。

继承更加严格，对象通常会完成很多他们不使用的功能。你将以著名的“香蕉-大猩猩-丛林”问题结束，在这个问题中，你想要一只香蕉，但你得到的是一只大猩猩拿着香蕉和整个丛林。

在接下来的示例中，我们将使用继承和组合来创建相同的功能，以查看它们的差异，为此，请记住以下注意事项:

1.  魔术师会不会游泳。在这种情况下，我们希望哈利不会游泳，但丽芙会。
2.  我们希望哈利能够变魔术，但是我们不希望丽芙变魔术。

# 遗产

各种对象的典型属性用于形成彼此之间的关系。抽象和公共属性存储在超类中，超类可用于更具体的子类。

在这个例子中，使用类，丽芙可以变戏法，而哈利可以游泳，但是因为丽芙和哈利是魔术师，他们继承自人类，你不能避免丽芙变戏法而哈利可以游泳。

```
class Person  {
    eat() {
        console.log('I am eating');
    }
    breathe(){
        console.log('I am breathing');
    }    
    swim(){
        console.log('I am swimming');
    } 
}class Magician  extends Person{
    trick() {
        console.log('I am doing a trick');
    }
}let liv= new Magician();
let harry = new Magician();//Liv can:
liv.eat();
liv.breathe();
liv.swim();
liv.trick();
//I am eating
//I am breathing
//I am swimming
//I am doing a trick//Harry can:
harry.eat();
harry.breathe();
harry.swim();
harry.trick();
//I am eating
//I am breathing
//I am swimming
//I am doing a trick
```

# 作文

不同的抽象提供了特定的功能，这些功能需要与其他对象相结合来表示更大的抽象。您可以从其他较小的对象创建一个对象。

使用 Composition，您可以通过使用 ES6 Object.assign()来决定向每个对象添加什么功能。

```
const eat = function () {
    return {
        eat: () => { console.log('I am eating'); }
    }
}const breathe = function () {
    return {
        breathe: () => { console.log('I am breathing'); }
    }
}const swim = function () {
    return {
        swim: () => { console.log('I am swimming'); }
    }
}const trick = function () {
    return {
        trick: () => { console.log('I am doing a trick'); }
    }
}const superMagician = ()=> {
 return Object.assign(
     {},
     eat(),
     breathe(),
     trick()
   );
}const noviceMagician = () => {
 return Object.assign(
     {},
     eat(),
     breathe(),
     swim()
   );
}
```

现在，哈利不会游泳，丽芙不会变戏法。

```
let harry = superMagician();
harry.eat();
harry.breathe();
harry.trick();
//I am eating
//I am breathing
//I am doing a tricklet liv = noviceMagician();
harry.eat();
harry.breathe();
harry.swim();
//I am eating
//I am breathing
//I am swimming
```

# 结论

在某些情况下，很难在两者之间做出选择。重要的是要记住，没有什么是银弹。继承的问题是你必须预测未来，你必须在项目的早期构建你的对象的结构，这是一件复杂的事情，因为软件产品的需求是变化的。使用组合模式，代码更容易更改，耦合性更低，因为它更灵活，所以如果可能的话，更喜欢组合而不是继承。

总而言之:

*   当关系是“A 具有 X 能力”时，使用组合
*   当关系为“A 属于 X 类型”时，使用继承

我希望你喜欢这篇文章。谢谢你阅读它。