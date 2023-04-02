# 8 个高频 JavaScript 数组函数

> 原文：<https://javascript.plainenglish.io/8-high-frequency-javascript-array-functions-939b4eac300e?source=collection_archive---------6----------------------->

## 每个 JS 开发者都知道的数组操作函数。

![](img/4713b20a607a36cee426a620a552f887.png)

Photo by [Charles Deluvio](https://unsplash.com/@charlesdeluvio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 是一种强大的语言，因为你可以编写一个完整的软件，根本不用采用任何其他编程语言。

如果没有**数组**操作，你认为有可能编写一个软件吗？这里我试着列出一些你可能需要用到的常用数组操作。

> 试着少说话，多编码。直接使用代码示例，没有文档之类的东西

## 包含

```
const array = [1,2,4];
console.log(array.includes(3)) // false as item 3 is not present
```

## 过滤器

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 16, name: "Headphone", price:20, sku:"12345"}]const filteredProduct = product.filter((item)=>{
   return item.price>=100
})
console.log("Filtered Product",filteredProduct)
```

输出

```
filteredProduct = [
  {productId: 12, name: "Monitor", price:100, sku:"12341"}
]
```

## 地图

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 16, name: "Headphone", price:20, sku:"12345"}]const filteredProduct = product.map((item)=>{
 return item.name
})console.log("Filtered Product",filteredProduct)
```

输出

```
['Monitor', 'Mouse', 'Keyboard', 'Headphone'] // Map with Name
```

# 发现

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 16, name: "Headphone", price:20, sku:"12345"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 17, name: "AnotherKeyboard", price:12, sku:"12347"},]const filteredProduct = product.find((item)=>{
  return item.price === 12
})
console.log("Filtered Product",filteredProduct)
```

输出

```
{productId: 15, name: 'Keyboard', price: 12, sku: '12343'}
```

**注意:如您在示例中所见，多次出现被忽略。请记住这一点。**

## 为每一个

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 16, name: "Headphone", price:20, sku:"12345"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 17, name: "AnotherKeyboard", price:12, sku:"12347"},]
product.forEach((item)=>{
 console.log(item.name)
})
```

输出

```
Monitor
Mouse
Headphone
Keyboard
AnotherKeyboard
```

## 一些

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 16, name: "Headphone", price:20, sku:"12345"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 17, name: "AnotherKeyboard", price:12, sku:"12347"},]const hasValue = product.some((item)=>{
  return item.price > 100 
 // return false as no item price > 100
 // return item.price >= 100 // return true as one item price = 100
})console.log(hasValue)
```

# 每个

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 16, name: "Headphone", price:20, sku:"12345"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 17, name: "AnotherKeyboard", price:12, sku:"12347"},]const hasValue = product.every((item)=>{
  return item.price >= 10
})
console.log(hasValue)
```

输出

```
True // All item price is > 10
```

# 减少

```
const product = [{productId: 12, name: "Monitor", price:100, sku:"12341"},
{productId: 14, name: "Mouse", price:10, sku:"12342"},
{productId: 16, name: "Headphone", price:20, sku:"12345"},
{productId: 15, name: "Keyboard", price:12, sku:"12343"},
{productId: 17, name: "AnotherKeyboard", price:12, sku:"12347"},]//totalPrice will accumulate to the totalconst totalCartValue = product.reduce((totalPrice,item)=>{
  return totalPrice+ item.price
},0) // 0 is the position of array you want to reduceconsole.log(totalCartValue)
```

输出

```
154 // Calculate the price of all item price 
```

## 蓄电池

```
const accumulate = (...nums) => nums.reduce((acc, n) => [...acc, n + +acc.slice(-1)],[]);console.log(accumulate(1, 2, 3, 4))
console.log(accumulate(...[1, 2, 3, 4]))
```

输出

```
[1, 3, 6, 10] //for the input of [1, 2, 3, 4]
```

# 感谢阅读！🍻