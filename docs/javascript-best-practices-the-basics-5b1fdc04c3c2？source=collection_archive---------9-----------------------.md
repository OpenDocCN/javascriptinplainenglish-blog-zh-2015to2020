# JavaScript 最佳实践—基础

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-the-basics-5b1fdc04c3c2?source=collection_archive---------9----------------------->

![](img/49daa98fe4fd07d40737c02cd7fc2d69.png)

Photo by [twinkal solanki](https://unsplash.com/@twinkal_291?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 让代码易于理解

我们应该让我们的代码易于理解。

变量名和函数名应该是描述性的，这样我们才能理解。

例如，不要写:

```
x y z
```

我们写道:

```
numApples numOranges
```

我们还应该写出在任何地方都有意义的名字。

例如，不要写:

```
isOver21()
```

我们写道:

```
isDrinkingAge()
```

# 没有全局变量

我们不应该使用全局变量来共享代码。

这些名称很容易发生冲突，可能会被任何东西覆盖。

因此，与其写:

```
var current = null;
var labels = {
  foo:'home',
  bar:'articles',
  baz:'contact'
};
function init(){
};
function show(){
   current = 1;
};
function hide(){
   show();
};
```

我们写道:

```
module = function(){
   const current = null;
   const labels = {
     foo:'home',
     bar:'articles',
     baz:'contact'
   };
   const init = function(){
   };
   const  show = function(){
      current = 100;
   };
   const hide = function(){
      show();
   }
   return{ init, show, current }
}();
```

我们把一切都放在一个函数里。

# 坚持严格的编码风格

如果我们坚持一个严格的风格，那么当它被任何开发工作时，我们不会有任何问题。

代码也可以在任何环境下工作。

我们可以使用 ESLint 使样式自动一致。

# 根据需要进行评论，但不要过多

我们可以为代码中没有描述的东西添加注释。

此外，我们应该使用`/* */`作为注释，因为它们在任何地方都有效。

我们可以用`/* */`注释掉要调试的代码:

```
/*
   var init = function(){
   };
   var show = function(){
      current = 1;
   };
   var hide = function(){
      show();
   }
*/
```

# 避免与其他技术混合

如果我们正在设计样式，我们应该尽可能多地使用 CSS。

如果我们需要动态地设计样式，我们可以随意添加或删除类。

例如，不写:

```
const f = document.getElementById('mainform');
const inputs = f.getElementsByTagName('input');
for (let i = 0; i < inputs.length; i++) {
  if (inputs[i].className === 'required' && inputs.value === '') {
    inputs[i].style.borderColor = '#f00';
    inputs[i].style.borderStyle = 'solid';
    inputs[i].style.borderWidth = '1px';
  }
}
```

我们将样式移到 CSS 中:

```
.required {
  border: 1px solid #f00
}
```

在 JavaScript 中，我们写道:

```
const f = document.getElementById('mainform');
const inputs = f.getElementsByTagName('input');
for (let i = 0; i < inputs.length; i++) {
  if (inputs.value === '') {
    inputs[i].className = 'required';
  }
}
```

# 使用快捷符号

如果可以的话，我们应该使用更短的符号。

它们不影响可读性，并且节省打字。

例如，不要写:

```
let food = new Array();
food[0]= 'pork';
food[1]= 'lettuce';
food[2]= 'rice';
food[3]= 'beef';
```

我们写道:

```
let food = [
  'pork',
  'lettuce',
  'rice',
  'beef'
]
```

简短明了。

而不是写:

```
let x;
if (v){
  x = v;
} else {
  x = 100;
}
```

我们写道:

```
let x = v || 10;
```

而不是写:

```
let direction;
if(x > 200){
   direction = 1;
} else {
   direction = -1;
}
```

我们写道:

```
let direction = (x > 200) ? 1 : -1;
```

# 模块化

我们的代码应该模块化。

我们可以通过导出其他模块将使用的内容并导入所需的项目来将它们划分为模块。

例如，我们可以写:

`foo.js`

```
export const numApples = 1;
```

并且:

```
import { numApples } from './foo';
```

这样，我们保留私有项目和私有内容，并公开我们需要的内容。

此外，代码被分成更小的文件。

# 逐渐增强

我们不应该创建大量依赖于 JavaScript 的代码。

如果我们不需要操纵 DOM，那么我们就不应该这样做。

相反，我们尽可能多地使用 CSS 作为样式。

# 允许配置和转换

我们应该把共享代码放在一个地方，这样我们就可以很容易地修改它们。

例如，我们可以将它们都放在一个模块中:

```
export const classes = {
  current: 'current',
  scrollContainer: 'scroll'
}
export const IDs = {
  maincontainer: 'carousel'
}
export const labels = {
  previous: 'back',
  next: 'next',
  auto: 'play'
}
export const setting = {
  amount: 5,
  skin: 'blue',
  autoplay: false
}
export const init = function() {};
export const scroll = function() {};
export const highlight = function() {};
```

![](img/a274a64f9815be95313ee2dbd4fd95fc.png)

Photo by [alex lauzon](https://unsplash.com/@unknown_613?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该将代码保存在共享模块中，尽可能减少 JavaScript 操作。

此外，我们应该使代码易于理解和组织。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**