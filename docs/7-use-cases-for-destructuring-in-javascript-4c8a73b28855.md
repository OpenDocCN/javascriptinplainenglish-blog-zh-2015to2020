# JavaScript 中析构的 7 个用例

> 原文：<https://javascript.plainenglish.io/7-use-cases-for-destructuring-in-javascript-4c8a73b28855?source=collection_archive---------7----------------------->

## JavaScript 中数组和对象析构的介绍及其用例

![](img/47be4e6c4d2910b8043f71f958f0128b.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

析构赋值语法是一个 JavaScript 表达式，它可以将数组中的值或对象中的属性解包到不同的变量中。 [*MDN*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

析构赋值有助于以清晰和简单的方式构建代码片段。它将数组中的单个值或对象中的属性解包，并将它们分配给不同的变量。简单地说，它接受一个对象或数组，并将它们转换成更小的对象、元素或变量。JavaScript 中有两种类型的析构。

*数组析构*
*对象析构*

# **1。数组析构**

数组析构允许从一个数组中取出单个元素，并将它们赋给单个变量。我们用一个例子来讨论。一周中的日子被设置为 ***周*** 数组。

```
const week = [‘monday’, ‘tuesday’, ‘wednesday’, ‘thuresday’, ‘friday’];
```

在没有析构赋值的情况下，你可以像这样访问数组中的元素。

access elements of the array week without array destructuring

现在，让我们用析构赋值来做。在这里，*(你要析构的数组)被放到等号(=)的右边， ***day1，day2，day3，day4，day5*** 变量被赋值给***周*** 数组中的每一个值。*用于析构数组。分配数组 week 的每个值的单行代码**

**access elements of the array week with array destructuring**

# ****2。对象析构****

**对象析构允许从一个对象中取出单独的属性，并将它们分配到更小的对象元素或单独的变量中。 ***{}*** 用于析构对象。这里，我们必须确保属性名与变量名相同。**

**access studentDetails with object destructuring**

# ****3。析构赋值的用例****

## ****3.1 将一个数组与另一个数组组合****

**当我们编码时，通常使用组合两个数组。析构赋值可以用来在任意位置将一个数组与另一个数组组合。在这个例子中，数组 ***平日*** 被添加到数组 ***星期*** 的开头。**

**week array is added weekend values**

## ****3.2 组合两个对象****

**对象析构可以用来组合两个不同的对象。在这个例子中， ***studentDetails*** 和 ***studentGrade*** 对象的属性都被复制到 ***studentResult*** 对象。**

**studentResult contains properties of both studentDetails and studentGrade**

**输出:**

```
**{ name: ‘John’, age: 20, subject: ‘Physics’, grade: ‘A’ }**
```

**请注意，要合并的两个对象中有相同的属性，第二个对象的属性值被覆盖。在本例中，name 属性在两个对象中都可用。因此， ***StudentGrade*** 对象名称值 ***【约翰玛丽】*** 就是***student result***中 ***名称*** 的值。**

**property name is availbe both studentDetais and studentGrade**

**输出:**

```
**{ name: ‘John Mary’, age: 20, subject: ‘Physics’, grade: ‘A’ }**
```

## ****3.3 函数参数析构****

**函数的参数析构有助于防止将不必要的对象属性传递给函数。让我们用一个例子来阐明这个观点。在下面的代码中，***student details***对象被传递给 ***printName()*** 函数。但是函数内部只使用了 ***名*** 和 ***姓*** 。**

**only firstName and lastName is used inside the printName function**

**使用析构，我们可以只将必要的属性作为对象放入函数中。当我们处理大型对象时，这非常有用。在下面的代码中，只有 ***名*** 和 ***姓*** 属性的值作为独立变量输入到 ***printName()*** 函数中。**

**only firstName and lastName properties’ values are input to the printName() function**

**输出:**

```
**Full name is John Mary**
```

## ****3.4 将数组转换成对象****

**在本例中， ***birthDayArr*** 数组被转换成 ***birthDayObj*** 对象。这里， ***生日*** 被解构为 ***年*** ， ***月*，*日*** ，作为***arrayConvertToObj***函数的输入参数。**

**birthDayArr array convert to birthDayObj object**

**输出:**

```
**{ year: 2000, month: 1, day: 1}**
```

## ****3.5 跳过项目****

**在析构过程中，我们可以跳过数组中不想使用的元素。它有助于节省不必要的使用内存的变量，无用的元素在数组中。我们可以跳过这些元素，只需加上**逗号** ( ***，*** )。例如，给定 numebers 数组，我只想要第三和第五个元素。因此，跳过其他人输入逗号。**

**only third and fifth elements are assigned to variables**

**输出:**

```
**3
5**
```

## ****3.6 在对象析构中休息****

**Rest 运算符有助于使单个变量包含一组尚未被析构模式拾取的元素。在该示例中， ***名*** 和 ***姓*** 属性被单独分配，其余属性( ***年龄*** 、**、国家、T15)被分配给单个变量**、、其余、。******

**age and country properties are available in rest variable**

**输出:**

```
**John
Mary
{ age: 20, country: ‘Sri Lanka’ }**
```

## ****3.7 分配新的变量名称并设置默认值****

**在对象析构期间，对象的属性名必须与变量名匹配。然而，我们可以把它分配给一个不同名字的变量。同样，我们可以给变量赋值，而不是**未定义。**在给定示例中， ***名字*** 值被映射到 ***fName*** 并且**“未给定”被指定为默认值而不是**未定义的**。****

**输出:**

```
**John
Mary**
```

## ****参考****

**[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) [## 破坏分配

### 析构赋值语法是一个 JavaScript 表达式，它使得从数组中解包值成为可能，或者…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)**