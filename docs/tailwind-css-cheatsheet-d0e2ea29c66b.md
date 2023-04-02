# 顺风 CSS 备忘单

> 原文：<https://javascript.plainenglish.io/tailwind-css-cheatsheet-d0e2ea29c66b?source=collection_archive---------8----------------------->

## 使用此备忘单的主顺风 CSS

![](img/23aa66a36b8ff7e9e5cc93fbe12be368.png)

Photo by [alleksana](https://www.pexels.com/@alleksana?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/green-vegetable-on-white-ceramic-plate-4451872/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

Tailwind CSS 允许我们使用各自的类来构建现代网站，而无需编写一个单独的原生 CSS。

就像 Bootstrap 等其他 CSS 库一样，Tailwind 也有响应类，我们只需要在应用程序中指定这些类。

这篇文章将帮助你快速开始使用 Tailwind 来制作漂亮的网站和用户界面。

Tailwind 很神奇，因为我们只需要指定各自的类，Tailwind CSS 会处理好一切。

最重要的是，我们不必担心响应度，因为所有这些都由不同设备上的顺风处理。

> 这是一个实用的 CSS 框架，包含 flex、pt-4、text-centre 和 rotate-90 等类，可以直接在你的标记中构建任何设计。来自 Tailwind.com。

在本文中，我们将看到 CSS 中最基本的 CSS 类及其相关属性。

## **背景**

**背景重复**

此属性设置背景图像的重复方式。
控制元素背景图像重复的工具。

```
 Class                  Properties bg-repeat               background-repeat: repeat; bg-no-repeat            background-repeat: no-repeat; 
```

**背景尺寸**
该属性定义了背景图片的尺寸。

```
 Class                Properties bg-auto             background-size: auto; bg-cover            background-size: cover; bg-contain          background-size: contain; 
```

**背景位置**
该属性设置每个背景图像的初始位置。

```
 Class                 Properties bg-bottom           background-position: bottom; bg-center           background-position: center; bg-left             background-position: left; 
```

**背景附件**
该属性设置背景图像是应该与页面的其余部分一起滚动还是保持固定。

```
 Class              Properties bg-fixed            background-attachment: fixed; bg-local            background-attachment: local; bg-scroll           background-attachment: scroll;
```

**字体大小**
字体大小是 CSS 属性，控制字符或文本的字体大小。

```
 Class             Properties     text-2xl       font-size: 1.5rem;
                    line-height: 2rem; text-3xl       font-size: 1.875rem;
                    line-height: 2.25rem; text-4xl       font-size: 2.25rem;
                    line-height: 2.5rem; text-5xl       font-size: 3rem;
                    line-height: 1; text-6xl       font-size: 3.75rem;
                    line-height: 1; text-7xl        font-size: 4.5rem;
                    line-height: 1; text-8xl        font-size: 6rem;
                    line-height: 1; text-9xl        font-size: 8rem;
                    line-height: 1; 
```

**字体粗细**
该属性定义文本中字符的粗细程度。

```
 Class                Properties font-thin            font-weight: 100; font-extralight      font-weight: 200; font-light           font-weight: 300; font-normal          font-weight: 400; font-medium          font-weight: 500; font-semibold        font-weight: 600; font-bold            font-weight: 700; font-extrabold       font-weight: 800; font-black           font-weight: 900; 
```

**字母间距**
该属性设置文本和字符之间的水平间距行为。

```
 Class                  Properties tracking-tighter       letter-spacing: -0.05em; tracking-tight         letter-spacing: -0.025em; tracking-normal        letter-spacing: 0em; tracking-wide          letter-spacing: 0.025em; tracking-wider         letter-spacing: 0.05em; tracking-widest        letter-spacing: 0.1em;
```

**行高**该属性用来定义一个元素的高度。

```
 Class                      Properties leading-3              line-height: .75rem; leading-4              line-height: 1rem; leading-5              line-height: 1.25rem; leading-6              line-height: 1.5rem; leading-7              line-height: 1.75rem; leading-8              line-height: 2rem; leading-9              line-height: 2.25rem; leading-10             line-height: 2.5rem; leading-none           line-height: 1; leading-tight          line-height: 1.25; leading-snug           line-height: 1.375; leading-normal         line-height: 1.5; leading-relaxed        line-height: 1.625; leading-loose          line-height: 2; 
```

**文本对齐**
该属性用于设置文本的水平对齐方式。

```
 Class                  Properties text-left             text-align: left; text-center           text-align: center; text-right            text-align: right; text-justify          text-align: justify; 
```

**文本装饰**
该属性用于设置或删除文本的装饰。

```
 Class              Properties underline             text-decoration: underline; line-through          text-decoration: line-through; no-underline          text-decoration: none; 
```

**边框半径**该属性定义了元素边角的半径。

```
 Class                 Properties rounded-none        border-radius: 0px; rounded-sm          border-radius: 0.125rem; rounded             border-radius: 0.25rem; 
```

**边框宽度**
该属性控制 am 元素边框的宽度。

```
 Class                  Properties border-0              border-width: 0px; border-2              border-width: 2px; border-4              border-width: 4px; border-8              border-width: 8px; border                border-width: 1px; 
```

**边框样式**
该属性设置不同元素的边框样式。

```
 Class                    Properties border-solid           border-style: solid; border-dashed          border-style: dashed; border-dotted          border-style: dotted; border-double          border-style: double; border-none            border-style: none;
```

**Height**
Height 属性允许我们设置元素的高度。在顺风中，我们使用指定的类来设置各自的高度。

```
 Class             Properties h-1               height: 0.25rem; h-2               height: 0.5rem; h-3               height: 0.75rem; h-4               height: 1rem; h-5               height: 1.25rem; h-6               height: 1.5rem; h-7               height: 1.75rem; h-8               height: 2rem; h-screen          height: 100vh;
```

**Display**
该属性设置并指定一个元素是否显示以及如何显示。

```
 Class                   Properties block                  display: block; inline-block           display: inline-block; inline                 display: inline; flex                   display: flex; inline-flex            display: inline-flex; 
```

**FLEX**
**Flex 方向**
该属性定义了 flexbox 项目在 flexbox 容器中的排序方式。

```
 Class                     Properties flex-row                 flex-direction: row; flex-row-reverse         flex-direction: row-reverse; flex-col                 flex-direction: column; flex-col-reverse         flex-direction: column-reverse; 
```

**Flex Wrap**

```
 Class                   Properties flex-wrap                 flex-wrap: wrap; flex-wrap-reverse         flex-wrap: wrap-reverse; flex-nowrap               flex-wrap: nowrap; 
```

**Flex**
该属性允许控制 Flex 项目如何增长和收缩。

```
 Class                     Properties flex-1                    flex: 1 1 0%; flex-auto                 flex: 1 1 auto; flex-initial              flex: 0 1 auto; flex-none                 flex: none;
```

**对齐内容**
该属性定义浏览器如何在 flex 元素之间和周围分配空间。

```
 Class                   Properties justify-start             justify-content: flex-start; justify-end               justify-content: flex-end; justify-center            justify-content: center; justify-between           justify-content: space-between; justify-around            justify-content: space-around; justify-evenly             justify-content: space-evenly; 
```

**对齐项目**
该属性指定灵活容器内项目的默认对齐方式。

```
 Class                      Properties items-start               align-items: flex-start; items-end                 align-items: flex-end; items-center              align-items: center; items-baseline            align-items: baseline; items-stretch             align-items: stretch; 
```

**可见性**
该属性控制元素的可见性。

```
 Class                   Properties visible                visibility: visible; invisible              visibility: hidden; 
```

在本文中，我们没有涉及到一些 tailwind CSS 属性和类。

请随时查看[顺风文档](https://tailwindcss.com/)，它非常棒，很容易理解。

**结论**
简单回顾一下，我们已经检查了一些基本的顺风类别。

顺风还加快了开发过程和工作流程。因为我们不用太担心造型，而是专注于开发部分。

感谢阅读这篇文章，如果你觉得有帮助，请不要忘记分享。

在这里看看我的一些 [**博客。**](https://amjohnphilip.medium.com/)