# 面向对象的 JavaScript

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-part-1-1881c3aa694f?source=collection_archive---------2----------------------->

## [第一部分]

![](img/bbbab6fc99628470add15052bb51d73c.png)

JavaScript

# JavaScript 中的面向对象编程是什么？💻

面向对象编程是一种流行的编程风格，它从一开始就扎根于 JavaScript。

如果你已经有了用另一种语言进行面向对象编程的经验，请把你所掌握的知识放在一边，以初学者的心态通读整个模块。

这是因为 JavaScript 中的面向对象编程与其他语言中的面向对象编程非常不同。这比实际情况更困难。

现在，让我们从了解 JavaScript 中的 OOP 是什么开始:

# 从公共对象创建对象:

JavaScript 中的对象有`properties`和`methods`。这些物体可以模拟现实生活中的事物。

这里，我们试图用 JavaScript 构建一个`user`对象。一个`user`有一个`firstName`、`lastName`和一个`age`。我们可以在 JavaScript 中添加这些属性。

用户有一些细节，我们可以通过函数`getDetails()`提取这些细节，我们也可以将这个函数作为`method`写在`user`对象中。

Simple_Object.js

通过使用这种方法，对于每个新用户，我们必须重新编写整个代码，这将使代码又长又乱。

我们可以不使用上述方法，而是创建一个函数(或类似的东西)来使个体。

然后构造一个带有`this`关键字的函数，我们将创建带有`new`关键字的个体。

Constructor_Function.js

**注意:**在面向对象编程中，**构造函数的第一个字母是大写的** ( `User`)，而每个实例的写法都像普通变量(akash，abha 等)..).

这种微小的差异立即向您展示了代码中构造函数和实例之间的区别。

# “这个”是什么？

`this`是 JavaScript 中的一个关键字。当在构造函数中使用时，它指的是用构造函数创建的实例。

`this`是面向对象编程中一个非常重要的概念。所以你需要熟悉它。

不幸的是，JavaScript 中的`this`的值会根据调用函数的方式而变化，这是意料之外的，会造成很多混乱。

因此，在给定的课程中，您将了解更多关于`this`的知识，以帮助您熟悉`this`可能采用的不同值。

