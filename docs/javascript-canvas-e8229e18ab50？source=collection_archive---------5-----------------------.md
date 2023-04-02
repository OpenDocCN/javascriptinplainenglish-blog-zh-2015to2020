# 什么是 JavaScript 画布？

> 原文：<https://javascript.plainenglish.io/javascript-canvas-e8229e18ab50?source=collection_archive---------5----------------------->

他们说一张照片胜过千言万语。我同意！谁不喜欢在网站上看到漂亮的视觉效果呢？游戏公司总是试图用更好的图形使他们的产品更有吸引力。

JavaScript Canvas 是封装图片的单个 DOM 元素。它提供了一个编程接口，用于在节点占据的空间上绘制形状。画布和 SVG 图片之间的主要区别在于，在 SVG 中，形状的原始描述被保留，以便它们可以随时移动或调整大小。

画布图形可以被绘制到

<canvas>元素上。你可以给元素'宽度'和'高度'属性来决定它的像素大小。</canvas>

画布一画出形状就将它们转换成像素，并且不记得这些像素代表什么。在画布上移动形状的唯一方法是清除画布，然后用新位置的形状重新绘制。

如果你对阅读更多关于 Canvas 以及如何使用它感兴趣，我推荐你阅读这个 MDN 的教程:[https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)