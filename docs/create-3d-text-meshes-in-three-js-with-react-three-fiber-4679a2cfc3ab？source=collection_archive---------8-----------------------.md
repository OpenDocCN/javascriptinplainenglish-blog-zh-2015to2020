# 使用反应三纤维在三个 js 中创建三维文本网格

> 原文：<https://javascript.plainenglish.io/create-3d-text-meshes-in-three-js-with-react-three-fiber-4679a2cfc3ab?source=collection_archive---------8----------------------->

![](img/f35656242da9f10d26ca58319dbdb19f.png)

Photo by [Ricardo Gomez Angel](https://unsplash.com/@ripato?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 利用您的反应技能来渲染 3D 文本

我第一次遇到 T2 的时候，就被吹走了。很难相信我看到的复杂的 3D 场景不是视频，而是直接在浏览器中渲染的！我想用它做点什么，什么都行，只是想了解这项技术是什么，以及它在 HTML、CSS 和 JS 生态系统中的位置。

虽然有一些探索性的尝试将 trial . js 结合到我网站的早期版本中，但是现在剩下的就是我网站的[主页](https://ilyameerovich.com)上旋转的文本网格。在这篇文章中，我将介绍如何使用 trial . js 和 reaction，使用惊人的[reaction-三纤维](https://github.com/pmndrs/react-three-fiber)库来复制它。

我们将充分利用您在下面的[和](https://codesandbox.io/s/recursing-sun-mc2ny)框中看到的内容，所以如果您要继续，您将需要一个空白的反应项目来使用代码。

# 背景

## 三. js

如果你以前没有遇到过 trial . js，它是一个用于创建 3D 场景的 API。它位于大多数现代浏览器附带的 WebGL API 之上，这反过来使得在浏览器中进行 GPU 加速的 3D 图形编程成为可能。

就其本身而言，3D 图形编程是一个巨大的领域，完全独立于网络开发，这里我们将只关注一个相对简单的例子，您可以稍后进行扩展。

您可以通过查看他们文档中的[教程](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene)来品尝 trial . js 的传统风味。如果你先通读一遍，这篇文章的其余部分会更有意义。

## 反应三纤维

因为我的网站是用 Gatsby 构建的，它使用了 React，我很高兴发现了`react-three-fiber`。这是来自`[react-spring](https://www.react-spring.io/)`的制造商保罗·亨舍尔的一个包，它允许我们以同样的基于组件的方式来声明性地构建三个. js 场景，就像我们习惯于使用 reaction 一样。

正如丹·阿布拉莫夫[对](https://overreacted.io/how-does-setstate-know-what-to-do/)的解释:

> *自从 React 0.14 中的包拆分后，React 包有意只公开定义组件的 API。React 的大部分实现都存在于“渲染器”中。*`*react-dom*`*`*react-dom/server*`*`*react-native*`*`*react-test-renderer*`*`*react-art*`*都是一些渲染器的例子(你也可以* [*构建自己的*](https://github.com/facebook/react/blob/master/packages/react-reconciler/README.md#practical-examples) *)*****

**`react-three-fiber`就是这样一个渲染器——取 JSX 并将其翻译成用于画布的 Three.js 代码。**

**为了给出一个快速的说明性比较，如果我们想给我们的场景添加一个网格，下面是我们如何使用传统的 Three.js API*来完成的:**

**下面是我们如何利用 react-three-fiber 实现同样的目标:**

***要使这些示例成为完全可用的 Three.js 场景，需要更多代码。**

# **布置我们的场景**

**要设置一个可以显示网格的场景，我们需要三样东西——一个场景、一个摄像机和一个渲染器。场景包含了我们所有的网格，灯光等。相机代表感知场景的视点，渲染器接收所有这些信息，然后“决定”在当前帧中应该渲染什么，并将所有内容渲染到画布上。**

**对于我们的示例，您首先需要安装依赖项:**

```
**npm i three react-three-fiber**
```

**我们将从`react-three-fiber`导入`Canvas`组件并渲染它。**

**`Canvas`组件需要一堆道具来调整渲染器、相机和其他东西，但是`react-three-fiber`为我们设置了[默认值](https://inspiring-wiles-b4ffe0.netlify.app/1-canvas)，所以我们什么都不用做！**

**我们的代码现在应该是这样的:**

# **添加我们的文本网格**

**Three.js 中的网格至少需要两个属性:**

1.  **定义其形状的几何图形，以及**
2.  **一种定义外观的材料——它的颜色，它如何与光线、阴影等相互作用。**

**我们不会为我们的文本网格从头开始雕刻每一个字形，那将会有很多工作要做。**

**相反，我们将使用[facetype . js](http://gero3.github.io/facetype.js/)——一种服务，它获取一个字体文件，并生成一个 JSON 文件，表示每个字体在 3D 空间中的坐标。我们将把这个 JSON 文件传递给 Three.js，它将为我们构造一个几何图形。**

**在我的例子中，我使用了 Roboto 字体文件。在获得 JSON 文件并将其导入到我的沙箱中之后，我们得到了:**

**现在我们应该在屏幕上看到一些东西！**

**不过，这里有很多新代码要解包，所以让我们来看看组件中发生了什么，然后调整我们的场景，使其看起来更像示例。**

**我们使用`THREE.FontLoader`类解析 JSON 文件并返回一个[字体](https://threejs.org/docs/#api/en/extras/core/Font)对象。**

**我们有一个选项对象，我们在其中指定:**

1.  **`font`是指这个新创建的字体对象**
2.  **我们的几何图形具有任意的大小 5，**
3.  **我们的几何体的高度是 1，这意味着它被挤出了 1。如果这个值是 0，我们的网格将完全 2D。增加这个值会增加我们字形的“深度”**

**在我们的`App`组件中，我们返回了之前的`Canvas`组件，但是现在它有了一个`mesh`子组件，这个子组件又有了一个几何体和一个材质。在`react-three-fiber`中，这些属性作为子属性被传入。**

**至于`attach`道具，文档并没有明确说明在什么情况下我们必须提供它。然而，[这个注释](https://github.com/pmndrs/react-three-fiber/blob/300b685c1a92a9a0d5029b7fddef3fe3a83d1adb/src/renderer.tsx#L370)和附带的代码表明这个属性告诉协调器将组件附加到它的父组件，不管`attach`的值是多少。我们不必对这些信息做太多，只要我们在创建几何图形和材料时包含这些信息，就应该没问题。**

**您还会注意到`textGeometry`组件带有一个`args`道具。在这里，我们首先传入我们想要制作成 3D 网格的字符串，然后传入我们上面创建的 options 对象。**

**确保你的网格的字符串是由 JSON 字体文件中的字形组成的，否则你会得到一个错误！**

# **调整位置**

**现在，你很可能无法一次看到整个网格。要对此进行调整，您可以:**

1.  **调整我们的`textOptions`对象的`size`属性**
2.  **调整摄像机的位置。您可以通过向画布组件传递一个`camera`道具来实现这一点，如下所示:**

```
**...
<Canvas
  camera={{ position: [0, 0, 0] }}
>
...**
```

**这里，`position`值分别对应于 x、y 和 z 坐标。**

**3.调整网格的位置。您可以通过将一个`position`道具传递给`mesh`组件来实现这一点，如下所示:**

```
**<mesh position={[0, 0, 0]}>**
```

**你可以猜猜这些值指的是什么:)**

# **鼓舞**

**所以现在我们可以看到我们的网格…但它没有做任何事情。为了让它看起来更有趣，我们可以给它一个简单的动画，让它绕轴旋转。**

**要做到这一点，我们应该做的第一件事是提取网格到它自己的组件，使它更容易工作。下面是我们的`TextMesh`组件的样子:**

**现在，为了制作动画，我们需要确保 WebGL 渲染器理解帧与帧之间需要改变的内容。因此，我们将导入并使用来自`react-three-fiber`的`useFrame`钩子来指定在渲染下一帧之前需要进行哪些更新。**

**所有这些都将发生在我们的`TextMesh`组件中，所以你可能会想，到底是什么东西将被动画化？网格？几何学？好的，我们将更新对我们网格的引用，我们将通过用另一个 React 钩子`useRef`创建一个引用来获得这个引用。**

**所以现在，我们更新的组件看起来像这样:**

**我们已经创建了一个 ref，并将其传递给我们的`mesh`组件。我们还有一个函数，在我们执行更新的每一帧都会被调用。**

**如果您在`useFrame`回调中添加以下内容，您应该会看到一些移动！**

```
**mesh.current.rotation.x += 0.01
mesh.current.rotation.y += 0.01
mesh.current.rotation.z += 0.01**
```

**确保还添加了**

```
**mesh.current.geometry.center**
```

**以确保网格绕轴旋转，而不是绕一个角旋转。**

# **添加纹理**

**我想做的最后一件事是给我们的网格添加纹理。这意味着，我们将采取一个 2D 的形象，并拉伸它周围的网格像皮肤。你也可以通过改变颜色和材质来添加不同的效果，这样它会对场景中的光线做出不同的反应。在这里，我们将仅限于改变纹理。**

**一般来说，我们将使用 Three.js 加载器加载纹理图像，就像我们对字体所做的那样。然后，我们将把纹理应用到材质上。**

## **加载纹理**

**网上有很多纹理图片。快速搜索会找到数百个。在这个例子中，我使用的是熔岩纹理，但是你可以使用任何你喜欢的纹理。**

**首先，我们将纹理导入到我们的项目中。然后我们将使用 Three.js 纹理加载器类将其转换为纹理对象。**

```
**import texture from 'path/to/texture.jpg'// ...const three_texture = new THREE.TextureLoader().load(texture)**
```

**根据您选择的纹理，下一步可能会有所不同。在下面的代码中，我们确保纹理在网格上水平和垂直重复缠绕。我们也指定我们想要纹理缠绕多少次。**

```
**three_texture.wrapS = THREE.RepeatWrapping
three_texture.wrapT = THREE.RepeatWrapping
three_texture.repeat.set(0.1, 0.1);**
```

**关于`Texture`属性的更多信息，[文档](https://threejs.org/docs/#api/en/textures/Texture)是一个很好的起点。这里还有引人入胜的解释[这里](https://threejsfundamentals.org/threejs/lessons/threejs-textures.html)和[这里](https://discoverthreejs.com/book/first-steps/textures-intro/)。**

# **应用纹理**

**使我们的纹理可见的最后一步是将它添加到我们的材质中。我们通过给我们的材质传递一个`args`道具来做到这一点，就像我们对几何体所做的一样。道具将包含一个带有`map`键的对象，其值就是我们的纹理。**

**我们的材料现在应该是这样的:**

```
**<meshBasicMaterial attach='material' args={{ map: three_texture }}/>**
```

**这样，我们现在应该可以在屏幕上看到成品了——一个应用了纹理的动画 3D 文本网格。最终的代码应该与此类似:**

# **结论**

**通过一起构建这个场景，我们引入了`react-three-fiber`和它的反应关系。我们已经学习了如何将文本网格添加到一个 Three.js 场景中并制作动画。我们稍微讨论了一下纹理。**

**你可能注意到了，上面的代码和开头提到的[沙箱](https://codesandbox.io/s/recursing-sun-mc2ny)不一样。与 Three.js *本身*相比，React 的差异更大，因此鼓励读者扩展这个示例，使其成为自己的示例。**

**并不是每个项目都需要 3D 图形来增添趣味。但是通过使用 Three.js，您能够实现更大的(几乎是无限的！)文本样式的效果范围比单独使用 CSS 要大。**

# **资源**

**Three.js 文档:[https://three js . org/docs/index . html # manual/en/introduction/Creating-a-scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene)**

**反应三纤维文档:[https://inspiring-wiles-b4ffe0.netlify.app/](https://inspiring-wiles-b4ffe0.netlify.app/)**

**丹·阿布拉莫夫谈 React 建筑:[https://overreacted.io/how-does-setstate-know-what-to-do/](https://overreacted.io/how-does-setstate-know-what-to-do/)**

**facetype . js:[http://gero3.github.io/facetype.js/](http://gero3.github.io/facetype.js/)**

**三个 JS 基础(纹理):[https://three JS Fundamentals . org/three JS/lessons/three JS-Textures . html](https://threejsfundamentals.org/threejs/lessons/threejs-textures.html)**

**Discover Three.js(纹理):[https://discover three js . com/book/first-steps/Textures-intro/](https://discoverthreejs.com/book/first-steps/textures-intro/)**