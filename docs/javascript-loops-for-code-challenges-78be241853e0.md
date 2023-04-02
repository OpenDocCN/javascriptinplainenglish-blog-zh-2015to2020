# JavaScript 中遍历数据的五种方法

> 原文：<https://javascript.plainenglish.io/javascript-loops-for-code-challenges-78be241853e0?source=collection_archive---------7----------------------->

![](img/d959280df0bac3e029d1cd2057d69aa7.png)

作为我关于代码挑战中常用方法的系列文章的继续，这篇博客将讨论 JavaScript 中的循环。在练习和处理许多算法问题和代码挑战时，您会发现解决方案通常会包含某种类型的循环。

当我们想要重复做一些事情时，循环允许我们重复代码和逻辑。不用多次硬编码相同的代码，它们有助于保持我们的代码干燥(“不要重复”)，而不是潮湿(“每件事都写两遍”)。有五种不同类型的循环:

1.  **为循环**
2.  **While 循环**
3.  **Do/while 循环**
4.  **For…of loop**
5.  **For…in 循环**

# For 循环

```
for(initialize; condition; step){
//run code block
}
```

For 循环接受 3 个参数:

1.  First —初始化计数器变量。
2.  第二个-说明运行循环的条件。它用于评估计数器变量的条件。如果返回 true，循环将重新开始；如果返回 false，循环将结束。
3.  第三—该语句用于递增或递减计数器变量。

例如，假设我有一个城市数组，我想将短语“我要搬到”添加到数组中的每个城市。我将执行一个 for 循环，而不是为每个城市硬编码这个语句。

```
let cities = ["LA", "Miami", "Toronto", "Boston", "DC"]for (i=0; i<cities.length; i++){
console.log( "I'm moving to " + cities[i])
}I'm moving to LA
I'm moving to Miami
I'm moving to Toronto
I'm moving to Boston
I'm moving to DC
```

*   首先，我们在循环开始之前设置一个变量(i = 0)。在这种情况下，这将指示数组的索引。
*   其次，我们陈述循环运行的条件。只要索引变量小于城市数组的长度(即 4)，循环就会继续执行
*   每次循环运行时，变量都会增加 1 (i++)
*   一旦变量不再小于 4(数组的长度)，条件为假，循环将结束。

# While 循环

While 循环类似于“if”语句，只是它们重复给定的代码块，而不是只运行一次。只要 while 循环的条件为真，它就会继续运行。一旦条件为假，它将停止运行。

```
while(someCondition){
//run code block
}
```

假设我们只想列出数组中的前 3 个城市。

```
let i = 0while(i < 3){
console.log("I'm moving to "+ cities[i])
i++
}I'm moving to LA
I'm moving to Miami
I'm moving to Toronto
```

*   首先，我们在循环开始前设置一个变量(设 I = 0；)
*   然后，我们陈述循环运行的条件。只要索引变量小于 3，循环就会继续。
*   每次循环执行时，变量都会增加 1 (i++)
*   一旦变量不再小于 3，条件为假，循环将结束

# Do/While 循环

do/while 循环在检查条件是否为真之前执行一次代码块。然后，只要条件为真，它就会重复循环。

无论如何，当您想至少执行一次循环**时，使用 do/while 语句。**

```
do {
 *run code block* }
while (*condition*)
```

**示例:**

```
let i = 0
do {
console.log("I'm moving to "+ cities[i]);
i++;
}
while (i < 3)I'm moving to LA
I'm moving to Miami
I'm moving to Toronto
```

# **For…of 循环**

**For…of 循环遍历 iterable 的值，就像字符串或数组一样。它们提供了比常规 for 循环更清晰的语法。**

```
for(variable of iterable){
*code block*
}
```

1.  **变量-对于每次迭代，将下一个属性的值赋给变量。**
2.  **iterable-具有 iterable 属性的对象，如数组或字符串。**

**使用与上面“for 循环”中相同的示例:**

```
let cities = ["LA", "Miami", "Toronto", "Boston", "DC"]for(let city of cities){
console.log("I'm moving to " + city)
}I'm moving to LA
I'm moving to Miami
I'm moving to Toronto
I'm moving to Boston
I'm moving to DC
```

**我们称城市数组中的每个元素为`city`。我们可以直接使用 for…of 循环，而不必像在常规 for 循环中那样执行`(i=0; i<cities.length; i++)`。**

# **对于…在循环中**

**For…in 循环遍历对象的属性(键)。而 for…of 循环遍历数组的值或字符串中的字符。**

```
for(variable in object){
*code block*
}
```

1.  **变量—迭代对象的属性。**
2.  **object *—* 指定将被迭代的对象**

**例如，这里我们有一个城市及其生活成本指数的对象(不，这不是指数，我谷歌了)。**

```
let cities = {
"LA": 77,
"Miami": 74,
"Toronto": 78,
"Boston": 83,
"DC": 87
}for (let city in cities){
console.log("Cost of Living in " + city + " is " + cities[city])
}Cost of Living in LA is 77
Cost of Living in Miami is 74
Cost of Living in Toronto is 78
Cost of Living in Boston is 83
Cost of Living in DC is 87
```

**我把城市中的每一个关键物体称为`city`。为了得到每个`city`键的值(生活成本指数)，我们做`cities[city]`。**

**你有它！JavaScript 循环综合指南。继续杀那些 algos！**

**Couldn’t write about loops without thinking about Froot Loops 😋**