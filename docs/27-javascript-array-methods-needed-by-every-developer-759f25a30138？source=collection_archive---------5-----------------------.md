# 每个开发人员都需要的 27 种 JavaScript 数组方法

> 原文：<https://javascript.plainenglish.io/27-javascript-array-methods-needed-by-every-developer-759f25a30138?source=collection_archive---------5----------------------->

## 所有重要 JavaScript 数组函数的综合列表。

在本文中，我将编译每个开发人员在日常工作中需要的 27 个 JavaScript 数组方法。在 JavaScript 中，[数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)是一个变量，用于存储相同或多种类型的多个项目。JavaScript 数组方法是内置的 JavaScript 数组函数，可以应用于数组。这些内置方法中的每一个都有一个独特的功能，它产生一个数组，使我们不必从头开始编写通用函数。

![](img/68a27d8ab685a121e4e02b71b8cdbcc2.png)

Photo by [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **1。Array.isArray( )**

我们列表中的第一项是 JavaScript 中的 isArray 方法，它决定传递的输入是否是数组。

✔️ **语法**

在下面的代码中,`**arr**`将是我们要检查的输入，是否是一个数组。`**isArray()**`函数的结果将是一个[布尔](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)值。

```
Array.isArray(arr)
```

➡️ **例子**

```
Array.isArray([1, 'something', true]);  
// trueArray.isArray({anything: 123}); 
// falseArray.isArray('some string');   
// falseArray.isArray(undefined);  
// false
```

# 2.长度

列表中的第二项是**不是方法**，而是数组的一个重要的**属性**。我想它一定也在列表中。从属性名可以清楚地看出，它返回数组中项目的长度(总数)。

![](img/058cb75965e754a0dbb30ec4d5dc3f09.png)

Photo by [patricia serna](https://unsplash.com/@sernarial?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

在下面的代码中，`**arr**`是我们的输入数组，通过使用`**length**`属性，我们将得到它的项数。

```
arr.length;
```

➡️ **举例**

```
[].length;
// 0[1, 'something', true].length;
// 3
```

# **3。forEach( )**

**forEach** 方法的行为类似于循环的[。但是你不必定义它将做多少次迭代，因为它将做的迭代相当于输入数组中的项数。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)

✔️ **语法**

在下面的代码中，`**arr**`将是我们的输入数组，`**forEach()**`方法将在其上执行。forEach 方法包含一个回调函数`**callbackFn**`作为 forEach 方法的参数。回调函数是一个 [ES6 箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)。但是您也可以使用传统的样式函数。这个 callbackFn 将进行相当于输入数组中存在的项目数的迭代。在每次迭代中，callbackFn 的[范围](https://developer.mozilla.org/en-US/docs/Glossary/Scope)内会有一些代码执行。此外，在每次迭代中，callbackFn `**arrItem**`和`**index**`的参数将使用连续的数组项和递增的索引值进行更新。

```
arr.forEach(callbackFn);
const callbackFn = (arrItem, index) => {callbackFn Scope Code Exec}// combining above statements together into one line of code
arr.forEach((arrItem, index) => {callbackFn Scope Code Exec});
```

如果你还是不明白，不要着急，让我们看看下面的例子。

➡️ **举例**

在下面的例子中，我们有一个输入数组 **['苹果'，'香蕉'，'胡萝卜']** ，它在 forEach callbackFn 中迭代。在 callbackFn 的范围内，我们打印 callbackFn 的参数。callbackFn 有两个参数， **arrItem** 和 **index** 。第二个参数 index 是可选的，如果你不需要它，你可以简单地省略它。因为 arr 有 3 个项目，所以 callbackFn 将迭代 3 次，并在每次迭代中打印 callbackFn 的参数。

```
['apple', 'banana', 'carrot'].forEach((arrItem, index) => {
  console.log(index + ' => ' + arrItem);
});// 0 => apple
// 1 => banana
// 2 => carrot
```

# **4。map( )**

`**map()**`方法的行为类似于 **forEach** ，但主要区别在于，它返回一个**新数组作为结果**。您在一个数组上执行 map()，并且在 map 方法 **callbackFn** 的范围内(如果您不清楚 callbackFn，请查看 forEach()方法的解释)，您执行一些语句。callbackFn 的每次迭代都为结果数组返回一个项。

✔️ **语法**

**arr** 是我们的输入数组，map 方法将在该数组上执行，并且在每次迭代中，一个新项将被添加到结果数组中，该结果数组将作为 **resultIteration** 返回。

```
arr.map((arrItem, index) => { return resultIteration });
```

➡️ **举例**

我们对 arr**【2，4，6，8，16】**执行 map 方法，并将结果保存在变量 **mapResult** 中。mapResult 将是一个数组，其中将填充 map 方法每次迭代的结果。在 map 方法 callbackFn 作用域中，我们将 arr 的每一项乘以 2，并将每次迭代的结果追加到 mapResult 中。经过 5 次迭代后，我们最终的 mapResult 数组将是[4，8，12，16，32]。

```
const arr = [2, 4, 6, 8, 16];const mapResult = arr.map(arrItem => arrItem * 2);console.log(mapResult);
// [4, 8, 12, 16, 32]
```

# 5.过滤器( )

`**filter()**`方法的行为类似于 map()并返回一个数组作为结果。但是顾名思义，它返回一个经过过滤的结果数组。您可以使用 filter callbackFn 范围内的条件来筛选结果数组。

![](img/ba2c862176453aa9a66aa94692aabbc5.png)

Photo by [Tyler Nix](https://unsplash.com/@tylernixcreative?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

**arr** 将是 filter 方法将执行的数组，并且在每次迭代中，一个新项将被添加到结果数组中，该结果数组将作为 resultIteration 返回。

```
arr.filter((arrItem, index) => { condition to return arrItem });
```

➡️ **举例**

我们对 arr**【2，4，6，8，16】**执行 filter 方法，并将结果保存在变量 **filterResult** 中。我们过滤输入数组的条件是 **( arrItem < 5 或 arrItem > 10 )** 。mapResult 将是一个数组，其中将填充符合条件的 arr 项。在 map 方法函数代码块中，我们将 arr 的每一项乘以 2，并将每次迭代的结果追加到 mapResult 中。

```
const arr = [2, 4, 6, 8, 16];const filterResult = 
  arr.filter(arrItem => arrItem < 5 || arrItem > 10);console.log(mapResult);
// [2, 4, 16]
```

# 6.排序( )

JavaScript 中的`sort()`方法按字母顺序对输入数组进行排序。默认情况下，sort()函数将值排序为**字符串**。

![](img/c7c292fca5bede837824b80429eb687d.png)

Photo by [Tolga Ulkan](https://unsplash.com/@tolga__?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

```
const arr = ['banana', 'orange', 'apple', 'mango'];arr.sort();
// ['apple', 'banana', 'mango', 'orange']
```

如果我们试图对一组数字进行排序，那么默认的排序方法的结果会有点混乱。

```
const arr = [22, 14, 0, 100, 89, 201];arr.sort();
// [0, 100, 14, 201, 22, 89]
```

如果使用 **sort()** 方法对一个数字数组进行排序，那么在默认情况下，它被视为一个字符串数组。所以“**22”**会在**“100”**之后，因为“22”的 **2** 比“100”的 **1** 大。我们可以通过提供一个**比较函数** (compareFn)来解决这个问题。

```
// compareFn for ascending order
const compareFnAsc = (a, b) =*> a - b;*// compareFn for descending order
const compareFnDes = (a, b) =*> b - a;*const arr = [22, 14, 0, 100, 89, 201];arr.sort(compareFnAsc);
// [0, 14, 22, 89, 100, 201]arr.sort(compareFnDes);
// [201, 100, 89, 22, 14, 0]
```

关于一些高级排序的例子，请看 [**这里**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 。

# 7.concat()

`**concat()**`方法用于合并两个或多个数组。它不改变现有的数组，而是返回一个新的结果数组。

![](img/330030d2afb3c810a53462502b4d8f98.png)

Photo by [Duy Pham](https://unsplash.com/@miinyui?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

您可以在 concat 方法中传递 **1** 到 **N** 个值作为参数。每个值可以是任何类型。在 result concat 方法中会将这些所有的值合并成一个结果数组。

```
arr.concat(value);
arr.concat(value0, value1, ... , valueN);
```

➡️ **举例**

让我们来看两个 concat 方法的例子。

```
// **Example 1** 
// Concat 2 arrays of string and number type
const letters = ['a', 'b', 'c'];
const numbers = [1, 2, 3];

letters.concat(numbers);
// ['a', 'b', 'c', 1, 2, 3]// **Example 2** 
// Concat a string array with 2 values (number and number array)
const letters = ['a', 'b', 'c'];

const alphaNumeric = letters.concat(1, [2, 3]);

console.log(alphaNumeric);
// ['a', 'b', 'c', 1, 2, 3]
```

# 8.每隔( )

我们列表中的下一个有用的数组方法是`**every()**`。此方法测试输入数组中的所有元素是否都通过了实现的条件。它返回一个布尔值。

✔️ **语法**

```
arr.every(callbackFn);
const callbackFn = (arrItem, index) => {
  condition to check on every arrItem
}
```

➡️ **例子**

在下面的例子中，我们有带数值的数组 **arr1** 和 **arr2** 。我们希望检查它们的数组值是否都大于或等于 0。对于 arr1，结果将为**假，**因为它包含-4 和-1。所以我们的条件会失败，但是 arr2 会通过每个数组项的条件，我们会得到**真**结果。

```
const arr1 = [89, 0, -4, 34, -1, 10];
const arr2 = [89, 0, 45, 34, 1, 100];arr1.every(arrItem => arrItem >= 0);
// falsearr2.every(arrItem => arrItem >= 0);
// true
```

# 9.一些( )

我们列表中的下一个方法是`**some()**`。这个方法和 **every()** 一模一样。不同之处在于我们检查输入数组中所有元素的条件，如果**任何元素**通过条件，我们返回 **true** 。如果输入数组的**个元素都没有通过条件，那么我们返回 **false** 。语法与 every()方法相同。**

➡️ **举例**

在这个例子中，我们有两个 number 类型的数组。我们正在检查的条件与 every()方法中的最后一个示例相同，任何 arrayItem 值**大于或等于 0** 。这里我们使用了 some()方法，所以如果任何一项通过我们的条件，那么结果将为真，否则为假。

```
const arr1 = [89, 0, 44, 34, -1, 10];
const arr2 = [-8, -45, -1, -100, -9];arr1.some(arrItem => arrItem >= 0);
// truearr2.some(arrItem => arrItem >= 0);
// false
```

# 10.包括( )

`**includes()**`方法检查输入数组的条目中是否包含某个值，并返回一个布尔值。

➡️ **举例**

```
const arr = [1, "2", 3, 4, 5];// Type of matching argument should match with the array entry
// In the code below 2 is of type number but in arr "2" is stringconsole.log(arr.includes(2));
// falseconsole.log(arr.includes("2"));
// true
```

# 11.加入( )

`**join()**`方法通过连接输入数组中的所有条目返回一个新字符串。如果数组只有一项，则该项将在不使用分隔符的情况下返回。向 join 方法传递一个**分隔符**参数是可选的。无论您将在 join 方法中传递什么参数字符串，输入数组的所有条目都将使用该字符串连接起来。

✔️ **语法**

```
arr.join();
arr.join(separator);
```

➡️ **举例**

```
const arr = ['Hello', 'World', '!'];arr.join();      
// 'Hello,World,!'
arr.join(', ');  
// 'Hello, World, !'
arr.join(' + '); 
// 'Hello + World + !'
arr.join('');    
// 'HelloWorld!'
arr.join(' ');    
// 'Hello World !'
```

# 12.减少( )

`**reduce()**`方法执行一个 reducer 功能，由用户应用于输入数组的项目。对数组的所有项运行 reducer 的最终结果返回一个值。

我认为理解 reduce()方法的最简单的方法是返回一个数组中所有项的总和。这里的 reducer 函数将遍历每个数组项，并在每一步将当前数组项的值添加到上一步的结果中(该结果是所有先前步骤的运行总和)，直到没有更多的项要添加为止。

✔️ **语法**

输入数组 **arr** 对其执行 **reduce()** 方法。reduce()方法有一个回调函数( **callbackFn** )作为参数。这个 callbackFn 基本上是 reducer 函数，它累加输入 arr 的所有值并返回一个**单个**值。reduce()方法的第二个参数是初始值( **initVal** )，是可选参数。

现在缩减器函数/回调函数(callbackFn)有 4 个参数。第一个参数 **total** 是前一次 callbackFn 调用的值。第二个参数是当前数组项的值(***curraritem***)。第三个参数是索引，它是可选的。最后一个参数是输入数组( **arr** )，也是可选的。

```
const callbackFn = *(total, currArrItem, index, arr)* =*>* { ... }
*arr*.reduce(callbackFn*, initVal*)// Combining above statements into one statement
*arr*.reduce(*(total, currArrItem, index, arr)* =*>* { ... }*, initVal*)
```

让我们看看这个例子，以便更好地理解 reduce()方法。

➡️ **举例**

在下面的例子中，我们使用 reduce()方法来累积输入 arr。

```
const arr = [1, 2, 3, 4];
const reducer = (prevVal, currVal) => prevVal + currVal;arr.reduce(reducer);
// 10// In the code below *initVal is set to 5, which means that we have // to init the sum with initial value 5: 5 + 1 + 2 + 3 + 4* arr.reduce(reducer, 5);
// 15
```

# 13.查找( )

列表中的下一个 JavaScript 数组方法是`find()`。返回输入数组中满足 find 方法 callbackFn 中给定条件的第一个元素的**值**。如果没有满足 callbackFn 条件的值，则返回 [**未定义**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) 。

![](img/96e055a3b59f31214fd492d7296391f2.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

```
arr.find((arrItem) => condition to check on arrItem);
```

➡️ **举例**

```
const arr = [5, 10, 100, 12, 8, 130, 44];const result = arr.find(arrItem => arrItem > 10);console.log(result);
// 100
```

# 14.findIndex()

`**findIndex()**`方法返回**满足 find 方法 callbackFn** 中给定条件的输入数组中第一个元素的**索引**。否则返回`**-1**`，表示没有数组项通过条件检查。语法与 find()方法相同，只是返回结果不同。

➡️ **举例**

```
const arr = [5, 10, 100, 12, 8, 130, 44];const result1 = arr.findIndex(arrItem => arrItem > 10);
const result2 = arr.findIndex(arrItem => arrItem > 1000);console.log(result1);
// 2console.log(result2);
// -1
```

# **15。indexOf( )**

这类似于 findIndex()方法，但区别在于`**indexOf()**`方法返回在输入数组中可以找到给定元素(作为 indexOf()方法的参数传递)的第一个索引。如果没有找到，那么它返回`**-1**`。

➡️ **举例**

```
const arr = [5, 10, 100, 12, 8, 130, 44];arr.indexOf(12);
// 3// As the input argument **'12'** is of type string and the input array // **arr** elements are of type number so there will be no match in 
// input arr
arr.indexOf('12');
// -1
```

# 16.填充( )

`**fill()**`方法用一个静态值改变输入数组中的所有元素。此方法返回修改后的数组。

✔️ **语法**

传递给 fill()方法需要第一个参数。如果我们没有在 fill()方法中传递第二个和第三个参数，那么第二个参数将默认为 0 (startIndex ),第三个参数将是数组(arr.length)索引中的最后一项。

```
arr.fill(value)// default startIndex = 0
arr.fill(value, startIndex)// default endIndex is arr.length
arr.fill(value, startIndex, endIndex)
```

➡️ **例子**

```
const arr1 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
arr1.fill(0);
// [0, 0, 0, 0, 0, 0, 0, 0]const arr2 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
arr2.fill(0, 5);
// ['A', 'B', 'C', 'D', 'E', 0, 0, 0]const arr3 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
arr3.fill(0, 5, 7);
// ['A', 'B', 'C', 'D', 'E', 0, 0, 'H']const arr4 = [1, 2, 3, 4, 5];
arr4.fill();
//[undefined, undefined, undefined, undefined, undefined]
```

# 17.切片( )

`**slice()**`方法是一个非常有用的 JavaScript 数组方法。它为 slice 方法中传递的给定参数复制一个原始数组。这些参数定义了新数组副本的**开始和结束索引。原始数组不会被修改。**

✔️ **语法**

```
arr.slice()
arr.slice(startIndex)
arr.slice(startIndex, endIndex)
```

➡️ **举例**

```
const arr = ['rats', 'sheep', 'cows', 'chickens', 'dogs', 1001];arr.slice();
// ['rats', 'sheep', 'cows', 'chickens', 'dogs', 1001]
arr.slice(3);
// ['chickens', 'dogs', 1001]
arr.slice(2, 5);
// ['cows', 'chickens', 'dogs']
arr.slice(-4);
// ['cows', 'chickens', 'dogs', 1001]
```

# 18.拼接( )

`**splice()**`方法通过移除或替换现有项目和/或添加新项目来改变数组的内容。原始数组不会被修改。

✔️ **语法**

```
arr.splice(startIndex)
arr.splice(startIndex, deleteCount)
arr.splice(startIndex, deleteCount, item1)
arr.splice(startIndex, deleteCount, item1, ..., itemN)
```

➡️ **例子**

```
**// Example 1
// Remove 0 items before index 2 & insert "drum"** let arr = ['angel', 'clown', 'mandarin', 'sturgeon'];
let removedArr = arr.splice(2, 0, 'drum');
// arr: ["angel", "clown", "drum", "mandarin", "sturgeon"]
// removedArr: []**// Example 2
// Remove 0 items before index 2 & insert "drum", "guitar"** let arr = ['angel', 'clown', 'mandarin', 'sturgeon'];
let removedArr = arr.splice(2, 0, 'drum', 'guitar');
// arr: ["angel", "clown", "drum", "guitar", "mandarin", "sturgeon"]
// removedArr: []**// Example 3
// Remove 1 element at index 3** let arr = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
let removedArr = myFish.splice(3, 1);
// arr: ["angel", "clown", "drum", "sturgeon"]
// removedArr: ["mandarin"]**// Example 4
// Remove 1 element at index 2 & insert "trumpet"** let arr = ['angel', 'clown', 'drum', 'sturgeon'];
let removedArr = arr.splice(2, 1, 'trumpet');
// arr: ["angel", "clown", "trumpet", "sturgeon"]
// removedArr: ["drum"]**// Example 5
// Remove 2 items from index 0 & insert "parrot", "anemone", "blue"** let arr = ['angel', 'clown', 'trumpet', 'sturgeon'];
let removedArr = arr.splice(0, 2, 'parrot', 'anemone', 'blue');
// arr: ["parrot", "anemone", "blue", "trumpet", "sturgeon"]
// removedArr: ["angel", "clown"]
```

# 19.反向( )

`**reverse()**`方法，顾名思义，反转一个输入数组。

➡️ **举例**

```
const arr1 = [1, 2, 3];
arr1.reverse();
// [3, 2, 1]const arr2 = ['1', '4', '3', 1, 'some string', 100];
arr2.reverse();
// [100, 'some string', 1, '3', '4', '1']
```

# 20.推送( )

JavaScript 中的`**push()**`方法将一个或多个项目(任何类型)添加到数组的末尾，并返回数组中项目的总更新计数。

![](img/b9c722967cfb7e339e18894d1c558c80.png)

Photo by [Robert Katzki](https://unsplash.com/@ro_ka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

```
arr.push(value);
arr.push(value0, value1, ... , valueN);
```

➡️ **举例**

```
const animals = ['cats', 'rats', 'sheep'];animals.push('cows');
// 4
console.log(animals);
// ['cats', 'rats', 'sheep', 'cows']
animals.push('chickens', 'dogs', 1001);
// 7
console.log(animals);
// ['cats', 'rats', 'sheep', 'cows', 'chickens', 'dogs', 1001]
```

# 21.流行( )

`**pop()**`方法从数组中移除最后一个项并返回该项。输入数组的长度减少 1。

➡️ **举例**

```
const arr = ['rats', 'sheep', 'cows', 'chickens', 'dogs', 1001];arr.pop();
// 1001
console.log(arr);
// ['rats', 'sheep', 'cows', 'chickens', 'dogs']
```

# 22.shift()

JavaScript 的`**shift()**`方法从数组中移除第**个**项并返回该项。输入数组的长度减少了 1，与 pop()方法相同。

➡️ **举例**

```
const arr = ['rats', 'sheep', 'cows', 'chickens', 'dogs', 1001];arr.shift();
// 'rats'
console.log(arr);
// ['sheep', 'cows', 'chickens', 'dogs', 1001]
```

# 23.未移位( )

方法将一个或多个条目添加到数组的开头，并返回数组的新长度。语法与 push()相同。push()方法放在数组的末尾，而 unshift()方法放在数组的开头。

➡️ **举例**

```
const animals = ['cats', 'rats', 'sheep'];animals.unshift('cows');
// 4
console.log(animals);
// ['cows', 'cats', 'rats', 'sheep']
animals.unshift('chickens', 'dogs', 1001);
// 7
console.log(animals);
// ['chickens', 'dogs', 1001, 'cows', 'cats', 'rats', 'sheep']
```

# 24.的数组( )

`**Array.of()**`方法从可变数量的参数创建一个新的`Array`实例，而不管参数的数量或类型。

➡️ **举例**

```
Array.of(1);         
// [1]Array.of(1, 2, 3);   
// [1, 2, 3]Array.of(undefined); 
// [undefined]
```

# 25.数组. from()

静态方法从一个类似数组或可迭代的对象创建一个新的浅拷贝的`Array`实例。

✔️ **语法**

```
Array.from(value, callbackFn)
```

➡️ **举例**

```
Array.from('hello');
// ['h', 'e', 'l', 'l', 'o']Array.from([1, 2, 3], arrItem => arrItem * 4);
// [4, 8, 12]
```

# 26.扁平( )

`**flat()**` JavaScript 方法创建一个新数组，所有子数组项递归地连接到该数组中，直到指定的深度。

![](img/a806ebc4b22eddd64f590ba1d0584f57.png)

Photo by [Matteo Paganelli](https://unsplash.com/@matteopaga?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

✔️ **语法**

深度级别，指定嵌套数组结构应展平的深度。如果没有参数传递给 flat()方法，默认深度为 **1，**。或者可以传递任何深度参数。

```
arr.flat()
arr.flat(depth)
```

➡️ **举例**

让我们看一些 JavaScript 中的 flat()方法的例子。

```
**// Example 1**
const arr1 = [0, 1, 2, [3, 4]];console.log(arr1.flat());
// [0, 1, 2, 3, 4]**// Example 2** const arr2 = [0, 1, 2, [[[3, 4]]]];console.log(arr2.flat(2));
// [0, 1, 2, [3, 4]]
console.log(arr2.flat(3));
// [0, 1, 2, 3, 4]
```

# 27.在( )

`**at()**`方法接受一个整数作为参数，并返回输入数组中该索引处的项目。at()方法的参数可以是正的，也可以是负的。负整数从输入数组的最后一项开始倒数。

✔️ **语法**

```
arr.at(index)
```

➡️ **举例**

```
const arr = ['chickens', 'dogs', 1001, 'cows', 'cats', 'sheep']arr.at(1);
// 'dogs'
arr.at(-1);
// 'sheep'
arr.at(2);
// 1001
arr.at(-2);
// 'cats'
arr.at(-100);
// undefined
```

# 结论

如果你有扩展这个列表的想法，请在评论中提出你的想法。我很乐意用你的建议来扩展这个列表。如果你喜欢这篇文章，并希望在未来看到更多的内容，那么别忘了关注我。编码快乐！

# 关于作者

我在 [Lucid 担任全栈开发人员。工作室](https://medium.com/u/cb727ce3b3c0?source=post_page-----4ef4ecbdcc1b--------------------------------)我非常有兴趣学习并与社区分享我的知识。如果你喜欢我的工作，那就在 LinkedIn 上联系我:Sayyed Hammad Ali。

# 你可能想读的其他文章

[](/27-essential-one-line-javascript-functions-used-by-developers-daily-2cda9826700e) [## 开发人员日常使用的 27 个基本的单行 JavaScript 函数

### 在本文中，我编译了一个 27 行 JavaScript 函数的列表，这些函数每天都在使用，每个人都需要…

javascript.plainenglish.io](/27-essential-one-line-javascript-functions-used-by-developers-daily-2cda9826700e) 

**来源信用**

1.  [https://developer.mozilla.org/en-US/](https://developer.mozilla.org/en-US/)
2.  https://www.w3schools.com/jsref/jsref_obj_array.asp

*更多内容尽在*[***plain English . io***](http://plainenglish.io/)