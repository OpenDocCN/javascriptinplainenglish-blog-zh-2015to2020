# 用 TypeScript 构建游戏。绘制网格 2/5

> 原文：<https://javascript.plainenglish.io/building-a-game-with-typescript-drawing-grid-2-5-206555719490?source=collection_archive---------2----------------------->

教程[系列](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-in-action-with-typescript-d29b913858e)中的第三章讲述了如何用 TypeScript 和本地浏览器 API 从头开始构建游戏

![](img/11071dbab5771bf388f253c425416bd9.png)

[Background photo created by freepik](https://www.freepik.com/free-photos-vectors/background)

你好，欢迎回来！这是我们讨论如何用 TypeScript 和本地浏览器 API 构建一个简单的回合制游戏的系列文章！第三章致力于为这个游戏建立一个网格，其他章节可以在这里找到:

*   [简介](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-in-action-with-typescript-d29b913858e)
*   [第一章实体组件系统](https://medium.com/@gregsolo/entity-component-system-in-action-with-typescript-f498ca82a08e)
*   第二章。游戏循环([第一部分](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-with-typescript-game-loop-part-1-2-699919bb9b71)，[第二部分](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-in-action-with-typescript-game-loop-2-2-c0d57a8e5ec2))
*   第三章。绘制网格([第 1 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-1-5-aaf68797a0bb)，第 2 部分[，第 3 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-3-5-1fb94211c4aa)，[第 4 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-iii-drawing-grid-4-5-398af1dd638d)，[第 5 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-5-5-49454917b3af))
*   第四章。舰船([第一部分](https://medium.com/@gregsolo/building-a-game-with-typescript-colors-and-layers-337b0e4d71f)、[第二部分](https://medium.com/@gregsolo/building-a-game-with-typescript-team-and-fleet-f223d39e9248)、[第三部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-ship-14e6c19caa38)、[第四部分](https://gregsolo.medium.com/building-a-game-with-typescript-ship-and-locomotion-4f5969675993))
*   第五章输入系统([第一部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-1-3-46d0b3dd7662)、[第二部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-2-3-cd419e36027c)、[第三部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-3-3-8492552579f1))
*   第六章。寻路和移动([第一部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-17-introduction)、[第二部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-27-highlighting-locomotion-range)、[第三部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-37-graph-and-priority-queue)、[第四部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-47-pathfinder)、[第五部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-57-finding-the-path)、[第六部分](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-6-instant-locomotion)、[第七部分](https://blog.gregsolo.me/articles/pathfinding-and-movement-7-animated-locomotion))
*   第七章。玛奇纳州
*   第八章。攻击系统:生命和伤害
*   第九章。比赛的输赢
*   第十章敌人 AI

在本章的[第一部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-1-5-aaf68797a0bb)中，我们成功地*绘制了*网格。Canvas API 当时对我们帮助很大。然而，这个解决方案相当脏而且不灵活。我们简单地把所有的代码放到一个地方:`Game`实体。如果我们继续走这条路，很快，我们的游戏脚本会变得非常大，很难维护。而且，我们上次没有写测试，让自己没有任何保险。在这篇文章中，我们将改进我们的代码，使其更易于维护和扩展。

除了代码质量之外，我们还必须考虑其他一些事情。在这一点上，我们所做的就是*画了一个网格的静态图像*。它没有任何功能。事实上，现在网格唯一动态的部分是它的大小和颜色。但是正如我们将在以后的章节中看到的，网格不仅仅是一个图像。这是游戏中至关重要的一部分，我们需要它来满足我们日益增长的需求。网格应该成为一个实体。

> 随意切换到[库](https://github.com/soloschenko-grigoriy/gamedev-patterns-ts)的`drawing-grid-1`分支。它包含了前几篇文章的工作成果，是这篇文章的一个很好的起点。

# 目录

1.  网格实体
2.  用网格测试游戏实体
3.  引入节点实体
4.  编写我们的第一个组件
5.  测试节点实体
6.  结论

# 网格实体

![](img/33d3936d015f07e26e5c3b41d0563d86.png)

[Background vector created by pikisuperstar](https://www.freepik.com/free-photos-vectors/background')

我们将网格代码与游戏实体结合起来。虽然网格对游戏来说确实很重要，但这并不意味着它们必须生活在一起。是时候定义我们的第一个子实体了:

并为其添加一个桶文件:

我们现在可以将这个新实体作为孩子添加到游戏中。同样，让我们通过使用一个公共 getter 使`entities`成为私有字段来确保只有 game 拥有对其子节点的写访问权:

这就是我们要做的！游戏将唤醒并更新网格，就像我们在之前的帖子中设置的那样。

# 用网格测试游戏

![](img/4bf4f45fe5ea20f5d1fdc2cf70031b86.png)

[Background vector created by freepik](https://www.freepik.com/free-photos-vectors/background)

在我们继续之前，让我们更新一下游戏的测试。当我们建立`game.spec.ts`的时候，我们也创造了一堆虚假的实体:

他们为我们提供了很好的服务，但现在我们可以更进一步。我们确切地知道`Game`的子实体是什么:它是`Grid`。我们不再需要一个*假*孩子了。这种方法允许我们验证两者:

*   游戏和它的实体一起正常工作
*   它很适合*特别是*电网

我首先从规范中删除所有虚假的实体:

然后，我添加了一个新的测试来检查所有的孩子都被唤醒和更新。即使现在我们只有一个孩子，网格，将来我们可以在引入更多孩子时更新测试:

该测试的工作方式与*假实体*测试的工作方式相似。首先，我窥探一下`Grid`的`Awake`和`Update`方法。然后，我确保只有在分别执行了`game.Awake`和`game.Update`之后才调用它们:

> 注意，我没有访问网格实例的权限，因为它是由游戏创建和封装的。但是我可以依靠`Grid.prototype`来访问`Grid`方法，而不需要实际的实例。

如果您运行`npm start`，代码现在应该编译无误。如果运行`npm t`，所有测试都应通过

# 引入节点

我们为网格创建了一个专用实体。

将整个网格呈现为一个整体是一件简单的事情。不幸的是，这对我们来说还不够。出于不同的目的，我们必须跟踪网格中的各个矩形:突出显示它们并指示玩家可以将他们的船移动到哪里，存储附近节点的信息以使寻路成为可能，等等。

![](img/cf3f497a7f9ec41ee96ef86add80a09d.png)

[Business vector created by fullvector](https://www.freepik.com/free-photos-vectors/business)

为了使这种跟踪成为可能，我们迈出了第一步:我们为网格创建了一个实体。现在我们应该为每个`Grid`的`Node`创建一个实体:

让我们不要忘记桶文件:

像任何其他实体一样，它应该在游戏对象的层次结构中占据自己的位置。让它成为`Grid`的孩子感觉很自然。然后，我们可以在`Grid`中的所有节点上执行批量操作。

将孩子添加到`Grid`的过程与我们添加到`Game`的过程相同。我定义了私有字段和公共 getter:

网格应该唤醒并更新所有节点:

> 注意，我们应该叫超级。清醒()和超级。Update()允许抽象实体完成其默认工作，并唤醒和更新组件。我们还没有，但我们将在未来的章节中添加一些。

太好了，但是我们如何*画出*节点呢？我们应该在`Node`实体的 Awake 方法中添加绘图功能吗？我们*可以*，但这不是一个灵活的解决方案。

如果有几种方法可以画出一个`Node`呢？比方说，有些节点是圆形而不是矩形。或者也许我们根本不想画出一些节点，让它们看不见？为了实现这种灵活性，我们可以使用条件:

这样，我们在`Node` : `drawRectNode`，`drawCircleNode`中有了所有的绘图逻辑。

但是我们有一个更强大的工具:组件。我们可以定义不同的组件:`RectangleDraw`、`CircleDraw`，只附加必要的组件。或者，如果我们想跳过绘图，甚至不分配任何值。此外，我们可以实时地做到这一点！按照这种方法，我们将绘图逻辑从`Node`的核心逻辑中分离出来。

# 编写我们的第一个组件

![](img/70e401b84b6907c645f8191a6b35b402.png)

[Business photo created by d3images](https://www.freepik.com/free-photos-vectors/business)

我们将从小处着手，向为我们处理绘图逻辑的节点添加一个`Draw Component`。然而，我们保留选择的余地。如果我们需要不同的绘图组件，我们可以在不影响现有代码的情况下快速创建它们。

> 请注意，这种方法与可靠的“打开-关闭”原则配合得多么好:我们保持代码开放以供扩展。

组件是符合`IComponent`的任何类:

首先，我定义了一个实现`IComponent`的类。接口要求指定组件可以附加到哪个实体。在这种情况下，它是`Node`实体。我们还必须按照`IComponent`要求实施`Awake`和`Update`。

不要忘记添加和更新必要的桶文件:

> 注意，我为*组件*定义了一个专用文件夹，甚至为**绘制**组件定义了一个单独的文件夹。这个文件夹结构将贯穿整个系列。

留给我们的就是将`NodeDrawComponent`加到`Node`上:

# 测试节点

![](img/8d947ca4fc92f3738d0489b81abe3244.png)

[The watercolor vector created by milano83](https://www.freepik.com/free-photos-vectors/watercolor)

在我们结束这篇文章之前，让我添加一些测试。`NodeDrawComponent`现在是空的，所以还没有什么可测试的。但是我们可以测试`Node`。

`Node`现在的功能已经相当一般了。它所做的就是添加`NodeDrawComponent`。请允许我更一般化一点，为所有组件创建一个测试套件，尽管我们现在只有一个:

用`NodeDrawComponent`测试通信听起来应该很熟悉。我们不能访问组件的实例，但是我们可以窥探它的原型。然后，当实体被唤醒和更新时，我们使用 spy 来验证组件确实被唤醒和更新:

如果你用`npm start`开始你的代码，它应该编译没有错误。如果您用`npm t`运行测试，它们应该也能通过。

> 你可以在[库](https://github.com/soloschenko-grigoriy/gamedev-patterns-ts)的`drawing-grid-2`分支中找到这篇文章的完整源代码。

# 结论

厉害！我们做了很多，但在屏幕上什么也没有改变:我们仍然只能看到网格的旧“脏”图。但是我们完成了很多:我们学习了如何在不创建实例的情况下测试类的功能，我们引入了两个新的实体:`Grid`和`Node`，将它们从`Game`中分离出来，甚至建立了我们的第一个组件！

在[第三部](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-3-5-1fb94211c4aa)中，我们将与`NodeDrawComponent`紧密合作，一劳永逸地摆脱*肮脏的*抽签。我们还会遇到一个新朋友，他会在这方面帮助我们(并且在未来继续帮助我们！):`Vector2D`。

如果您有任何意见、建议、问题或任何其他反馈，请不要犹豫，给我发私信或在下面留下评论！感谢您的阅读，我们下次再见！

*这是系列教程“* ***用 TypeScript*** *”中的第三章。其他章节可点击此处:*

*   [简介](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-in-action-with-typescript-d29b913858e)
*   [第一章实体组件系统](https://medium.com/@gregsolo/entity-component-system-in-action-with-typescript-f498ca82a08e)
*   第二章。游戏循环([第一部](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-with-typescript-game-loop-part-1-2-699919bb9b71)，[第二部](https://medium.com/@gregsolo/gamedev-patterns-and-algorithms-in-action-with-typescript-game-loop-2-2-c0d57a8e5ec2))
*   第三章。绘制网格([第 1 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-1-5-aaf68797a0bb)，第 2 部分[，第 3 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-3-5-1fb94211c4aa)，[第 4 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-iii-drawing-grid-4-5-398af1dd638d)，[第 5 部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-grid-5-5-49454917b3af))
*   第四章。舰船([第一部分](https://medium.com/@gregsolo/building-a-game-with-typescript-colors-and-layers-337b0e4d71f)、[第二部分](https://medium.com/@gregsolo/building-a-game-with-typescript-team-and-fleet-f223d39e9248)、[第三部分](https://medium.com/@gregsolo/building-a-game-with-typescript-drawing-ship-14e6c19caa38)、[第四部分](https://gregsolo.medium.com/building-a-game-with-typescript-ship-and-locomotion-4f5969675993))
*   第五章输入系统([第一部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-1-3-46d0b3dd7662)、[第二部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-2-3-cd419e36027c)、[第三部分](https://gregsolo.medium.com/building-a-game-with-typescript-input-system-3-3-8492552579f1))
*   第六章。寻路和移动([部分 1](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-17-introduction) 、[部分 2](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-27-highlighting-locomotion-range) 、[部分 3](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-37-graph-and-priority-queue) 、[部分 4](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-47-pathfinder) 、[部分 5](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-57-finding-the-path) 、[部分 6](https://blog.gregsolo.me/articles/building-a-game-with-typescript-pathfinding-and-movement-6-instant-locomotion) 、[部分 7](https://blog.gregsolo.me/articles/pathfinding-and-movement-7-animated-locomotion) )
*   第七章。玛奇纳州
*   第八章。攻击系统:生命和伤害
*   第九章。比赛的输赢
*   第十章敌人 AI