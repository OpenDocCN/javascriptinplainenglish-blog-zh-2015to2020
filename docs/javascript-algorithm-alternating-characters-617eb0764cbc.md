# JavaScript 算法:交替字符

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-alternating-characters-617eb0764cbc?source=collection_archive---------5----------------------->

![](img/f2a8d753d320fe81e0eb99f02423c088.png)

对于今天的算法，我们将编写一个名为`alternatingCharacters`的函数，它将接受一个字符串`s`作为输入。

给你一个只包含字符`A`和`B`的字符串。该函数的目标是改变字符串，使字符交替，没有匹配的相邻字符。该函数将返回匹配的相邻字符删除的数量，这将产生一个`A`和`B`仅替换字符串。让我们看一个例子:

```
let s = "ABAABBAB";
```

在我们上面输入的字符串中有一些重复的相邻字符。我们将突出显示可以删除的字符，以获得交替字符串。

```
"ABA**A**B**B**AB" --> remove A at position 3 and B at position 5
```

我们去掉这两个字符，这样我们就可以得到一个不包含相邻匹配字符的字符串。该函数将返回`2`。

让我们把这变成代码:

```
let deleteCount = 0;
```

我们的`deleteCount`变量将保存匹配的相邻字符删除的数量。

```
for(let i = 0; i < s.length; i++){
    if(s[i] === s[i+1]){
      deleteCount++;
    }
}
```

接下来，我们遍历输入字符串中的字符。我们使用 if 语句来检查当前字符是否与字符串中它旁边的字符匹配。如果它们匹配，当前字符是不需要的，所以我们增加变量`deleteCount`。

循环结束后，我们返回`deleteCount`变量。

```
return deleteCount;
```

我们的短函数到此结束。以下是剩余的代码:

```
function alternatingCharacters(s) {
  let deleteCount = 0; for(let i = 0; i < s.length; i++){
    if(s[i] === s[i+1]){
      deleteCount++;
    }
  }

  return deleteCount;
}
```

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [## JavaScript 算法:Pangrams

### 对于今天的算法，我们要写一个叫做 pangrams 的函数，它接受一个字符串 s 作为输入。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-angry-professor-89570ba8d863) [## JavaScript 算法:愤怒的教授

### 对于今天的算法，我们将编写一个名为 angryProfessor 的函数，它将接受两个输入:一个整数…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-angry-professor-89570ba8d863) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85) [## JavaScript 算法:电子商店

### 对于今天的算法，我们将编写一个名为 getMoneySpent 的函数，它将接受三个输入:两个数组…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-electronics-shop-240a4fcb0e85)