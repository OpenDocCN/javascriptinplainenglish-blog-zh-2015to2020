# JavaScript 的 10 个简单而有用的技巧

> 原文：<https://javascript.plainenglish.io/10-simple-and-useful-tricks-with-javascript-f5b6cff806ee?source=collection_archive---------2----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Two monitors with no visible code

本文的内容是:

*   1.用值填充数组
*   2.获取数组唯一值
*   3.替换 JavaScript 字符串中出现的所有单词
*   4.得到一个概率为 X 的随机值
*   5.生成唯一的通用标识符 UUID
*   6.数组项的对象破坏
*   7.创建纯对象
*   8.格式化 JSON 代码
*   9.展平多维数组
*   10.合并对象

所有示例均已在铬≥ 72 中进行了测试

# 1.用值填充数组

规格:ECMAScript 2015(第 6 版，ECMA-262)。

Array.prototype.fill()方法用静态值填充数组中的指定元素。

语法:array.fill(value，start，end)。

```
let array = Array(3).fill('hello');
console.log(array); 
// ["hello", "hello", "hello"]let array = Array(6).fill('hello',2,4);
console.log(array); 
// ["hello", "hello", "hello"][empty × 2, "hello", "hello", empty × 2]
2: "hello"
3: "hello"
length: 6
```

# 2.获取数组唯一值

规格:ECMAScript 2015(第 6 版，ECMA-262)。

您可以使用数组的本机**过滤器**方法，或者通过以下方式使用 **Set 对象** : new Set([iterable])来获取具有唯一值的数组:

```
const numbers = [
    '1’, 
    '2’, 
    '1’, 
    '2’, 
    '3'
]//**With array From + Set**
const uniques = Array.from(new Set(numbers));/With Spread operator + Set
const uniquesWithSpreadOperator = [...new Set(numbers)];//**With filte**r
function only(value, index, self) { 
    return self.indexOf(value) === index
}const uniquesWithFilter = numbers.filter();uniques
// ["1", "2", "3"]uniquesWithSpreadOperator
// ["1", "2", "3"]uniquesWithFilter
// ["1", "2", "3"]
```

# 3.替换 JavaScript 字符串中出现的所有单词

为了替换指定字符串的所有实例，您可以使用
使用以下内容:

使用正则表达式:

```
let str = 'hello world! hello you! Hello Elsy!';str.replace(new RegExp('hello', 'g'), 'hi');//"hi world! hi you! Hello Elsy!"
```

使用拆分()和连接()JavaScript 函数:

```
let str = 'hello world! hello you! Hello Elsy!';str.split('hello').join('hi');"hi world! hi you! Hello Elsy!"
```

# 4.获取概率为“%”的随机值

规格:ECMAScript 1。

以同样的概率随机返回**真**或**假**值:

```
let randomValue = Math.random() >= 0.5;randomValue
//truelet randomValue = Math.random() >= 0.5;randomValue
//false
```

# 5.生成唯一的通用标识符 UUID

这里有一个使用加密 API (ECMAScript 2015 (ES6))的解决方案。

```
function uuid() {
  return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c /   
       4).toString(16)
  );
}console.log(uuid());
//f6e1a686-7fd7-410b-bdaa-ebdfbdd5cc52
```

# 6.数组项的对象破坏

规格:ECMAScript 2015(第 6 版，ECMA-262)。

使用对象析构将数组项分配给单个变量:

```
const csvString = 'A team,Alabama,US,Hannibal Smith,New York,1000';const {3: name, 4: country,0: team} = csvString.split(',');name
//"Hannibal Smith"country
//"New York"team
//"A Team"
```

# 7.创建纯对象

常规对象继承自对象:

```
const regularObject = {};
regularObject.name = "Hannibal Smith";
//"Hannibal Smith"regularObject 
//{name: "Hannibal Smith"}
name: "Hannibal Smith"
**__proto__: Object**
```

使用对象创建纯对象。创建():

规格:ECMAScript 2015(第 6 版，ECMA-262)

Sintax: Object.create(proto[，propertiesObject])。

纯物无属性:x. **__proto__** 且不继承**物**:

```
const myPureObject = Object.create(null);myPureObject.name = "Hannibal Smith";myPureObject
// {name: "Hannibal Smith"}
```

# 8.格式化 JSON 代码

规格:ECMAScript 2015(第 6 版，ECMA-262)。

用 JSON.stringify 将一个对象格式化为 JSON 字符串非常简单

```
const myTeam = {"name":"Hannibal", "lastName":"Smith", "team":"A team"};JSON.stringify(myTeam);// Json:
//"{"name":"Hannibal","lastName":"Smith","team":"A team"}"
```

# 9.展平多维数组

您可以使用 ECMAScript 2015(第 6 版，ECMA-262)扩展运算符来执行此操作。

阵列深度为 1:

```
const multiDimensionalArray = [ ['a', 'b'], ['c', 'd'], [1, 2] ];const flattenedArray = [].concat(...multiDimensionalArray);flattenedArray
// ["a", "b", "c", "d", 1, 2]
```

您可以传递参数来选择展平的深度

```
const multiDimensionalArray2 = [ **[** **[**'a', 'b'**]** **]**, **[** **[**'c', 'd'**]** **]**, [1, 2] ];const flattenedArray2 = multiDimensionalArray2.flat(2);flattenedArray2
// ["a", "b", "c", "d", 1, 2]
```

# 10.合并对象

规格:ECMAScript，2018。

使用对象扩散运算符在 JavaScript 中合并 N 个对象非常简单。

注意:如果**对象**有一个同名的属性，则最后一个**对象**属性会覆盖前面的属性。

使用扩展运算符的示例。

```
let hannibal = {leader: 'Hannibal', eyes:'blues'};let templeton = {faceman: 'Templeton', lastname: 'Peck', eyes:"blues"};let baracus = {strongman: 'B.A', fears: 'fly in a plane', eyes:"brown"};let murdock = {howlingMad: 'H. M.', abilities: 'pilot'};let theATeam = {...hannibal, ...templeton, ...baracus, ... 
murdock};theATeam;//{leader: "Hannibal", eyes: "brown", faceman: "Templeton", //lastname: "Peck", strongman: "B.A", …}leader: "Hannibal"
    eyes: "brown"
    faceman: "Templeton"
    lastname: "Peck"
    strongman: "B.A"
    fears: "fly in a plane"
    howlingMad: "H. M."
    abilities: "pilot"
```

使用 ECMAScript 2015(第 6 版，ECMA-262)对象。

```
let hannibal = {leader: 'Hannibal', eyes:'blues'};let templeton = {faceman: 'Templeton', lastname: 'Peck', eyes:"blues"};let baracus = {strongman: 'B.A', fears: 'fly', eyes:"brown"};let murdock = {howlingMad: 'H. M.', abilities: 'pilot'};let theATeam = {}; 
Object.**assign**(theATeam, hannibal, templeton, baracus, murdock);//{leader: "Hannibal", eyes: "brown", faceman: "Templeton", //lastname: "Peck", strongman: "B.A", ...}leader: "Hannibal"
    eyes: "brown"
    faceman: "Templeton"
    lastname: "Peck"
    strongman: "B.A"
    fears: "fly"
    howlingMad: "H. M."
    abilities: "pilot"
```

## 如果这对您有帮助，请单击下面的鼓掌按钮。谢谢！！