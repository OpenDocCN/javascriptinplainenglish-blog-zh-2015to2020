# JavaScript 中的循环

> 原文：<https://javascript.plainenglish.io/loops-in-javascript-7598e51f2841?source=collection_archive---------5----------------------->

## 有许多方法可以循环通过代码块

![](img/95489497dac9ce6769f468790acbc552.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

循环一直是大多数编程语言的重要组成部分。它们允许我们通过只写一次代码来执行重复的任务。在 JavaScript 中，有很多方法可以遍历一个代码块。在本文中，我将涵盖您必须自己编写的内容，如 ***表示*** ， ***表示……在*** ， ***表示……的*** ， ***而*** 和 ***做……而*** 循环。我将提供一个关于如何以及何时使用它们的示例代码。

# 为

***为*** 回路是最常见的一种。当您需要一遍又一遍地运行相同的代码时，就会用到它。例如，如果您有一个姓名数组，并希望为每个姓名打印一条欢迎消息，那么您应该使用它。当循环运行不需要任何先决条件时，可以使用这种方式。for 循环语法由三条语句组成。

```
for (***var i = 0***; **i < 10**; ***i++***) {
    //code goes here
}
```

第一个是初始化器，通常在遍历数组时用作索引。第二个条件是循环代码块必须计算为 true 才能执行。最后一条语句非常重要，它通过递增或递减初始值来防止无限循环。它还控制着迭代数组的顺序，可以是正向或反向。

考虑这个数组。

```
var names = ["James",
    "Jacob",
    "Jackson",
    "Jordan",
    "Jared",
    "Jude",
    "Jeremiah",
    "James"
];
```

数组有一个从零开始的索引，循环计数器的初始值在遍历一个数组时必须设置为零，以避免越界而导致错误。如果您只是在一个没有数组的代码块中循环，那么您可以将初始值设置为任何适合您需要的值。要为每个名字打印问候语，可以使用 for 循环。

```
for (var index = 0; index < names.length; index++) {
    console.log("Welcome " + names[index]);
}
```

# 为了…在

循环中的***for…遍历对象的属性。考虑这个学生对象。***

```
var student = {
    firstname: "James",
    lastname: "Gordon",
    age: 19,
    id: "GSE28298ID"
}
```

您不能使用 for 循环来遍历属性，因为它不是一个数组，然而，for…in 循环正是为这种情况而设计的。要循环访问学生对象属性，您的代码应该如下所示。

```
for (var property in student) {
    console.log(property);
}--output--
firstname
lastname
age
id
```

您还可以在同一个循环中获取对象属性值。

```
for (var property in student) {
    console.log("my " + property + " is " + student[property]);
}--output--
my firstname is James
my lastname is Gordon
my age is 19
my id is GSE28298ID
```

# 为了…的

for…of 循环与中的 for…相似。但是它不是遍历一个对象的属性，而是让你遍历任何可迭代的数据结构，比如字符串、映射、数组等等…

例如，如果您想打印您名字的每个字母，您可以使用 for…of，因为字符串是一种可迭代的数据结构。这样做的代码应该是这样的。

```
var myname = "Awesome Sauce";for (var char of myname) {
    console.log(char);
}--output--
A
w
e
s
o
m
e

S
a
u
c
e
```

# 在…期间

只要条件语句为真，while 循环就会运行一段代码。其语法如下:

```
while (condition) {
    // code goes here...
}
```

假设您正在编写一个提示用户输入姓名列表的应用程序。一旦用户从菜单中选择了打印选项，您的应用程序就会打印用户提供的姓名。但是，只要用户一直选择允许他们插入更多名称的选项，您的应用程序就会一直循环遍历代码，并提示用户输入另一个名称。您将使用 while 循环来收集姓名，然后在条件评估为 false 时打印姓名。

```
var listOfNames = [];var menu = "What would you like to do? " +
    "\n1\. Insert new name" +
    "\n2\. Print names";//condition
var command = prompt(menu);//collects name as long as user 
//chooses to insert new namewhile (command == "1") {
    var name = prompt("Enter name:");
    listOfNames.push(name);
    command = prompt(menu);
}//print list of names
alert(listOfNames);
```

您需要添加所有必要的验证以避免收集坏数据，但这是您需要使用 while 循环的情况之一。

# 做…的同时

***do…while*** 循环的行为类似于 *while* 循环，只有一个例外。在检查条件之前，它至少运行一次。因此，如果您需要运行一次代码块，然后根据条件重复它，那么您将需要使用 *do…while* 循环，而不是 *while* 循环。换句话说， ***do…while*** 循环是 ***while*** 循环的变体。

如果您还记得前面的例子，显示和提示用户从菜单中选择一个动作的代码在循环之外，那么我们的条件语句检查以确保用户不想打印姓名，而是插入姓名。同样的代码可以用一个 ***do …while*** 循环编写，如果我们在循环中移动代码来显示菜单并检查用户选择。

```
var listOfNames = [];var menu = "What would you like to do? " +
    "\n1\. Insert new name" +
    "\n2\. Print names";var command = "";do {

    command = prompt(menu);
    if (command == "1") {
        var name = prompt("Enter name:");
        listOfNames.push(name);
    }
} while (command == "1")//print list of names
alert(listOfNames);
```

如果仔细观察，您会发现这段代码片段的行为与前面的示例相同。记住，do…while 循环中的代码至少执行一次。在我们的场景中，我们需要显示菜单并至少提示用户一次，如果用户选择输入更多的名字，我们必须再次提示他们。前面我说过 do…while 循环只是 while 循环的变体。这意味着 while 循环的另一个版本。在这个版本中，我们唯一不同的地方是将一些代码从循环外部移到循环内部。while 循环之前的代码首先运行，然后循环的条件语句在执行代码块之前检查条件。

谢谢你坚持到最后。快乐编码。