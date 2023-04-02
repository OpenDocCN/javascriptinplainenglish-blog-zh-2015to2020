# 深入研究 JavaScript 数组 sort()方法

> 原文：<https://javascript.plainenglish.io/how-to-sort-arrays-in-javascript-with-sort-method-1da130e559be?source=collection_archive---------6----------------------->

![](img/50a749bfc36f89c64af892e2a14599f6.png)

Photo by UX Indonesia from Unsplash

如果您正在处理数据数组，那么以一种排序的方式保存数据总是更好。JavaScript 的`sort()`方法按照字母或数字顺序对数组中的项目进行排序。

```
const cars = ['BMW', 'Jaguar', 'Audi', 'Tesla', 'Ferrari']; console.log(cars.sort()); 
// ["Audi", "BMW", "Ferrari", "Jaguar", "Tesla"]
```

`sort()`方法根据字母或数字顺序进行默认排序

对于数字，sort()方法将值作为字符串进行排序。如果将数字作为字符串排序，那么它将产生不正确的结果，如“8”大于“55”，因为“8”大于“5”，以及“-52”大于“-206”，因为“5”大于“2”。

```
const numbers = [50, -22, 8, -12, 30];numbers.sort(); console.log(numbers); // [-12, -22, 30, 50, 8]
```

在这里可以看到，“-22”大于“-12”，因为“2”大于“1”，而“8”大于“5”，所以“8”大于“50”，因此都是不正确的结果。我们马上会看到如何解决这个排序问题并提供正确的结果。

有时，我们可能需要根据一些参数或条件对数组进行排序。Array 的`sort()`方法接受一个名为**比较函数**的回调函数，在这里你可以定义你的比较逻辑。

compare 函数接受两个参数，并对数组中的每两项执行 compare 函数。你可以用这两样东西来做比较。比较函数返回三个可能的值，即正、零或负。

*   如果比较函数返回负值，则第一项小于第二项。
*   如果返回零，则两项相等。
*   如果返回正值，第一项大于第二项。

```
const numbers = [50, -22, 8, -12, 30]; numbers.sort((item1, item2) => { 
    return item1 > item2 ? 1 : -1; 
}); console.log(numbers); // [-22, -12, 8, 30, 50]
```

# 包含对象的排序数组

```
const students = [ 
  {name: 'Amitav', id: 32}, 
  {name: 'Ranjit', id: 19}, 
  {name: 'Rohit', id: 43}, 
  {name: 'Suraj', id: 15} 
]; students.sort((obj1, obj2) => { 
    return obj1.id > obj2.id ? 1 : -1; 
}); console.log(students);
```

输出:

```
[ 
  {name: 'Suraj', id: 15}, 
  {name: 'Ranjit', id: 19}, 
  {name: 'Amitav', id: 32}, 
  {name: 'Rohit', id: 43}, 
]
```

现在，您可以根据要排序的对象定义自己的排序方法。

# 有关系的

[6 种向数组中添加项目的方法](https://jscurious.com/how-to-add-items-to-an-array-in-javascript/)
[5 种从数组中移除项目的方法](https://jscurious.com/how-to-remove-items-from-array-in-javascript/)
[6 种在数组中查找元素的方法](https://jscurious.com/how-to-find-elements-in-array-in-javascript/)

*感谢您的宝贵时间* ☺️
更多网络开发博客，请访问[jscurious.com](http://jscurious.com/)