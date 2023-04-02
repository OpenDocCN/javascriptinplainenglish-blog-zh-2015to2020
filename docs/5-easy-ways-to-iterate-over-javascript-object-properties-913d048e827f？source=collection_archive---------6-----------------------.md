# 迭代 JavaScript 对象属性的 5 种简单方法

> 原文：<https://javascript.plainenglish.io/5-easy-ways-to-iterate-over-javascript-object-properties-913d048e827f?source=collection_archive---------6----------------------->

## 每种方式都有自己的优势

![](img/c037dcd988acb9b348eb25cadcf8648a.png)

Photo by [Tine Ivanič](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你知道多少种迭代对象属性的方法？

我知道五个。

在本文中，我将带您了解每一个问题。继续读。

# 1.对象.值

**Object.values** 返回对象属性值列表:

```
[value1, value2, …, value3]
```

当你不在乎按键是什么的时候就用这个。

例如:

```
let job = {
  name: ‘Programmer’,
  averageSalary: 60000
};Object.values(job).forEach(value => {
  // Do something
});
```

**Object.values(job)** 的结果是:

```
[‘Programmer’, 60000]
```

然后使用该数组值来满足您的需求。

# 2.对象.条目

**Object.entries** 返回对象属性键和值对的列表:

```
[[key1, value1], [key2, value2], …, [keyN, valueN]]
```

如您所见，除了值之外，还返回了键。所以，如果你想用钥匙做点什么，就用这个。

让我们看看如何操作这个列表:

```
let book = {
  title: ‘Learn JavaScript in 30 minutes’,
  price: 14.3,
  genre: ‘Technology’
};Object.entries(book).forEach(pair => {
  let key = pair[0];
  let value = pair[1]; // Do something with key and value
});
```

**上例中的 Object.entries(book)** 将返回:

```
[[‘title’, ‘Learn JavaScript in 30 minutes’], [‘price’, 14.3], [‘genre’, ‘Technology’]]
```

# 3.object . getownpropertymanames

**object . getownpropertymanames**返回属性列表:

```
[key1, key2, …, key3]
```

例如:

```
let phone = {
  name: ‘Galaxy Note10’,
  price: 500.0
};Object.getOwnPropertyNames(phone).forEach(key => {
  let value = phone[key]; // Do something
});
```

**object . getownpropertynames(phone)**的结果将是:

```
[‘name’, ‘price’]
```

# 4.对象.键

**Object.keys** 与**object . getownpropertymanames**类似，返回一个对象键列表:

```
[key1, key2, …, key3]
```

例如:

```
let person = {
  name: ‘Amy’,
  age: 28,
  job: ‘Entrepreneur’
};Object.keys(person).forEach(key => {
  let value = person[key]; // Do something
});
```

**Object.keys(person)** 将返回:

```
[‘name’, ‘age’, ‘job’]
```

# 5.用于循环中

您可以在中使用 **for 来迭代对象属性。**

```
let course = {
  name: ‘Learn Python through examples’,
  level: ‘Beginner’
};for (let key in course) {
  let value = course[key]; // Do something
}
```

注意，这个循环包括继承的属性。例如，我们将创建另一个从上面的对象**课程**继承而来的课程对象。

```
let discountCourse = {
  rate: 25
};Object.setPrototypeOf(discountCourse, course);for (let key in discountCourse) {
  console.log(key);
}
```

上面的日志将记录**折扣课程**的属性，包括来自**课程**的属性:

```
ratenamelevel
```

与 **Object.keys** 不同，因为 **Object.keys** 只包含可枚举的属性键:

```
Object.keys(discountCourse); // [‘rate’]
```

所以，如果你想把继承的属性和可枚举的属性分开处理，使用 hasOwnProperty 检查一个属性是否是继承的[。](https://medium.com/javascript-in-plain-english/how-to-check-if-an-object-has-a-specific-property-in-javascript-6086177014d1)

```
for (let key in discountCourse) {
  let value = course[key]; if (course.hasOwnProperty(key)) {
    // Handle enumerable property
  } else {
    // Handle inherited property
  }
}
```

到目前为止，我们有各种方法在 JavaScript 中遍历一个对象。就看你需要用最适合你的那个了。

如果您只需要处理数值，选择**对象。数值**

有时你也和键有关，那么就用 **Object.entries** 吧。

仅对于关键帧，使用 **Object.keys** 或**object . getownpropertymanames**

出于某种原因，您必须访问继承的属性。如果是这种情况，在循环中选择**。**

你知道其他方法吗？请在下面的评论中告诉我。

[](https://medium.com/javascript-in-plain-english/4-ways-to-compare-objects-in-javascript-97fe9b2a949c) [## JavaScript 中比较对象的 4 种方法

### #4 是为那些真正想磨练编码技能的人准备的。

medium.com](https://medium.com/javascript-in-plain-english/4-ways-to-compare-objects-in-javascript-97fe9b2a949c)