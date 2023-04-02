# 如何在 JavaScript 中使用新的 Nullish 合并操作符

> 原文：<https://javascript.plainenglish.io/nullish-coalescing-operator-for-javascript-8f502b970ba8?source=collection_archive---------2----------------------->

## 简明、有用的 JavaScript 课程——让它变得简单。

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Image of two monitors with code in their screens

无效合并运算符"？?"添加一个新的短路运算符，当其左侧操作数为空或未定义时，返回其右侧操作数，否则返回其左侧操作数。

他目前的阶段是第 4 阶段，这意味着该添加已经准备好包含在正式的 ECMAScript 标准中。

它可以在 Chrome 或 Firefox 等最新版本中运行。

这是一个简单的用例

```
const team = null ?? 'A team';
console.log(team);
//A team
```

## 零融合算子动机

你肯定知道其他的短路操作符，比如“&&”或者“||”这些运算符用于处理“真”和“假”值。但是，首先，JavaScript 中的“假”或“真”值是什么？

虚假值:

*   布尔假
*   空值
*   不明确的
*   数字 0
*   号码 NaN
*   BigInt 0n
*   空字符串""

真实值:

除先前值外的所有值。

“&&”和“||”运算符的问题是因为他对真和假的定义导致了错误。这些运算符适用于空值或未定义的值，但是许多假值可能会产生意外的结果。

在本例中，我希望在处理响应时，我们将变量“enableAudio”的值赋为零。相反，它将默认值设置为 1，因为响应的零值被解释为假值。

```
const response = {
     enableAudio : 0
}const enableAudio = response.enableAudio **||** 1;
enableAudio
**//1**
```

**零化凝聚算子？?"**的行为与运算符“||”非常相似，除了**在计算运算符时不使用“真值”。相反，它使用“nullish”的定义，**，这意味着该值严格等于 null 或未定义。

在下面的示例中，变量“enableAudio”的值为零，并且使用了 nullish 合并运算符“？?"这是一个非空值。因此，表达式计算到左侧。

```
const response = {
     enableAudio : 0
}const enableAudio = response.enableAudio **??** 1;
enableAudio
**//0**
```

使用运算符“||”时，如果第一个操作数为真，则计算第一个操作数。否则，它计算为第二个。使用 nullish 合并运算符时，如果第一个运算符为 falsy 但不是 Nullish，则计算第二个操作数。

```
console.log(false || true);//true
console.log(false ?? true);//false
```

这里，零被评估为一个假值；因此，该表达式使用“||运算符计算右边的值但是，使用 Nullish 合并运算符，零不为空。因此，表达式的计算结果是左边的值。

```
console.log(0 || 1); //1
console.log(0 ?? 1); //0
```

在下一个示例中，如果您使用运算符“||”对其求值，您获得的结果是右边的值，因为空字符串是一个假值。相反，使用 Nullish 合并运算符，空字符串不为 null，因此表达式的计算结果是左边的值。

```
console.log('' || 'Hello!');//Hello      
console.log('' ?? 'Hello');//''
```

总之，运算符“||”将值处理为 falsy 或 truthy，而运算符“？?"将值作为 nullish 处理。这意味着使用运算符“||”时，如果第一个操作数是未定义的、null、0、false、NaN 或空字符串，则使用运算符“|”计算第二个操作数。?"如果第一个操作数为空或未定义，则计算第二个操作数。

## 结论

使用运算符“||”计算表达式时，隐藏了大量错误，如果使用新的 nullish 合并运算符“|”，就可以避免这些错误。?"此外，您还可以防止许多代码样板。

我希望这有助于您了解无效合并运算符"？?"，谢谢你读我。