[](https://blog.skylinee.me/how-to-master-javascripts-this-keyword-ck1052xz3002civs1hwsuwj9k) [## 如何掌握 JavaScript 的“This”关键字！！

### 这是一个在面向对象编程中经常使用的关键字。在传统的面向对象编程中…

blog . skyline . me](https://blog.skylinee.me/how-to-master-javascripts-this-keyword-ck1052xz3002civs1hwsuwj9k) 

# js 中有 4 层 oops:

*   **第 1 层** —单个对象的对象定向
*   **第 2 层** —对象的原型链
*   **第三层** —构造函数作为实例的工厂，类似于其他语言中的类
*   **第 4 层** —子类化，通过继承现有的构造函数来创建新的构造函数

# 简而言之，这 4 层:

*   第一层:单一对象——JavaScript 的基本 OOP 构建模块——对象是如何独立工作的？
*   **第二层**:原型链:每个对象都有一个零个或多个原型对象的链。原型是 JavaScript 的核心继承机制。
*   第三层:类——JavaScript 的类是对象的工厂。类和它的实例之间的关系是基于原型继承的。
*   第 4 层:子类化——子类和超类之间的关系也是基于原型继承的。

# 第 1 层:单一对象的对象定向

# 什么是对象？

JavaScript 中的对象是一种数据类型，它由`names`或`keys`和`values`的集合组成，用`key` : `value`对表示。`key` : `value`对由可能包含任何数据类型的属性组成——包括字符串、数字和布尔值——以及方法，即包含在对象中的函数。

# 什么是属性、键和值？

JavaScript 中的所有对象都是从字符串到值的映射(字典)。对象中的一个条目被称为`Property`。属性的`key`总是一个文本字符串，属性的`value`可以是任何 JavaScript 值，包括函数。方法是其值是函数的属性。

Property_Keys_Example

## 有三种属性:

*   **属性**:这些是对象中的普通属性，即从字符串键到值的映射。&多包含 1 个调用为`named data properties`，其中包含`methods`
*   **访问器**:特殊的方法，其调用看起来像读取或写入属性。(普通属性是属性值的存储位置，访问器允许我们计算属性值)。
*   **内部属性**:这些不能从 JavaScript 直接访问，但是可能有一些间接的方法访问它们:
    **例子** : `[[Prototype]]`保存一个对象的原型，可以通过`Object.getPrototypeof()`读取。

# JavaScript 中对象的两个角色:

*   对象作为记录
*   对象作为字典

# 作为记录的对象:

*   **记录**:作为记录的对象有固定数量的属性，它们的键在开发时是已知的。它们的值可以有不同的类型。

对象作为记录是通过所谓的对象文字创建的。对象文字是 JavaScript 的一个突出特性:它允许我们直接创建对象而不需要类。

示例:

objectAsRecords

# 点运算符(。):通过固定键访问属性:

*   **获取属性**:点运算符允许我们“获取”一个属性(读取它的值)。

Getting_Properties

*   **调用方法**:点运算符也用于调用方法。

Calling_Properties

*   **设置属性**:对于设置值，我们通过点符号使用赋值运算符(=)。

Setting_Properties

*   **删除属性**:T2 操作符允许我们完全删除一个属性(整个键-值对)。

Delete_Properties_1

`delete`返回`false`，如果该属性是不能删除的自有属性。否则在所有其他情况下返回`true`

Delete_Properties_2

`delete`返回`true`，即使它没有改变任何东西(继承的属性永远不会被删除)

```
delete user.toString // true
console.log(user.toString) // [λ: toString] (Still Exists)
```

## **访问器:**JavaScript 中有两种访问器:

1.  **Getter** (读取)一个属性。
2.  **Setter** (写)一个属性。

*   **Getter:**Getter 是通过在方法定义前加上关键字`get`来创建的:

Getter_Properties

*   **Setter :** 通过在方法定义前加上关键字`set`来创建 Setter:

Setter_Properties

## 扩展成对象文字(…)

在对象文字内部，扩展属性将另一个对象的属性添加到当前对象中:

Spreading_In_Object_literals

## 方法:

我们知道普通函数扮演着几个角色。`Method`就是那种角色。

示例:

Methods_in_Object

*   **Call() :** `call`是一个允许我们调用/调用/激活另一个函数的函数。当我们使用`call`调用一个函数时，我们可以改变`this`的上下文。
    **例如:**

Call_Method_1

`call`用于从另一个函数(或方法)借用方法。

例:假设你有一个数字数组。您想使用`slice`将数字复制到一个新的数组中。使用`slice`的一种方法是通过数字数组调用 slice 方法。

通过`call`的另一种方式是当您使用`call`时，您需要传递数字数组作为`this`上下文。

Call_Method_2

**但为什么我们用这个** `**Array.prototype.slice.call**` **胜过** `**array.slice**` **时其如此复杂呢？**

人们将`call`与数组方法一起使用，因为大多数数组方法(如`slice`、`forEach`、`filter`和`map`)既可以处理数组，也可以处理类似数组的对象。

**例子:**我们也可以对类似数组的对象进行操作。

```
const obj = { foo: [1, 2, 3] } 
const arrayMethods = Array.prototype.reverse.call(obj.foo) console.log(arrayMethods)// [3, 2, 1]
```

*   **应用()**

call 和 apply 方法之间的唯一区别是传递参数的方式。对于`apply`方法，第二个参数是一个`array`参数，而对于`call`方法，参数是单独传递的。

Apply_Method

**注意**:在 ES6 中，不需要`apply`方法，因为我们可以用`spread`操作符展开数组。

*   **Bind()**

`bind`允许您更改`this`上下文。然而，`bind`并没有立即调用函数，而是返回一个带有您提供的参数的函数。

Bind_Method

## 异常属性键:

我们不能用保留字(如 var 和 function)作为变量名，但我们可以用它们作为属性键:

```
var obj = { var: 'a', function: 'b' };
obj.var // 'a'
obj.function // 'b'
```

# 作为词典的对象:

**字典:**作为字典的对象具有数量可变的属性，其关键字在开发时是未知的。它们所有的值都具有相同的类型。

对象最好用作记录。但是在 ES6 之前，JavaScript 没有字典的数据结构(ES6 带来了地图)。因此，对象必须被用作字典。因此，`keys`必须是字符串，但是`values`可以有任意的类型。

**任意固定字符串作为属性键:**

随着我们从作为记录的对象向作为字典的对象前进，将会有一个重要的变化，那就是我们必须能够使用`arbitrary strings & numbers`作为属性键。

Object_As_Dictionaries

# 方括号运算符([ ]):通过计算的键访问属性

括号运算符允许我们通过表达式引用属性。

*   **获取属性**

Getting_Property_With_Bracket_Operator

*   **调用方法**

Calling_Property_With_Bracket_Operator

*   **设置属性**

Setting_Property_With_Bracket_Operator

*   **删除属性**

Delete_Property_With_Bracket_Operator

# 检测属性:

我们将使用`in`操作符来检测属性，因为`in`操作符检查给定的键是否存在于哈希表中，这被认为是确定属性是否存在于对象中的最佳方式。此外，从性能角度来看，它不会延迟。

在某些情况下，我们可能希望只在某个属性是自己的属性时才检查它是否存在。`in`操作员检查自身属性和原型属性。所以我们将需要采取不同的方法`hasOwnProperty()`函数，如果当属性为对象所拥有时返回`true`，对于原型属性返回`false`。

在给定的例子中,`toString`是一个原型属性。

Detecting_Property

# 高级方法:

## 标准方法:

`Object.prototype`定义了几个可以被覆盖的标准方法。两个重要的是:

*   `.toString`:配置如何将对象转换为字符串:主要用于调试目的

toString_Method

*   `.valueOf`:配置如何将对象转换为数字

valueOf_Method

# 枚举:

默认情况下，添加到对象中的所有属性都是可枚举的，这意味着您可以使用一个`for-in`循环迭代它们。可枚举属性的内部`[[Enumerable]]`属性设置为 true。`for-in`循环枚举一个对象的所有可枚举属性。

借助给定的示例，您可以很容易理解这一点:

Enumeration_1

如果您想列出一个对象的所有属性以便以后在程序中使用。ECMAScript 5 引入了`Object.keys ()`方法来检索可枚举属性名的数组。

**注意:**并不是所有的属性都是可枚举的。

事实上，对象的大多数本地方法都将其`[[Enumerable]]`属性设置为。您可以使用`propertyIsEnumerable()`方法检查属性是否可枚举，该方法存在于每个对象中:

Enumeration_2

# 属性和属性描述符

**属性属性:**属性属性是属性的原子构建块。

属性的所有状态，包括数据和元数据，都存储在属性中。它们是属性拥有的字段，就像对象拥有属性一样。

*   属性键通常写在双括号中。
*   属性对于普通属性和访问器很重要。

**属性描述符:**这是一个数据结构，用于以编程方式处理属性。

**常用属性:**

*   **可枚举**:决定是否可以迭代属性。
*   **可配置**:决定属性是否可以更改。

**1。** **数据属性属性: (包含多余的两个)**

*   **Value** :保存属性值(在示例中可以更好的理解)
*   **可写**:表示属性是否可写的布尔值。

Data_Properties

**2。** **取值属性(包含不同的两个属性)**

访问者属性还有两个附加属性。由于没有为访问器属性存储值，这就是为什么不需要`[[value]]`或`[[writable]]`。

*   **获取**:包含获取功能
*   **设定**:包含设定器功能

Accessor_Properties

**3。多重属性**

如果使用`Object.defineProperties()`而不是`Object.defineProperty()`，也可以同时定义一个对象的多个属性

Multiple_Properties

**4。检索方法**

如果你想获取属性，你可以使用`**Object.getOwnPropertyDescriptor().**`很容易做到

*   `Object.getOwnPropertyDescriptor()`:返回一个具有可枚举、可配置、可写值的对象

Reriveal_Method

# 防止对象修改:使对象不可变

正如我们所知，默认情况下，对象是可变的，这意味着我们可以向它添加新的属性，但是为了防止这些添加，我们使用了名为`[[Extensible]]`的属性，它本质上是布尔型的。

如果`[[Extensible]]`是`false`，那么我们可以阻止新的属性被添加到一个对象中。

默认情况下，`[[Extensible]]`被设置为每个对象上的一个`true`

还有三种不同的方法可以防止对象被修改:

1.  **防止扩展**
2.  **密封对象**
3.  **冻结物体**

# 1.防止扩展

在这个方法中，我们可以使用`Object.preventExtensions()`法。在此之后，我们将再也不能添加任何新的属性。

我们也可以通过使用方法`Object.isExtensible()`来检查`[[Extensible]]`的值

**例如:**

Preventing_Extensions

# 2.密封物体

在这个方法中，我们可以使用`Object.seal()`方法和这个函数`seal`对象，这意味着我们既不能添加属性，也不能删除或更改它们的类型。

**例如:**

Sealing_Object

# 3.冷冻物体

在这个方法中我们可以使用`Object.freeze()`方法和这个函数`freezes`对象。这类似于密封函数，但在这个方法中，我们不能写入现有的属性。

此方法中的数据属性是只读的。

**注**:冻结的物体不能解冻

**例如:**

Freezing_Objects