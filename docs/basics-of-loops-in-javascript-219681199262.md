# JavaScript 中循环的基础

> 原文：<https://javascript.plainenglish.io/basics-of-loops-in-javascript-219681199262?source=collection_archive---------4----------------------->

## 了解 JavaScript 中不同的循环方式

![](img/53c094ec0de876cc2f45206b38d774ab.png)

Image by [Tine Ivanič](https://unsplash.com/@tine999?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

什么是循环？

考虑我们需要打印`“hi”` 5 次。如果我们没有循环，我们要做的是

```
console.log("hi");
console.log("hi");
console.log("hi");
console.log("hi");
console.log("hi");
```

但是，如果我们知道如何使用循环，那么我们可以将上面的代码简化为

```
for(let i = 1; i <= 5; i++) {
   console.log('👋');
}
```

# 环

```
Looping is a way of executing the same function repeatedly .
```

换句话说

```
Loops are used to execute the same block of code again and again, until certain condition is met.
```

JavaScript 中不同类型的循环

*   为
*   在…期间
*   做什么

# 为

**语法**

```
for(initialValue; condition; step or increment) {
   // operation
}
```

举例。

```
for(let i = 0 ;i < 5; i = i + 1) {
    console.log(i);
}
```

在上面的代码中

*   **initialValue** → `i = 0` →将在循环开始时执行
*   **条件** → `i < len` →每次执行循环体之前检查
*   **操作** → console.log(一)；→循环体
*   **步进/增量** → `i = i+1` →每次迭代后每次执行。

让我们来分析一下它是如何工作的

```
At the beginning of the for loop i = 0 ; is initialised 
**First Iteration** i < len is checked i is printed

**After end of first iteration** 
   i = i + 1 is executed**Second iteration

  ** i < len check   print iThis will be repeated  until the **condition** becomes false.
```

上面的代码将像这样执行

```
for (let i = 0; i < 2; i++)  {
    console.log(i);
}// initial Value
let i = 0// if condition → execute body 
if (0 < 2)  → true → log(1)// increment 
i = i+1; 
i = 1;// if condition → execute body 
if (1 < 2)  → true → log(1)// increment 
i = i+1; 
i = 2;// if condition → execute body
if (2 < 2)  → false→ loop terminated
```

如果你想打破循环，而运行循环，我们可以使用`break`。`break`关键字将中断循环，break 关键字只在循环语句内部有效。

```
for(let i =0; i < 5; i = i +1 ){
   if(i > 2) {
      break;
   }
   console.log(i);
}// the above code will print0,1,2, when the value of i is 3 the loop breaks
```

在某些情况下，我们不需要对特定值执行操作，因为我们可以使用`continue`语句。`continue`语句将中断当前迭代。一旦 JavaScript 引擎执行 continue 语句，continue 语句下面的代码块将不会被执行。

```
for(let i =0; i < 5; i = i +1 ) {
    if(i%2 == 0) {
       continue; // if continue is 
    }
    console.log(i);
}1,3
```

初始值、条件和增量都是可选的

```
for(;;) {
   console.log("Infinite loop");
}
```

示例 1:没有初始值

```
**var i = 0;**for( ; i< 5 ; i = i+1 ) {
   console.log(i);
}
```

示例 2:无条件

```
var i = 0;for(;; i =i + 1) {
   **if( i < 5) { 
      break;
   }**
   console.log(i);
}// output0,1,2,3,4
```

示例 3:无增量

```
var i = 0;for(;;) {
   if( i < 5) { 
      break;
   }
   console.log(i);
   **i = i + 1;
}**// output0,1,2,3,4
```

# while 循环

**语法**

```
while(condition) {
   operation
}
```

在 while 循环中，我们只有检查条件的空间。

```
let i = 0;while(i < 5) {
   console.log(i);
   i = i + 1;
}//output 1,2,3,4,5
```

如果我们忘记了增量，那么它将导致无限循环

```
let i = 0;**while(i < 5) {**
   console.log(i);
**}**
```

在上面的代码中，I 的值总是 0，所以循环无限地运行。

`break`和`continue`也同时工作

```
let i = 0;while(i < 5) {
  **if(i == 2) {
     break;
  }**
  console.log(i);
}//output**0,1**
```

继续

```
let i = 0;while(i < 5) {
  **if(i == 2) {
     continue
  }**
  console.log(i); 
  i = i + 1; 
}//output**0,1,3,4,5**
```

# 做什么

句法

```
do {
   // body
} (condition is checked)
```

`do…while`循环将执行一次主体，然后检查条件，如果条件为`true`，则进行下一次迭代，否则停止循环。

```
let i = 0;do{ console.log(i);} while(**i != 0**);//output
0;
```

在上面的代码中，即使`i !=0`条件失败，do…while 的主体也会执行一次。

```
let i = 0;
do {
  console.log(i);
  i = i + 1;
} while (i < 3);// output**0,1,2**
```

我们也可以在循环中使用`break`和`continue`。

让我们牵起你的手🤚与我一起 [JavaScript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----98efbae5e8aa----------------------)。

[**给我买杯咖啡**](https://www.buymeacoffee.com/Jagathish) **。**

![](img/08a59dc268ec49ea11ebd45d7b225e0d.png)

[**Buy me a coffee**](https://www.buymeacoffee.com/Jagathish)**.**