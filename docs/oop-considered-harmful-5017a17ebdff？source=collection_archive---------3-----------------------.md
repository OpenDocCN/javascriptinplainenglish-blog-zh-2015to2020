# 面向对象编程死了吗？

> 原文：<https://javascript.plainenglish.io/oop-considered-harmful-5017a17ebdff?source=collection_archive---------3----------------------->

## 重新考虑面向对象编程

![](img/d7414c6c5a84bdaf0387bf468ebdeaa4.png)

## 以下是有史以来最受欢迎的媒体文章之一:

[](https://medium.com/@cscalfani/goodbye-object-oriented-programming-a59cda4c0e53) [## 再见，面向对象编程

### 我已经用面向对象语言编程几十年了。我用的第一个 OO 语言是 C++然后是 Smalltalk…

medium.com](https://medium.com/@cscalfani/goodbye-object-oriented-programming-a59cda4c0e53) 

这篇文章的前提是，面向对象编程承诺良好设计的代码，交付一塌糊涂，应该被[函数式编程](https://medium.com/swlh/functional-programming-vs-object-oriented-programming-48eee6cf6830)所取代。

这种反应将从两个方面进行。

首先，我们来看看文章中的实际论点。

其次，我们将在现实世界中寻找帮助，看看 OOP 是否真的应该以 p 开头(这是一个便便笑话)。

# 香蕉猴子丛林问题

这是作者对 OOP 的第一次批判。以下是他发现这个多彩名字的引文:

> *面向对象语言的问题是，它们随身携带着所有这些隐含的环境。你想要一个香蕉，但你得到的是一只大猩猩拿着香蕉和整个丛林。*

*(* ***乔·阿姆斯特朗*** ，二郎的创造者)

这方面没有太多细节。没有代码。但批评似乎是，如果你想从一个类继承，你实际上必须从该类的整个层次结构中继承。

现在，我认为关于设计使用对象的应用程序有一个有效的*顾虑*，因为你可以在运行时创建交互的对象图*变得很霸道。也就是说，物体可以暗示一个环境的宇宙。*

我认为乔·阿姆斯特朗的话指的就是这个。注意，他说的是*“内隐环境”。我们一会儿会好好看看这是什么意思。*

然而，那根本不是媒体文章的作者所指的。相反，他抱怨在设计类的结构时，由于隐含的与层次结构的耦合，继承对于引用类来说变得很麻烦。

现在，对设计 OOP 代码和层次结构有一个真正的批评:在层次结构的祖先之间真的有隐含的耦合。

但是问题是在整个代码的变化中， ***而不是*** 在消耗对象/类的客户端代码/类中。

后一点是作者在《面向对象》中提出的批评:

> 现在不会编译了。为什么？？哦，我明白了……***这个*** 物体包含 ***这个*** 物体。所以我也需要那个。没问题。

嗯，底线是这不是对 OOP 的有效批评。这是对糟糕的 OOP API 设计的有效批评。

所以，为了让大家明白这一点:不要创建将自己暴露给客户端代码的交互类层次结构。将它们打包，以便暴露一个干净的接口，并尽可能隐藏更多的内容。

完成了。

第一个批评暗示了有效的*关注*，但并不是对 OOP 的真正攻击。

# 钻石问题

第二个批评是“钻石问题”

这个关于 OOP 语言设计的事实长期以来被冠以“[多重继承](https://en.wikipedia.org/wiki/Multiple_inheritance)”的名称。这实际上意味着:很难创建一个从两个具体类派生出来的类。

为什么？

因为你有冲突的公共成员。

这根本不是对 OOP 的批评。这是对逻辑本身的批评。

然而，文章中的抱怨是，有时这正是我们所需要的！(但是，但是……如果我必须对此建模呢？我要我的重用！)

下面是作者提到的一个类似的代码情况:

```
Class WheeledVehicle {
}Class Automobile inherits from WheeledVehicle{
  function roll() {
  }
}Class Scooter inherits from WheeledVehicle {
  function roll() {
  }
}Class PoweredScooter inherits from Automobile, Scooter{
}
```

可以看到多重继承问题:`PoweredScooter`要继承什么`roll()`方法？

作者提供了一个“包含和委托”的解决方案。本质上，PoweredScooter 类“拥有”汽车和小型摩托车，而不是“是”汽车和小型摩托车。

这是一个潜在的解决方案。作者没有说明为什么它是不可接受的。这实际上是 OOP 设计中的一个常见主题:[更喜欢组合而不是继承](https://en.wikipedia.org/wiki/Composition_over_inheritance)。(这个想法是最小化第一部分中提到的层次结构的强耦合的一种方法。)

还有其他解决方案，比如用正在讨论的方法创建一个具体的基类。但是我对这个批评最大的不满是:它抱怨他想用对象来建模他的问题，但是它不够灵活。

因此:扔掉对象建模？即使你想用它？

这就像说我的车只能在路上行驶，不能割草，所以我要扔掉我的车。也许骑着我的割草机到处跑。

如果汽车带来优势，那就利用它们。不要因为它有局限性就湮灭整个事物。

嘘。

# 脆弱的基础类问题

接下来的热门话题是文章中所谓的“脆弱的基础阶级问题”。

TL；DR 总结是这样的:如果一个基类以一种破坏性的方式改变，它将破坏你的后代类。

你猜怎么着:如果你所依赖的代码是一个突破性的改变，那么它将会破坏你的代码。

这就是为什么语义版本化是一个好主意。但这与 OOP 没有任何关系。

在构建目标代码时，这是同样的*关注点*:子类和父类之间有很强的耦合。但这就是依赖 API 的本质。

我想我们可以很快取消这个。

诚然，基类是一个潜在的破坏依赖点，但这是编写代码的现实，应该通过良好的设计和谨慎的版本控制过程来处理。

不抛弃语言特征。

差不多完成了，但是让我们快速浏览一下…

# …封装

看这里。封装是代码组织的一个基本特征，包括函数式编程。

在面向对象的程序中，你可能对它的实现方式有问题，但是攻击封装本身是…poco loco。

# 让我们看看现实世界

现在，文章中还有一些条目，但它们都归结为同一个批评:对象层次结构意味着强耦合和(我会自己加上)不完整的信息隐藏。

我认为我们可以接受这些是好的设计应该避免的问题。

但是可以肯定的是，在现实世界中，存在着超级聪明、专注和资金充足的函数式编程类型，OOP 已经被完全取代了。

对吗？

# 反应

看看这个。[如果可以](https://www.youtube.com/watch?v=2gm29WZpBJc)。

```
class Parent extends React.Component {
 render() {
   return <div class=”parent”>Parent
     <Child></Child>
     </div>;
   }
 }class Child extends React.Component {
   render() {
   return <div class=”child”>child</div>;
 }
}
```

这是我对 React 的介绍(你应该[看看](https://medium.com/@matthewcarltyson/react-in-7-minutes-a4fe81eb13ef))。

上面明显是从`React.Component`往下的 React 组件。但这一定意味着开发人员只是被灌输了 OOP 的僵尸白痴…对吗？

React 使用函数式编程。有意义的地方。

然后在有意义的地方使用对象。

这是一个令人信服的想法。我们应该考虑一下:使用最适合手头任务的工具，*而不是，*使用看起来最酷或最流行的工具。或者甚至:用我们最了解的，因为那是我们用得最多的。

我是说，反应是我最喜欢脸书的一点。我认为这是真正天才的作品。我觉得，在与他们的代码互动之后，他们真的知道他们在做什么，而且真的关心框架(ok，library)如何为开发人员处理。

事实上，我真的很喜欢在这里使用对象。

这使得*很容易*插入他们的 API，并创建一个封装的代码块，具有有用的可变范围。事实上，组件内部状态的概念与对象封装非常相似。

你*可以*定义纯粹的功能组件。

你也可以用螺丝刀钉钉子。

本文的信息:**根据任务**的表现，选择最适合手头工作的工具。

# 有角的

Angular 2+实际上是强 FP(贯穿始终的 RxJs)和强 OOP (IoC 容器、基于类的组件和模块、基于类的装饰器)的巧妙结合。

创建模块:

```
class MyModule {} // Declare a module containing Angular assets
```

创建组件:

```
@Component({…})
class MyComponent() {}
```

创建指令:

```
**@**[**Directive**](https://angular.io/api/core/Directive)**({…})**
class MyDirective() {}
```

诸如此类。

# 某视频剪辑软件

```
@Component
export default MyBtn extends Vue {
  count = 0;

  handleClick() {
    this.count++;
    console.log('clicked', this.count);
  }
}
```

并且:

```
Vue.component('my-btn', {
  data() {
    return {
      text: 'Click me',
    };
  },
  methods: {
    handleClick() {
      console.log('clicked');
    },
  },
  render() {
    return (
      <button class="btn-primary" @click.prevent="handleClick">
        {this.$slots.default}(clicked - {{count}})
      </button>
    );
  },
});
```

# 表达

糟糕，也许这只是一个前端的事情，在后端，在节点。JS，现实世界的 JS 重量级人物都逃课了。

我们来看看 ExpressJS。

```
class Routes {
    constructor(){
        this.foo = 10
    }

    Root = (req, res, next) => {
        res.json({foo: this.foo});
    }
}

var routes = new Routes();
app.get('/', routes.Root);
```

好了，以上其实是 ES7 语法，用对象在 Express 中映射路线，由此 [SO 提问](https://stackoverflow.com/questions/33798933/nodejs-express-routes-as-es6-classes)。那只是为了好玩。

我不一定会那样做。实际上，我认为函数式风格非常适合处理 RESTful 端点。

但是代码中有一个更深层次的观点。它是快速请求对象。

什么？

快速请求对象。是的，请求被建模为一个对象。是的，他们从 [Java Servlets](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletRequest.html) 那里得到了这个想法。是的，因为这是个好主意。是的，不同种类的请求可以被建模为基本请求对象的派生。

但是不管怎样，我的意思是，Express 不会在它的*源代码中使用 OOP。*

比如这里的:

```
Route.prototype._handles_method = function _handles_method(method) {
  if (this.methods._all) { 
    return true; 
  } 
  var name = method.toLowerCase(); 
  if (name === ‘head’ && !this.methods[‘head’]) { 
    name = ‘get’; 
  } 
  return Boolean(this.methods[name]);};
```

或者这里的。

# NodeJS

当然，NodeJS 本身会避免使用有害的对象编程。我的意思是，该死，它的整个前提是一个事件循环。

像这里的、这里的和这里的。

而[在这里](https://github.com/nodejs/node/blob/0af62aae07ccbb3783030367ffe405f45687abb3/src/handle_wrap.h)，用这个整洁的花絮评论道:

```
// — uv_ref, uv_unref counts are managed at this layer to avoid needless
// js/c++ boundary crossing. At the javascript layer that should all be
// taken care of.
```

我无法想象他们究竟为什么会选择使用 C++(即带对象的 C)。

# 所以，总而言之

朋友们，我们正在看一场真正的范式战争。这就是:

> 根据我们对某样东西的感觉和它在给定目的下的表现来做出技术选择

卡波。

很多时候，当我们应该使用最简单的解决方案时，我们试图应用太复杂或太复杂的解决方案，并与对象对称性纠缠在一起。(顺便说一句，这个问题在设计模式中更严重，为可插拔架构设计的模式被应用到小问题中)。

有时候我们不喜欢 OOP，因为它不能按照我们想要的方式处理一个项目(或多个项目)，或者我们从来没有真正理解它，或者其他什么。

FP 也一样。

我们必须把这些都放在一边。看穿炒作进入目的。没有什么是最好的。正如鲍勃·马利所说:

> 生活中的每件事都有它的目的，
> 
> 找到它的原因，
> 
> 在每个季节。

好吧，这是因为我喜欢鲍勃·马利。但是很合适。

我们不想屈服于怨恨*或*群体思维。

不要因为有人告诉你就决定去做 OOP。但是，不要因为有人告诉你而不去做。

同样，这篇文章中的信息:**根据任务的表现为手头的工作选择最好的工具**。

我将留给你们最后一个，令人欢呼的，巨大的，高耸的 OOP 的例子，它只是做它该做的事情，让事情变得震撼。

在这里。

不在 JS 里，抱歉。

[弹簧框架](https://www.javaworld.com/article/3444936/what-is-spring-component-based-development-for-java.html)

马特·泰森是[黑马集团](http://www.darkhorse.tech/)的首席技术官

![](img/ad6655f7332c8a0e3c1f1190a5ec7574.png)

Dark Horse Group, Inc.

有关服务的信息:dev@darkhorse.tech