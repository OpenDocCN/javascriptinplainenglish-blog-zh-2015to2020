# ES2018 的最佳功能—新的正则表达式功能

> 原文：<https://javascript.plainenglish.io/best-features-of-es2018-new-regex-features-a79d04416af3?source=collection_archive---------6----------------------->

![](img/d88176dc28caf6944de50aa3f397fbfc.png)

Photo by [SHTTEFAN](https://unsplash.com/@shttefan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# Regex 属性转义

Unicode 有属性，这些属性是描述它的元数据。

有像`Lowercase_Letter`描述小写字母、`White_space`描述空格等属性。

有几种类型的属性。

枚举属性是一个属性值，它的值很少，并且被命名。

`General_Category`是一个枚举属性。

关闭枚举属性 ios 集合值固定的枚举属性。

布尔属性是值为`true`或`false`的封闭枚举属性。

数值属性的值是实数。

字符串值属性是其值为字符串的属性。

目录属性是一个枚举属性，可以随着 Unicode 的变化而扩展。

杂项属性是其值不是上述任何一个值的属性。

有各种匹配属性和属性值。

属性是松散匹配的，所以`General_Category`被认为和`GeneralCategory`以及其他变体一样。

# 正则表达式的 Unicode 属性转义

我们可以使用`\p`字符对 Unicode 属性进行转义。

这必须与`/u`标志一起使用，以启用 Unicode 模式。

`\p`与没有 Unicode 模式的`p`相同。

例如，我们可以写:

```
const result = /^\p{White_Space}+$/u.test(' ')
```

而`result`将会是`true`。

这意味着`\p{White_Space}`匹配空白。

这比常规的正则表达式模式更具描述性。

我们也可以写:

```
const result = /^\p{Letter}+$/u.test('abc')
```

来匹配字母。

为了匹配希腊字母，我们写:

```
const result = /^\p{Script=Greek}+$/u.test('μ')
```

我们可以将拉丁字母与:

```
const result = /^\p{Script=Latin}+$/u.test('ç')
```

也可以匹配长代理字符:

```
const result = /^\p{Surrogate}+$/u.test('\u{D83D}')
```

# 回顾断言

后视断言是正则表达式中的一个构造，它指定当前位置的周围环境应该是什么样子。

例如，我们可以写:

```
const RE_DOLLAR_PREFIX = /(?<=\$)\d+/g;const result = '$123'.replace(RE_DOLLAR_PREFIX, '456');
```

我们有`(?<=\$)`组来寻找前面有美元符号的数字。

那么当我们调用`replace`替换号码时，我们只是替换号码。

我们用那个正则表达式搜索带有`$`和其后数字的东西。

所以`result`就是`'$456'`。

如果前缀应该是先前匹配的一部分，则这不起作用。

我们还可以添加一个`!`来添加一个负的后视断言。,

例如，我们可以写:

```
const RE_DOLLAR_PREFIX = /(?<!\$)baz/g;const result = '&baz'.replace(RE_DOLLAR_PREFIX, 'qux');
```

正则表达式查找前面没有美元符号的`baz`。

因此，如果我们像以前那样调用了`replace`,我们会返回`'&qux’`,因为`$`不在字符串中。

# `s`(`dotAll`)Regex 标志

dotAll dlag 是 regex 中的`.`标志的一个实例。

正则表达式中的`.`与行结束符不匹配。

所以如果我们有:

```
/^.$/.test('\n')
```

我们得到`false`。

为了匹配行结束符，我们必须:

```
/^[^]$/.test('\n')
```

匹配除无字符或以下字符以外的所有内容:

```
/^[\s\S]$/.test('\n')
```

匹配空格或非空格。

有各种各样的行结束字符。

它们包括:

*   U+000A 换行(左前)(`\n`)
*   U+000D 回车(CR) ( `\r`)
*   U+2028 行分隔符
*   U+2029 段落分隔符

对于 ES2018，我们可以使用`/s`标志来匹配以行结束符结尾的内容:

```
const result = /^.$/s.test('\n')
```

所以`result`应该是`true`。

长名字是`dotAll`。

所以我们可以写:

```
/./s.dotAll
```

并且返回`true`。

![](img/56b53ddbc317dc9f4877aa1e4368b3fb.png)

Photo by [Clyde RS](https://unsplash.com/@imclyde?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

Regex 属性转义、lookbehind 断言和 dotAll 标志是 JavaScript regexes 中新增加的功能，可以匹配各种特殊情况。

![](img/787be6c671be8d345dc786dad8729ce5.png)

[Subscribe to Decoded, our official YouTube channel!](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)