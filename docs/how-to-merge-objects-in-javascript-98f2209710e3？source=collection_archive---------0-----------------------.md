# 如何在 JavaScript 中合并对象

> 原文：<https://javascript.plainenglish.io/how-to-merge-objects-in-javascript-98f2209710e3?source=collection_archive---------0----------------------->

## 了解如何在 JavaScript 中合并两个或多个对象。

![](img/ec52c3b404d5af531739e398405611a1.png)

learnersbucket.com

> 我开了一个名为[learnersbucket.com](https://learnersbucket.com/)的博客，在那里我写了关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) 、[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/)和[算法](https://learnersbucket.com/examples/topics/algorithms/)的文章，帮助其他人破解编码面试。在 Twitter[上关注我的定期更新。](https://twitter.com/LearnersBucket)

JavaScript 中几乎所有的东西都是对象，但它仍然没有任何好的方法来合并两个或更多不同的对象。

在 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro) 之后，增加了新的方法，可以用来合并 JavaScript 中的对象。

有两种不同类型的合并可以在对象上执行。
1)。浅层合并
2)。深度合并

# 浅层合并对象

在浅层合并中，只合并对象拥有的属性，而不会合并扩展属性或方法。

# 使用`...` [展开](https://learnersbucket.com/tutorials/es6/spread-and-rest-operators-in-javascript)操作符

`...` spread 操作符将对象的所有属性复制到另一个对象中。

```
let obj1 = {
  name: 'prashant',
  age: 23,
}let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript'
}let merge = {...obj1, ...obj2};console.log(merge);/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

# **使用**[**object . assign()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)**方法**

`Object.assign(target, source1, soure2, ...)`方法将源对象所有可枚举的自身属性复制到目标对象并返回目标对象。

```
let obj1 = {
  name: 'prashant',
  age: 23,
}let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript'
}let merge = Object.assign({}, obj1, obj2);;console.log(merge);/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

我们使用了一个空对象`{}`作为目标，并传递了应该合并到目标中的源对象。

# 使用自定义函数合并对象

我们也可以创建自定义函数来合并两个或多个对象。

```
let merge = (...arguments) => {

  // Create a new object
  let target = {}; // Merge the object into the target object
  let merger = (obj) => {
     for (let prop in obj) {
       if (obj.hasOwnProperty(prop)) {
         // Push each value from `obj` into `target`
         target[prop] = obj[prop];
       }
     }
  }; // Loop through each object and conduct a merge
  for (let i = 0; i < arguments.length; i++) {
    merger(arguments[i]);
  } return target;
}let obj1 = {
  name: 'prashant',
  age: 23,
}let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript'
}let merged = merge(obj1, obj2);;console.log(merged);/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  qualification: "BSC CS"
}
*/
```

# 深度合并对象

要深度合并一个对象，我们必须复制它自己的属性和扩展属性，如果它存在的话。

最好的方法是创建一个自定义函数。

```
let merge = (...arguments) => {
  let target = {}; // Merge the object into the target object
  let merger = (obj) => {
      for (let prop in obj) {
        if (obj.hasOwnProperty(prop)) {
          if (Object.prototype.toString.call(obj[prop]) === '[object Object]'){
            // If we're doing a deep merge 
            // and the property is an object
            target[prop] = merge(target[prop], obj[prop]);
          } else {
            // Otherwise, do a regular merge
            target[prop] = obj[prop];
          }
         }
      }
  }; //Loop through each object and conduct a merge
   for (let i = 0; i < arguments.length; i++) {
      merger(arguments[i]);
   } return target;
};let obj1 = {
  name: 'prashant',
  age: 23,
  nature: {
    "helping": true,
    "shy": false
  }
}let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript',
  nature: {
    "angry": false,
    "shy": true
  }
}console.log(merge(obj1, obj2));/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    helping: true,
    shy: true
  },
  qualification: "BSC CS"
}
*/
```

我们可以将浅层复制和深层复制的函数组合在一起，创建一个基于传递的参数执行 merge 的函数。

如果我们将`true`作为第一个参数传递，那么它将执行深度合并，否则它将执行浅层合并。

```
let merge = (...arguments) => { // Variables
   let target = {};
   let deep = false;
   let i = 0; // Check if a deep merge
   if (typeof (arguments[0]) === 'boolean') {
      deep = arguments[0];
      i++;
   } // Merge the object into the target object
   let merger = (obj) => {
      for (let prop in obj) {
         if (obj.hasOwnProperty(prop)) {
           if (deep && Object.prototype.toString.call(obj[prop]) === '[object Object]') {
           // If we're doing a deep merge 
           // and the property is an object
              target[prop] = merge(target[prop], obj[prop]);
          } else {
           // Otherwise, do a regular merge
           target[prop] = obj[prop];
          }
       }
     }
   }; //Loop through each object and conduct a merge
    for (; i < arguments.length; i++) {
       merger(arguments[i]);
    } return target;
};let obj1 = {
  name: 'prashant',
  age: 23,
  nature: {
    "helping": true,
    "shy": false
  }
}let obj2 = {
  qualification: 'BSC CS',
  loves: 'Javascript',
  nature: {
    "angry": false,
    "shy": true
  }
}//Shallow merge
console.log(merge(obj1, obj2));/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    shy: true
  },
  qualification: "BSC CS"
}
*///Deep merge
console.log(merge(true, obj1, obj2));
/*
Object {
  age: 23,
  loves: "Javascript",
  name: "prashant",
  nature: Object {
    angry: false,
    helping: true,
    shy: true
  },
  qualification: "BSC CS"
}
*/
```

还有其他一些库可以用来合并两个对象，比如

# Jquery 的 [$。](https://api.jquery.com/jQuery.extend/#jQuery-extend-target-object1-objectN)扩展()【方法】

`$.extend(deep, copyTo, copyFrom)`可用于对 javascript 中的任何数组或对象进行完整的深度复制。

# 洛达什[合并()](https://www.npmjs.com/package/lodash.merge)法

它将通过递归执行深度合并来合并对象和数组。

*原载于 2019 年 6 月 6 日*[*https://learnersbucket.com*](https://learnersbucket.com/examples/javascript/how-to-merge-objects-in-javascript/)*。*