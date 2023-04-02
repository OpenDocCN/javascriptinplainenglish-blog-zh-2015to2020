# JavaScript 中的 var，let，const 有什么区别？

> 原文：<https://javascript.plainenglish.io/whats-the-difference-between-var-let-and-const-in-javascript-598530f639b8?source=collection_archive---------2----------------------->

## 让我们澄清一下关于这些变量的任何困惑

![](img/fe469b2d60566fb5e37cc634d67c46d0.png)

Photo by [Raj Eiamworakul](https://unsplash.com/@roadtripwithraj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果您正在学习如何用 JavaScript 编码，您可能会遇到教程、博客文章或 YouTube 视频以不同的方式声明变量。当我开始的时候，看到这些有点困惑，不知道它们意味着什么或者它们有什么不同。我想为你弄清楚每一个之间的差异。因此，我们将讨论的主要话题是:

*   范围
*   再分配
*   提升

# 范围🔭

在我的 [*关于范围*](https://medium.com/@willjohnson.io/javascript-scope-basics-17c3c583e7fa) 的博文中，我谈到了 var、let 和 const 具有不同的范围。查看这篇文章，了解更多关于范围的信息。这篇博文的内容如下:

*   **让**和 **const:** 有块作用域，意味着花括号{}内声明的变量只能在块或{}内访问

```
if (true) {let warning = 'My Spider Sense is Tingling'}
console.log(warning);//This will throw an error saying warning is not defined
```

*   **var:** has function scope 意味着在函数内部声明的变量只能在该函数内部访问。如果在函数外部声明，它们也具有全局范围。没有带有 var 的块范围。

```
function whoIsSpiderMan () {
    var spiderMan = 'Miles Morales'; 
}console.log(spiderMan);//This would throw an reference errorfunction whoIsSpiderMan () {
    var spiderMan = 'Miles Morales';
    console.log(spiderMan); 
}whoIsSpiderMan();//Calling this function would log Miles Morales to the console
```

# **重新分配📃**

有时在你的项目中你需要改变变量，而其他时候你不需要。当变量发生意外变化时，它会导致失败，让我们看看 var、let 和 const 是如何处理重分配的。

*   **var:** 这是狂野的西部。你可以你可以重新分配变量到你的心满意足，不会有错误。在一个可能会出问题的大项目中。

```
var spiderMan = "Peter Parker";
var spiderMan = "Miles Morales";
spiderMan = "Miguel OHara";//All these will work
```

*   **let:** 与 var 非常相似，最大的区别是 let 变量*不能被*重新声明，但是它们可以被重新赋值。

```
let spiderMan = "Peter Parker";
spiderMan = "Miles Morales";//This will work because we only reassigned the valuelet spiderMan = "Peter Parker";
let spiderMan = "Peter Porker";//This will throw an error saying 'spiderMan' is already declared 
```

*   **const:** 常量的简称，规则最多。用 const 声明的变量不能重新声明或重新赋值。可以修改对象和数组的值，但不能将它们重新分配给另一个对象或数组。

```
const spiderMan = "Peter Parker";
const spiderMan = "Miles Morales";
spiderMan = "Otto Octavius";//Once 'spiderMan' is assigned to const variable it will throw error if you want to try to re-declare or reassign it
```

**修改常量对象:**

```
const spiderMan = {
  identity: "Peter Parker"
}
spiderMan.identity = "Otto Octavius"
//This will change the value of identity because you can modify const objects
```

**修改常量对象第二部分:**

```
const spiderMan = {
  powers: 'Spider Sense'
}
//This will throw an error saying spiderMan has been declared because it can't be assigned to a different objectspiderMan.powers = "Spider Sense"spiderMan = {
  identity: "Peter Parker"
  powers: "Spider Sense"
}
//This will add new key value pair to the existing spiderMan object, not try to replace the object
```

# 提升🏗

提升接受变量声明并将它们提升到当前范围的顶部。它不接受它们的赋值，而是将它们赋值为未定义的。

*   **var:** 只有 var 声明的变量参与提升。当用 var 创建变量时，它们被赋予一个缺省值 undefined。此外，在给它们赋值之前，它们可以作为未定义的对象被访问。这可能会增加出错的几率。

```
function whoIsSpiderMan(identity){
  console.log(identity);
  var identity = "Peter Parker"
}whoIsSpiderMan();//when the function get called it will return undefined
```

*   **让**和 **const:** 不参与吊装。如果您试图在声明这些变量之前访问它们，您将会收到一个错误，这在调试时会更有帮助。

```
function whoIsSpiderMan(){
  console.log(identity);
  let identity = "Peter Parker"
}whoIsSpiderMan();//Using let or const will throw an error 'Uncaught ReferenceError: Cannot access 'identity' before initialization'. 
```

上面的错误非常清楚地说明了问题所在，而不仅仅是返回 undefined。

# 我应该使用哪一个？

如果你问这个问题，专家们的一般规则是，当你不希望变量改变时，首先使用 **const** ，当你希望它改变时，使用 **let** (就像在 for 循环中)，不要使用 **var。当然，观点各不相同，所以做你的研究，看看什么最适合你。**

感谢您花时间阅读我的博客，希望对您有所帮助！下次再见！

*你也在推特上找我* [@willjohnsonio](https://twitter.com/willjohnsonio)