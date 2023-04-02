# 如何使用 Node 中的 Readline 读取测试用例？

> 原文：<https://javascript.plainenglish.io/how-to-read-test-cases-from-stdin-and-files-using-readline-in-node-js-cf5ef37e6b5e?source=collection_archive---------0----------------------->

![](img/00ef4df43d8f562f30ebc716adb98457.png)

Photo by [Max Chen](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有时当我们在一个在线挑战平台上解决一个编码问题时，我们需要用不同的测试用例或者测试用例的许多文件来测试我们的算法。

在这篇文章中，我试着写了一个 [**二分搜索法**](https://en.wikipedia.org/wiki/Binary_search_algorithm) 算法，它允许在一个排序的数组中找到一个元素的索引，我使用来自 stdin 和一个文本文件的一些测试用例来测试它。

所以让我们从编写二分搜索法算法开始我们的例子；

首先，我们检查数组是否为空，并返回-1 表示该元素不存在。

```
// verify if the array is not null and contains elements  
if (!array || !array.length) {
    return -1;    
}
```

然后，我们声明两个变量`start`和`end`，并通过数组的第一个和最后一个索引来初始化它们。

```
let start = 0;    
let end = array.length - 1;
```

之后，我们迭代数组，直到条件`start < end`无效；

```
while (start < end) {        
const middle = parseInt((start + end) / 2, 10);        
if (array[middle] === element) {            
     return middle;        
} else if (array[middle] < element) { 
     start = middle + 1;        
} else {            
     end = middle;        
}    
}
```

最后，我们检查最后一个案例时的`start = end`

```
// end condition: start === end    
if (start !== array.length && array[start] === element) return start
```

当数组中不存在该元素时，我们返回-1。

在实现了我们的基本算法之后，现在让我们进入主要部分；

为了读取测试用例，我们使用模块`readline`，它提供了一个接口，用于从[可读的](https://nodejs.org/api/stream.html#stream_readable_streams)流(比如`[process.stdin](https://nodejs.org/api/process.html#process_process_stdin)`)中一次一行地读取数据。所以我们必须使用`createInterface()`方法从`readline.Interface`类创建一个实例。

```
// create an interface
const rl = readline.createInterface({    
  input: process.stdin, // readable Stream: stdin
});
```

之后，接口应该监听`line`事件，每当`input`流接收到一个行尾输入(`\n`、`\r`或`\r\n`)时就会发出该事件。这通常发生在用户按下`<Enter>`或`<Return>`键时。

在我们的例子中，测试用例具有这样的格式:1≤T≤100；1≤ N ≤ 1000

```
T
N target
e1 e2 e3 ...... eN
```

我们设置了我们的接口，并创建了一个监听器来获取新行存在时的行值；

```
rl.on('line', (line) => {   
// Extract the first line value = T
if (!T) {        
T = line        
return    
}    
count++
// Get the value of N and the element to fetch in the arrayif (count % 2) {        
[ N, element ] = line.split(' ').map(e => Number(e));    
}    
if (!(count % 2)) {   
// extract the array element from the line 

const array = line.split(' ').map(e => Number(e)); // fetch the element's index in the array    

const fetchedIndex = binarySearch(array, element); 

console.log(`Case #${parseInt(count / 2, 10)}:`, fetchedIndex)            }  
// close the interface when the number or lines exceeds 2 * Tif (count >= 2 * T){        
rl.close();    
}   
})
```

当我们从文件中读取值时，唯一的区别是输入属性的初始化；

```
// Creation of readline instanceconst 
rl = createInterface({  
input: fs.createReadStream(filePath), // readable Stream: file  crlfDelay: Infinity});
```

**总结**

在本文中，我们遇到了一个简单的例子，我们使用[***readline***](https://nodejs.org/api/readline.html#readline_readline)*从 stdin 或文件中读取数据。*

*在打字或代码错误的情况下，请随时给我留下评论。*

*代码:*

*[](https://github.com/slim-hmidi/readline) [## 超薄 HMI di/读取线

### 一个使用 realine 接口从标准输入或文件中获取数据的例子

github.com](https://github.com/slim-hmidi/readline)*