# 用 JavaScript 创建数组的六种方法。

> 原文：<https://javascript.plainenglish.io/six-ways-to-create-an-array-in-javascript-ac4aa115b926?source=collection_archive---------3----------------------->

## 了解创建 JavaScript 数组的不同方法。

![](img/99fd2d210b9b9534c6f7935d00c69b5d.png)

Image by [Clem Onojeghuo](https://unsplash.com/@clemono2?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 1 .使用赋值运算符

```
var array = [];var array2 = [1,2,3,4,5];
```

# 2.使用新运算符

`new Array(length)`；

*   创建长度设置为数字的数组。
*   `length > 0`否则会抛出错误

```
var arr = new Array(2);arr; [empty × 2]arr.length; // 2arr[0]; undefined// when we setarr[3] = 10; //It will increase the array size[empty × 3, 10]
```

# 3.使用 Array.from

```
var arr = Array.from({length : 2});arr; // [undefined, undefined]arr[0] = 1; arr[1] = 2;var arrCopy = Array.from(**arr**);arrCopy; // [1,2]
```

# 4.Usign 扩展运算符

```
var arr =  [1,2,3,4,5]var array2 = [ ...arr ]array2; // [1,2,3,4,5]
```

# 5.使用数组

创建一个新数组，将参数作为项。数组的长度被设置为参数的数量。

```
var array = Array(1,2,3,4,5);array; // [1,2,3,4,5]
```

如果我们传递单个数字参数，那么`Array`构造函数将创建一个长度与参数相等的新数组

```
var array= Array(5);array; //[empty x 5]
```

如果我们传递字符串

```
var array= new Array("5");array; ["5"]
```

要创建一个单个数元素的数组，我们可以使用`Array.of`

# 6.使用`Array.of`

它类似于数组构造函数。但是

*   。`**Array.of(5)**` → `[5]`
*   `**Array(5)**` → `[empty x 5]`

```
var array = Array.of(5);array; / [5]var array2 = Array.of(1,2,3,4,5,6, "string");array2; // [1, 2, 3, 4, 5, 6, "string"]
```

关注我 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

[**给我买杯咖啡**](https://www.buymeacoffee.com/Jagathish) **。**

![](img/4801d26feab82d129fa9b1b840f89569.png)

[Buy me a coffee](https://www.buymeacoffee.com/Jagathish)

其他文章

[](https://medium.com/better-programming/different-ways-to-duplicate-objects-in-javascript-c199be34ecb7) [## 在 JavaScript 中复制对象的不同方法

### 原来复制物体有很多不同的方法

medium.com](https://medium.com/better-programming/different-ways-to-duplicate-objects-in-javascript-c199be34ecb7)