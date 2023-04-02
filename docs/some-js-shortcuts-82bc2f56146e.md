# 2021 年的 22+高频 JavaScript 代码提示和简写

> 原文：<https://javascript.plainenglish.io/some-js-shortcuts-82bc2f56146e?source=collection_archive---------0----------------------->

## 每个 JS 开发人员都知道的代码片段。

![](img/5bf73eae6a0448078869c61d94d99925.png)

Photo by [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/tool?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 是一种强大的语言，因为你可以编写一个完整的软件，根本不用采用任何其他编程语言。用 JS 写简写有时会帮助你写出更简洁、可读性更强的代码。

> "干净的代码看起来总是像是由关心它的人写的."**罗伯特·c·马丁，** [**干净的代码:敏捷软件工艺手册**](https://www.goodreads.com/work/quotes/3779106)

在本文中，我列出了一些您可能需要的最常用且必须知道的 JavaScript 代码片段。

## 三元运算符

```
let someThingTrue = true
if(someThingTrue){
    handleTrue()
}else{
    handleFalse()
}****** Short Hand version below ******let someThingTrue = true
someThingTrue ?  handleTrue() : handleFalse()
```

## 强空/未定义检查

```
function calculate(someThingImportant){ let result = someThingImportant ?? 1; 
   return result*10;
   // if undefined/null then default 1
}calculate() // return 10
calculate(null) // return 10
calculate(2) // return 20
```

## **短路评估**

```
const defaultValue = "SomeDefaultValue"
var someValueNotSureOfItsExistance = nullvar expectingSomeValue = someValueNotSureOfItsExistance ||     defaultValueconsole.log(expectingSomeValue) *** Output ** SomeDefaultValue
```

## 默认参数，而不是短路或条件

```
// short circuiting or conditionals Approachfunction createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

代替

```
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...using default arguments
}
```

## **如果存在**

```
var someValue = true
if(someValue){
   console.log(“Its exist")
}
```

## **用于循环**

```
for(let i=0;i<1e2;i++){ // instead of i<100, Cool, right?}
```

## 使用关键点在对象上循环

```
var someValues = [1,2,4]
for (let val in someValues){
  console.log(val)
}var obj = {
  “key1”:”value1",
  “key2”:”value2",
  “key3”:”value3"
}
for(let key in obj){
   console.log(key)
}
```

## **值到对象映射**

```
var x=”x”,y=”y”
var obj = {x,y}
console.log(obj)
```

## Object.entries()

```
const credits = { producer: "HBO", name: "Game Of thrones", rating: 9 };
const arr = Object.entries(credits);
console.log(arr); // Note Introduced in ES8 *** Output ** [
 [ 'producer', 'HBO' ],
 [ 'name', 'Game Of thrones' ],
 [ 'rating', 9 ] 
]
```

## Object.values()

```
const credits = { producer: "HBO", name: "Game Of thrones", rating: 9 };
const arr = Object.values(credits);
console.log(arr); // Note Introduced in ES8*** Output **[ 'HBO', 'Game Of thrones', 9 ]
```

## **模板文字**

```
var name = "John",age = 20
var someStringConcatenateSomeVariable = `My Name is ${name} and my age is ${age}`
console.log(someStringConcatenateSomeVariable)
```

## 销毁作业

```
import { observable, action, runInAction } from 'mobx';
```

## **多行字符串**

```
var multiLineString = `some string\n
with multi-line of\n
characters\n`
console.log(multiLineString)
```

## **传播算子**

```
const odd = [1, 3, 5 ];
const nums = [2 ,4 , 6, ...odd];
console.log(nums); // [ 2, 4, 6, 1, 3, 5 ]
```

## **Array.find 速记**

```
const pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]
pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
console.log(pet); // { type: 'Dog', name: 'Tommy' } 
```

## 多重条件

```
const conditions = ["Condition 2","Condition String2"];someFunction(str){
  if(str.includes("someValue1") || str.includes("someValue2")){
    return true
  }else{
    return false
  }
}
```

同样可以通过下面的

```
someFunction(str){
   const conditions = ["someValue1","someValue2"];
   return conditions.some(condition=>str.includes(condition));
}
```

## 默认参数值

这是通常的实现方式

```
function area(h,w){
   if(!h){
     h=1;
   } if(!w){
    w=1;
  }
  return h*w;
}
```

但是你可以用这个来代替。

```
function area(h=1,w=1){
  return h*w;
}
```

## 箭头功能短指针

```
var sayHello = (name)=>{
   return "Hello " + name
}
console.log(sayHello("XOR"))
```

代替

```
var sayHello = name=>"Hello " + name
console.log(sayHello("XOR"))
// OR Similar
["A","B","C"].forEach(item=>console.log(item)) // A B C
```

## 隐性回报

```
var someFuncThatReturnSomeValue = (value)=>{
  return value + value
}
console.log(someFuncThatReturnSomeValue(“John”))
```

代替

```
var someFuncThatReturnSomeValue = (value)=>(
  value + value
)
console.log(someFuncThatReturnSomeValue(“John”))
```

## 必须有参数值

```
function mustHavePatamMethod(param) {
  if(param === undefined) {
   throw new Error(‘Hey You must Put some param!’);
  }
  return param;
}
```

相反，你可以这样重写

```
mustHaveCheck = () => {
  throw new Error(‘Missing parameter!’);
}
methodShoudHaveParam = (param = mustHaveCheck()) => {
  return param;
}
```

## charAt()速记

```
“SampleString”.charAt(0); //Returns S
// OR in Short
“SampleString”[0]
```

## 条件函数调用

```
function fn1(){
  console.log(“I am Function 1”);
}function fn2(){
  console.log(“I am Function 2”);
}/*Long Version*/var checkValue = 3;
if(checkValue === 3){
  fn1();
}else{
  fn2();
}
```

现在简短的版本

```
(checkValue ===3 ? fn1:fn2)(); // Short Version
```

## 数学。地板短手

```
var val = "123"
console.log(Math.floor(val)) // Long version
console.log(~~val) // Short Version
```

## Math.pow 短手

```
Math.pow(2,3); // 8
// Or in short
2**3 // 8
```

## 将字符串转换为数字

```
const num1 = parseInt(“100”);
// In Short
console.log(+"100")
console.log(+"100.2")
```

## &&运算符作为条件

```
var value = 1;
if(value ===1)
 console.log(“Value is one”) //OR In short value && console.log(“Value is one”)
```

## toString 短指针

```
var someNumber = 123
console.log(someNumber.toString()) //return "123"
            // Or in SHORT CUT
console.log(`${someNumber}`) //return "123"
```

## 用动态键值对替换开关

```
const UserRole = {
  ADMIN: "Admin",
  GENERAL_USER: "GeneralUser",
  SUPER_ADMIN: "SuperAdmin",
};getRoute(userRole){ const appRoute = {

    [UserRole.ADMIN]: "/admin",
    [UserRole.GENERAL_USER]: "/user", 
    [UserRole.SUPER_ADMIN]: "/superadmin" };
  return appRoute[userRole] ?? "";}getRoute("Admin") // return "/admin"
getRoute("Anything") // return "" or default case// No more switch/if-else here.
```

## 函数参数太多

考虑下面这个函数。这个函数有四个参数。

```
function myFunction(employeeName,jobTitle,yrExp,majorExp){
}
// you can call it via myFunction("John","Project Manager",12,"Project Management"); //    ***** PROBLEMS ARE *****// Violation of ‘clean code’ principle
// Parameter sequencing is important
// Unused Params warning if not used 
// Testing need to consider a lot of edge cases.
```

相反**创建一个更高级的对象作为参数**

```
const mockTechPeople = {
   employeeName:"John",
   jobTitle:"Project Manager",
   yrExp:12,
   majorExp:"Project Management"
}
```

如下重写该函数

```
function myFunction({employeeName,jobTitle,yrExp,majorExp}){

     // ES2015/ES6 **destructuring** syntax is in action
     // map your desired value to variable you need.}
```

现在打电话很简单

```
myFunction(mockTechPeople); // Only One argument
```

## 可选的链接运算符(即将推出🍻)

ECMAScript 有了一个新的[提案，知道这个很好](https://claudepache.github.io/es-optional-chaining/)

```
var someUser = { name: 'Jack' }
var zip = someUser**?.**address**?.**zip // Optional Chaining like Swift !
```

瞧，`zip`就是`undefined`，不会抛出错误。

语法也支持函数和构造函数调用

```
var address = getAddressByZip**.?**(12345)
```

如果`getAddressByZip`是一个被调用的函数，否则，表达式被计算为`undefined`。

## 用对象文字查找替换开关

```
var fruit = 'banana';
var drink;switch(fruit) {case 'banana':

  drink = 'banana juice';
  break;case 'papaya':
  drink = 'papaya juice';
  break;default:
  drink = 'Unknown juice!';
}console.log(drink); // 'banana juice'
```

这样更好，可读性更强

```
function getDrink (type) { var drinks = {

  'banana': 'banana juice',
  'papaya': 'papaya juice',
  'default': 'Just water !!' };return 'The drink I chose was ' + (drinks[type] ||     drinks['default']);
}
var drink = getDrink('papaya');
console.log(drink); // The drink I chose was papaya juice
```

厌倦了看一些严肃的东西？让我们来复习一下。

## [32+搞笑代码评论人家居然写了](/javascript-in-plain-english/30-funny-code-comments-that-will-make-you-laugh-1c1b54d4ab00)。

# 感谢阅读。🍻