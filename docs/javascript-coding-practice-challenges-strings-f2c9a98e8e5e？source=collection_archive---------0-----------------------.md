# JavaScript 编码实践挑战—字符串

> 原文：<https://javascript.plainenglish.io/javascript-coding-practice-challenges-strings-f2c9a98e8e5e?source=collection_archive---------0----------------------->

## JavaScript 中的问题及解决方案

![](img/3661604eb532f7b40d0b5bdcdce0ec2c.png)

Photo by [Daniel McCullough](https://unsplash.com/@d_mccullough?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **挑战 1 —计算重复字符**

一个很常见的编程面试问题是，给定一个字符串，你需要找出字符串中的重复字符。

**输入** : `“adsjfdsfsfjsdjfhacabcsbajda”`

**输出** : `{ a: 5, b: 2, c: 2, d: 4, f: 4, j: 4, s: 5 }`

这个问题有两个解决方案。

## 解决方案# 1

第一种解决方案是迭代字符串字符，并使用一个`dictionary <key, value>`将字符存储为键，将出现的次数存储为值。

*   如果当前角色从未被添加到`dictionary`，那么将其添加为`<character, 1>`。
*   如果当前字符存在于`dictionary`中，那么简单地将其`occurrences`增加 1。

## **解决方案 2**

我们把字符串变成一个字符数组，然后对数组进行排序，把它们放在一起形成一个新的字符串。随着这个字符串被排序，我们可以使用一个正则表达式(这里是 `/(.)\1+/g`)来匹配重复的字符。

这是两种情况下的输出。

```
const str = “adsjfdsfsfjsdjfhacabcsbajda”count_duplicate_characters(str){ a: 5, b: 2, c: 2, d: 4, f: 4, j: 4, s: 5 }
```

你能用 Unicode 字符解决这个问题吗？在这里分享你的解决方案和你的评论。

# 挑战 2— **找到第一个不重复的字符**

编写一个 JavaScript 程序来查找字符串中第一个不重复的字符是编码挑战中的一个常见问题。我们可以在字符串的单次遍历或更完整/部分的遍历中解决这个问题。

**输入** : `"cbcbdde"`

**输出** : `e`

## 解决方法

我们将在单遍历方法中重新解决这个问题。这里，我们使用一个有 256 个条目的`flag`数组来存储不重复的字符，然后我们找到包含一个不重复字符的最小索引。

尝试测试这种方法。

```
console.log(first_non_repeated_character("cbcbdde"))
<< **e**
```

# 挑战 3 — **颠倒字母和单词**

倒写字母意味着你把某些字母(或数字)倒着写。这有时被称为镜像写入。

**输入** : `"I evol uoy os !hcum"`

**输出** : `I love you so much!`

## 解决方法

使用内置函数很容易颠倒字母和单词。

这是输出。

```
var test_string = "emocleW ot SJ ni nialP hsilgnE"
reverse(test_string)<< **Welcome to JS in Plain English**
```

# 挑战# 4 — **生成所有排列**

涉及排列的问题通常也涉及`recursivity`。基本上，递归被定义为一个给定初始状态的过程，每个`successive state`根据`preceding state`来定义。

**输入** : `ABC`

**输出** : `ABC, ACB, BCA, BAC, CAB, CBA`

## 解决方法

你可以这样称呼它

```
permute_and_print(“ABC”)
```

这将产生以下输出:

```
ABC ACB BCA BAC CAB CBA
```

# 挑战 5— **检查一个字符串是否是回文**

快速提醒一下，a `palindrome`倒转时看起来没有变化。这意味着加工 a `palindrome`可以从两个方向进行，并将获得相同的结果

比如`madam`这个词是回文，而`madame`这个词不是。

## 解决方法

实现依赖于 while 语句。

我们可以用一种更简洁的方法重写前面的解决方案，这种方法将依赖于一个`for`语句而不是一个`while`语句，如下所示。

我们能减少到一行代码吗？

# 挑战# 6— **按长度排序字符串数组**

给我们一个字符串数组，我们需要按照字符串长度的递增顺序对数组进行排序

**输入:** `["You", "are", "beautiful", "looking"]`
**输出:** `[“You", "are", "looking", "beautiful"]`

## **解决方案**

排序时首先想到的是使用比较器。在这种情况下，解决方案应该比较字符串的长度，因此通过为给定数组中的每个字符串调用`.length`来返回整数。

# 挑战 7— **检查字符串是否包含子字符串**

## **解决方案**

一个非常简单的一行代码解决方案依赖于`.includes()`函数。

```
const string = “foo”
const substring = “oo”string.includes(substring)
<< **true**
```

或者，可以依靠如下`String.prototype.indexOf()`实施解决方案。

```
string.indexOf(substring) !== -1
<< **true**
```

# 挑战 8 — **检查两个字符串是否是字谜**

一个字符串的变位词是包含相同字符的另一个字符串，只是字符的顺序不同。比如“`abcd`”和“`dabc`”是互为变位词。

## 解决方法

我们创建一个计数数组，并将所有值初始化为 0。对于输入字符串中的每个字符，递增相应计数数组中的计数。

```
for (i = 0; str1[i] && str2[i]; i++) {
    count[str1[i]]++
    count[str2[I]]-— 
}
```

如果两个字符串长度不同。去除这种情况将使程序对于类似“aaca”和“aca”的字符串失败

```
if (str1[i] || str2[i])
    return false
```

查看 count 数组中是否有非零值。

```
for (i = 0; i < NO_OF_CHARS; i++)
    if (count[i])
        return false
```

如果都不匹配，则返回一个`true`值。这是我的完整解决方案。在这种情况下，时间复杂度为`O(n)`。

简单对吗？

我将在本文中更新与字符串相关的新挑战，请🔖它重新阅读并获得最新的问题和解决方案。

感谢阅读！😘

【JavaScript 用简单英语写的一句话:我们总是乐于帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)