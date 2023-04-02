# JavaScript 算法:给学生评分

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-grading-students-210a89c5496f?source=collection_archive---------2----------------------->

![](img/8eee7839c99a38c2c7bd0292d238b521.png)

对于今天的算法，我们要写一个名为 gradingStudents 的函数，在这个函数中，我们决定了你班上所有学生的最终成绩。您将收到一个包含学生成绩的数组输入。该函数的输出将是一个包含学生最终成绩的数组。首先，让我们解释一下在决定最终分数之前必须满足的条件。

![](img/30c8c8bbf8a7b98d6e2fdaa50ead0f7d.png)

假设我们在一所学校里，每个学生都得到了一个从 0 到 100 的分数。任何低于 40 的分数都是不及格。让我们见见你的老师，他将决定你是通过还是失败。

![](img/021ffbe428122190bdb4c67c5bafe3a4.png)

The bushiest beard of them all.

你的老师会在每种情况下对你的分数进行四舍五入:

1.  如果当前分数和下一个 5 的倍数之间的差值小于 3，则将分数向上舍入到 5 的倍数。
2.  如果人数不足 40 人，四舍五入后，原成绩不变。

以下是一些例子:

如果一个学生的分数是 43，下一个 5 的倍数是 45，这两个数字之间的差只有 2，所以你要把分数四舍五入到 45。

如果一个学生的分数是 34，下一个是 5 的倍数的数字是 35，差额是 1，但四舍五入后的分数小于 40，所以你不用四舍五入。等级将保持在 34。

如果一个学生的分数是 60，那么下一个是 5 的倍数的数字是 65，但两者之差是 5，所以你不用向上取整。差值必须小于 3。

现在让我们开始为此编写代码。我们将创建一个名为 **roundedGrades** 的空数组。这是 gradingStudents 函数将输出的数组。

```
let roundedGrades = [];
```

除了 roundedGrades，我们还将在 gradingStudents 函数之外创建另一个函数或辅助函数。

```
function multipleOfFive(x) {
    let counter = 0; while (x % 5 != 0) { // if not divisible by 5, keep going
        x++;
        counter++;
    }
    return counter;
}
```

助手函数是可以在另一个函数中调用以执行特定任务的函数。这是为了让代码更容易阅读。我们的助手函数叫做 **multipleOfFive** 。

回到我们上面的例子，为了四舍五入一个分数，一个要求是这个分数和下一个 5 的倍数之差小于 3。该函数将返回差值(T4 计数器变量)。

这个函数的输入是我们的学生成绩。在我们的 while 循环中，我们将增加每个循环的输入，直到这个数字被 5 整除。百分号或模数运算符返回一个操作数除以第二个操作数后的余数。在这个函数中，如果余数是 0，这意味着这个数可以被 5 整除。

同样在 while 循环中，我们递增计数器，以便当 while 循环结束时，我们可以返回达到 5 的倍数所用的步数。

回到我们的 **gradingStudents** 函数，我们将遍历保存学生成绩的输入数组。

```
for (let i = 0; i < grades.length; i++) { let difference = multipleOfFive(grades[i]);
    let finalGrade = difference + grades[i]; if ((difference < 3) && (finalGrade >= 40)) {
        roundedGrades.push(finalGrade);
    } else {
        roundedGrades.push(grades[i]);
    }
}
```

在 for 循环中，每条语句都执行以下操作:

**差**变量是我们的 multipleOfFive 函数的输出。

**finalGrade** 变量是我们的四舍五入分数，或者是 5 的倍数。我们将**差异**变量添加到我们当前的等级**等级【I】**。

在了解了对学生成绩进行舍入所需的要求后，我们检查**差异**变量是否小于 3，以及**最终成绩**是否大于等于 40。如果这两个要求都满足，我们将**最终等级**添加到我们的**舍入等级**数组中。如果当前学生成绩不满足这两个要求，那么只需将该成绩添加到 **roundedGrades** 数组中。

for 循环结束后，我们返回 **roundedGrades** 数组。

```
return roundedGrades;
```

以下是我们的完整代码:

```
function gradingStudents(grades) {
    let roundedGrades = [];
    for (let i = 0; i < grades.length; i++) {
        let difference = multipleOfFive(grades[i]);
        let finalGrade = difference + grades[i];

        if ((difference < 3) && (finalGrade >= 40)) {
            roundedGrades.push(finalGrade);
        } else {
            roundedGrades.push(grades[i]);
        }
    }
    return roundedGrades;
}function multipleOfFive(x) {
    let counter = 0;
    while (x % 5 != 0) {
        x++;
        counter++;
    }
    return counter;
}
```

![](img/3ce7fa344bd947d41f3334469672cb4e.png)

要查看更多 JavaScript 算法的最新解决方案，请查看以下链接:

[](https://levelup.gitconnected.com/javascript-algorithm-time-conversion-71dc15dd13b8) [## JavaScript 算法:时间转换

### 对于今天的算法，我们将创建一个名为 timeConversion 的函数。这个函数将接受一个字符串作为…

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-time-conversion-71dc15dd13b8) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-birthday-cake-candles-86da2c686634) [## JavaScript 算法:生日蛋糕蜡烛

### 对于今天的算法，我们将创建一个名为生日蛋糕蜡烛的函数。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-birthday-cake-candles-86da2c686634)