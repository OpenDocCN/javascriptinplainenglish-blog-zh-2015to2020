# JavaScript 算法:足球进球总数

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-soccer-goal-totals-93223792b67f?source=collection_archive---------2----------------------->

## 我们将解决并研究在 JavaScript 中，在一个函数中添加多个数字总和的许多方法。

![](img/5cccef5b840baeea9d010fcf8f3b86a1.png)

我们将编写一个名为`goals`的函数，它将接受三个整数`laLigaGoals`、`copaDelReyGoals`和`championsLeagueGoals`作为输入。

因此，有一个名叫莱昂内尔·梅西的足球运动员，他因在以下联赛中进球而闻名:LaLiga，Copa del Rey，冠军因此得名。这个函数的目的是返回所有三个联赛的总进球数。

这个函数非常简单。你要做的就是把所有的参数加在一起，然后返回值。

```
function goals (laLigaGoals, copaDelReyGoals, championsLeagueGoals) {
  return laLigaGoals + copaDelReyGoals + championsLeagueGoals;
}
```

你也可以把函数中的所有参数放入一个数组中，然后你可以使用 reduce 方法得到数组中所有数字的总值。

```
const goals = (...goalsArray) => goalsArray.reduce((a, b) => a + b);
```

或者您可以在函数内部自己创建数组，然后使用 reduce 方法。

```
function goals (laLigaGoals, copaDelReyGoals, championsLeagueGoals) {
  let goals = [ laLigaGoals, copaDelReyGoals, championsLeagueGoals ];
  return goals.reduce( ( a, b ) => a + b ,0);
}
```

或者

```
function goals (laLigaGoals, copaDelReyGoals, championsLeagueGoals) {
  return [].reduce.call(arguments, (a, b)=> a + b);
}
```

在任何涉及数字相加的函数中，有许多方法可以解决这个简单的算法。

在 JavaScript 中，还有哪些有趣或巧妙的方法可以将多个数字相加？

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://medium.com/@endubueze00/javascript-algorithm-calculate-body-mass-index-6f14dce4075d) [## JavaScript 算法:计算身体质量指数

### 我们写了一个函数，计算你的身体质量指数，并确定你是否超重，体重不足，肥胖或正常。

medium.com](https://medium.com/@endubueze00/javascript-algorithm-calculate-body-mass-index-6f14dce4075d) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-convert-a-boolean-to-a-string-d720ac34ecd5) [## JavaScript 算法:将布尔值转换为字符串

### 我们来看看将布尔值转换成字符串的许多方法

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-convert-a-boolean-to-a-string-d720ac34ecd5) [](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f) [## JavaScript 算法:Pangrams

### 对于今天的算法，我们要写一个叫做 pangrams 的函数，它接受一个字符串 s 作为输入。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-pangrams-3e6add10f38f)