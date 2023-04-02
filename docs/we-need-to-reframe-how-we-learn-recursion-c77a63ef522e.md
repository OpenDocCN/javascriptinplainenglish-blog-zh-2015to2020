# 软件开发人员应该这样学习递归

> 原文：<https://javascript.plainenglish.io/we-need-to-reframe-how-we-learn-recursion-c77a63ef522e?source=collection_archive---------3----------------------->

## 是时候我们围绕现实生活场景而不是优雅的数学方程来重新构建递归教育了

![](img/ba3c9df83086b878ff2b6729c90de18b.png)

对于程序员——尤其是自学成才的程序员——我们对“递归”世界的第一次介绍通常是以数学相关的东西的形式。当我们想到递归时，程序员经常会想起一些我们最喜欢的 F 字——不，不是那个 F 字，是这些:

**斐波那契**

```
function fibonacci(position) {
  if (position < 3) return 1;
  return fibonacci(position - 1) + fibonacci(position - 2);
}
```

**阶乘**

```
function factorial(num) {
  if (num === 1) return num;
  return num * factorial(num - 1);
}
```

fibonacci 和 factorial 函数的递归版本是您可能会看到的一些更漂亮的代码。这些简洁的代码片段仅仅依靠自身来完成工作。它们体现了递归的定义——一个调用自身的函数(调用自身是递归的一部分)。这真的不足为奇，就像当一个教程通过演示它如何与一些琐碎的东西交互来引入一个新的主题，如计数器或“待办事项”列表，fibonacci 和阶乘函数封装了递归的主题，很少或没有外部复杂性的干扰，一旦你开始引入其他代码进行交互，你就会发现这种干扰。就像计数器和“待办事项”列表，斐波那契和阶乘是微不足道的。

你可能听过一句话“所有迭代算法都可以递归表示”。换句话说，使用循环的函数可以被重写以使用它自己。现在，如果任何迭代算法都可以递归地编写，那么逆算法也一定如此。

*注意:迭代算法，或称函数，是一种利用循环来完成工作的算法。*

让我们回到递归斐波那契函数，把它写成迭代斐波那契函数:

```
function fibonacci(index = 1) {
  let sequence = [0, 1, 1];
  if (index < 3) return 1;
  for (let i = 2; i < index; i++) {
    sequence.push(sequence[sequence.length - 1] + sequence[sequence.length - 2]);
  }
  return sequence[index];
}
```

让我们把递归阶乘函数写成一个迭代阶乘函数:

```
function factorial(num) {
  if (num === 1) return 1; for (let i = num - 1; i >= 1; i--) {
    num = num * i;
  }
  return num;
}
```

我们的递归和迭代方法都得出了相同的结论。如果你给他们同样的输入，他们会产生同样的输出。甚至可以说，选择的道路也是一样的。唯一的区别是在到达所述输出的路径上使用的隐喻运输模式。

那么，如果我们能够迭代地编写递归函数，为什么我们还需要首先学习递归，它在编程中有什么用呢？

我们使用递归的主要原因是为了将算法简化成大多数人容易理解的术语。这里需要注意的是，递归的目的(或者说，递归的好处)是让我们的代码更容易阅读，更容易推理。然而，重要的是要意识到递归不是一种我们可以用来优化代码性能的机制——如果有的话，与迭代编写的等效函数相比，它会对性能产生不利影响。

如果你想记住一小段话，我会说:*递归函数优化了开发者的易读性。迭代函数优化了计算机的性能。*

令人沮丧的是，我以前读过的很多关于递归的文章和教程都倾向于谨慎，只关注递归的 fibonacci 或阶乘等函数。开发人员无法从递归编写这些代码中获得真正的优势。这就像给某人一个笑话的笑点，却没有任何通向笑点的步骤。

在递归 fibonacci 和阶乘函数的例子中，我们前面看到，如果我们将它们转换成迭代函数，代码本身实际上并没有那么糟糕。可以说，它和递归等价体一样易读易懂。当然，它可能没有同样的优雅，但如果它仍然可读，我会继续支持性能优化。

因此，许多第一次学习递归的人可能很难看到使用递归的任何真正用途，这似乎是合理的。也许他们只是将其视为某种过度工程化的抽象。那么递归是如何有用的呢？或者更好的是，我们如何使递归的研究变得有用？

当我们实际尝试编写类似真实生活场景的代码时，可以发现有用的递归。

我可以打赌，在任何地方(除了白板访谈之外),几乎没有开发人员被期望为他们的工作编写递归 fibonacci 或阶乘函数——如果他们是，他们可以谷歌一下，因为已经有一百万篇文章解释了它。

然而，没有足够的文章证明递归如何在现实生活中使用。我们需要更少的“递归介绍”文章，更多的关于递归如何帮助你解决你目前在工作中面临的编码问题的文章。

既然我们谈到了这个话题，让我们想象以下场景:你的老板给了你一个充满部门的数据结构，每个部门都包含在该部门工作的每个人的电子邮件地址。然而，每个部门有时由不同级别的子部门、对象和数组组成。你的老板希望你写一个函数，可以发送电子邮件到每个电子邮件地址。

数据结构如下:

```
const companyEmailAddresses = {
  finance: ["jill@companyx.com", "frank@companyx.com"],
  engineering: {
    qa: ["ahmed@companyx.com", "taylor@companyx.com"],
    development: ["cletus@companyx.com", "bojangles@companyx.com", "bibi@companyx.com"],
  },
  management: {
    directors: ["tanya@companyx.com", "odell@companyx.com", "amin@companyx.com"],
    projectLeaders: [
      "bobby@companyx.com",
      "jack@companyx.com",
      "harry@companyx.com",
      "oscar@companyx.com",
    ],
    hr: ["mo@companyx.com", "jag@companyx.com", "ilaria@companyx.com"],
  },
  sales: {
    business: {
      senior: ["larry@companyx.com", "sally@companyx.com"],
    },
    client: {
      senior: ["jola@companyx.com", "amit@companyx.com"],
      exec: ["asha@companyx.com", "megan@companyx.com"],
    },
  },
};
```

那么，你如何解决这个问题呢？

嗯，从我们可以看到，一个子部门似乎采取了对象的形式，而数组已被用来存储电子邮件地址。因此，我可以尝试编写某种迭代函数，循环遍历每个部门，并检查它是子部门(对象)还是电子邮件地址列表(数组)。如果是一个数组，我们就可以遍历数组，向每个电子邮件地址发送一封电子邮件。如果它是一个对象，我们可以创建另一个循环来处理这个子部分，使用我们前面使用的“检查它是一个对象还是一个数组”策略。据我们所知，我们的数据结构不超过两个子层。因此，再进行一次迭代应该可以满足所有级别的要求，并完成我们希望它完成的任务。

我们的最终代码可能类似于这样:

```
function sendEmail(emailAddress) {
  console.log(`sending email to ${emailAddress}`);
}function gatherEmailAddresses(departments) {
  let departmentKeys = Object.keys(departments);
  for (let i = 0; i < departmentKeys.length; i++) {
    if (Array.isArray(departments[departmentKeys[i]])) {
      departments[departmentKeys[i]].forEach((email) => sendEmail(email));
    } else {
      for (let dept in departments[departmentKeys[i]]) {
        if (Array.isArray(departments[departmentKeys[i]][dept])) {
          departments[departmentKeys[i]][dept].forEach((email) => sendEmail(email));
        } else {
          for (let subDept in departments[departmentKeys[i]][dept])
            if (Array.isArray(departments[departmentKeys[i]][dept][subDept])) {
              departments[departmentKeys[i]][dept][subDept].forEach((email) => sendEmail(email));
            }
        }
      }
    }
  }
}
```

我已经检查过了，也许我甚至为它写了一个小的单元测试。看起来很乱，但是很管用。这里有大量的嵌套循环，你可能会说这是非常低效的。有人在后台大喊*“大 O？更像大 OMG，我说的没错吧？”*

当然，我们还有其他的方法来解决这个问题，但是我们目前的方法是有效的。无论如何，它只是一个小功能，里面没有太多的电子邮件地址，我们可能不会经常使用它，所以没关系。在老板给我找另一个副业之前，让我们继续做更重要的项目吧！

五分钟后，您的老板回来了，他说即使以后在这个数据结构中添加了新的部门、子部门和电子邮件地址，他们也希望这个功能仍然有效。好的，这改变了事情，因为现在我需要考虑子子部门，子子子部门等等的可能性。

突然，我的迭代函数不再满足标准。

但是你瞧，一个递归解决方案出现了！

```
function sendEmail(emailAddress) {
  console.log(`sending email to ${emailAddress}`);
}function gatherEmailAddresses(departments) {
  let departmentKeys = Object.keys(departments);
  departmentKeys.forEach((dept) => {
    if (Array.isArray(departments[dept])) {
      return departments[dept].forEach((email) => sendEmail(email));
    }
    return gatherEmailAddresses(departments[dept]);
  });
}
```

好了，我们终于有了一个函数，它在更适合现实生活的场景中使用了递归。让我们解释一下它是如何工作的。

我们的函数遍历来自`companyEmailAddresses`数据结构的键，检查该键的值是否是一个数组，如果是，它向该数组中的每个值发送一封电子邮件。但是，如果前面提到的键的值不是一个数组，它将再次调用自己- `gatherEmailAddresses`(它会递归)。但是，它没有像第一次那样传入整个 companyEmailAddresses 对象，而是传入最初循环的子目录的对象节点。

与之前的迭代函数相比，这个函数为我们提供了两个好处:

1.  这符合我们老板强加的额外标准。只要它继续遵循相同的模式，该函数仍然可以处理添加到它的任何新对象或数组，而无需更改一行代码；
2.  更容易阅读。我们没有一堆嵌套的循环，我们的大脑必须尝试并保持跟踪。

*您还会注意到，我们的函数实际上包含迭代和递归元素。没有理由为什么一个算法必须是唯一的。有一些迭代是非常好的，比如 forEach 函数包含对自身的递归调用。*

让我们暂时回到我的第二点。只有当你理解递归如何在斐波那契、阶乘和任何其他临床编码试验的范围之外工作时，你才能更容易阅读《如何破解编码面试》一书/课程。所以让我们花点时间解释一下递归函数内部到底发生了什么。

我们的函数将一个值(一个对象)作为唯一的参数。我们传入的对象是整个`companyEmailAddresses`变量，这是一个怪异的怪物:

```
const companyEmailAddresses = {
  finance: ["jill@companyx.com", "frank@companyx.com"],
  engineering: {
    qa: ["ahmed@companyx.com", "taylor@companyx.com"],
    development: ["cletus@companyx.com", "bojangles@companyx.com", "bibi@companyx.com"],
  },
  management: {
    directors: ["tanya@companyx.com", "odell@companyx.com", "amin@companyx.com"],
    projectLeaders: [
      "bobby@companyx.com",
      "jack@companyx.com",
      "harry@companyx.com",
      "oscar@companyx.com",
    ],
    hr: ["mo@companyx.com", "jag@companyx.com", "ilaria@companyx.com"],
  },
  sales: {
    business: {
      senior: ["larry@companyx.com", "sally@companyx.com"],
    },
    client: {
      senior: ["jola@companyx.com", "amit@companyx.com"],
      exec: ["asha@companyx.com", "megan@companyx.com"],
    },
  },
};
```

我们做的第一件事是对它运行`Object.keys()`,返回每个部门的数组。类似这样的东西:`["finance", "engineering", "management", "sales"]`。然后我们通过我们的`companyEmailAddresses`进入`forEach`循环，使用我们的部门数组作为检查每个部门某些事情的方法。在我们的例子中，我们用它来检查每个节点的结构是否是一个数组，这是我们用`Array.isArray(departments[dept]`做的。如果它返回`true`，我们就继续遍历数组，对每个值应用`sendEmail()`函数。很简单，到目前为止我们还没有使用任何递归。也许你根本不需要读这一段，但如果你需要的话，至少这里有这个解释。无论如何，让我们进入好的部分——递归部分。

如果我们的`Array.isArray(departments[dept]`返回`false`，这意味着我们有一个对象，这意味着我们有一个子部门。在我们的迭代函数中，我们简单地重复了这个过程——也就是说，我们做了另一个循环——但是是为子分部做的。但是相反，对于我们的递归函数，我们再次调用`gatherEmailAddresses()`函数，看起来像以前一样传入相同的`companyEmailAddresses`对象。这里的关键区别是，我们不是从对象的根(整个对象)传入对象，而是从子目录的位置传入对象——子目录成为新的根。我们知道我们的`companyEmailAddresses`对象只是由许多包含另一个对象或数组的对象组成。因此，我们的函数以这样的方式编写，如果它命中一个数组，它知道它是一个节点(一个 lead)的结束，所以它会尝试并'发送电子邮件'。但是如果它撞上了一个物体，它需要继续向更深处行进。

有道理吗？

这里是我拼凑的一些类似图表的代码块，试图帮助进一步说明。

我们的整个对象有四个部门。第一个部门是一个数组。它不需要额外的遍历，因为我们已经直接找到了叶节点。这个部门将返回 true 到`Array.isArray()`。

```
 finance: [
            "jill@companyx.com",
            "frank@companyx.com"
           ],
```

第二个部门是一个对象。该部门将向`Array.isArray()`返回 false。它需要额外的遍历，所以我们调用`gatherEmailAddresses()`函数，传入`department[dept]`，这相当于`companyEmailAddresses["engineering"]`或者你下面看到的代码。

```
engineering: {
    qa: [
         "ahmed@companyx.com",
         "taylor@companyx.com"
        ],
    development: [
                  "cletus@companyx.com",
                  "bojangles@companyx.com",
                  "bibi@companyx.com"
                 ],
  },
```

因为我们现在已经再次递归调用了我们的函数，所以我们还没有传递到第三个部门，因为我们的递归函数需要首先处理，也就是说，我们仍然在处理第二个部门。解释这一点的技术方法是，我们的递归调用被添加到我们的调用堆栈中。

如果您还记得，我们的函数做的第一件事是对传入的对象执行`Object.keys()`。这给了我们`["qa", "development"]`。然后，我们循环遍历每个部门(或本例中的子部门)。我们检查`"qa"`部门/子部门是否是带有`Array.isArray()`的数组。是的，所以会返回 true，所以我们可以使用`sendEmail()`函数。“development”也会出现同样的情况，因为它也是一个数组。

```
 engineering: {
    qa: [
         "ahmed@companyx.com",
         "taylor@companyx.com"
        ],
    development: [
                  "cletus@companyx.com",
                  "bojangles@companyx.com",
                  "bibi@companyx.com"
                 ],
  },
```

第三个部门(三个子部门)与第二个部门(两个子部门)的结构相似，而第四个部门有两个子部门，但这两个子部门也包含两个子部门，因此我们的递归函数在每个部门内部再次发生。我跳过了展示这些代码块的分解，希望你能理解递归部分是如何工作的。

## 结论

好了，现在我们已经看到了递归的一个实际例子。希望这已经让你对递归有了更深的理解，并提供了另一种处理需要某种循环的问题的方法。

然而，我想再次强调，如果在不应该的时候使用递归，性能可能会受到影响。递归不应该突然成为你的首选工具，而不是迭代，你在可读性上获得的好处可能会在性能上失去。最终这取决于呈现给你的场景。你会在编程中遇到一些适合递归的问题，而其他的问题可能更适合迭代。在某些情况下，就像我们之前遇到的问题，两种方法都用一点可能是最好的。

## 你觉得这有用吗？

如果有，通过 [**订阅我的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw?sub_confirmation=true) **获取更多类似内容！**或者更好的是， [**订阅我的免费时事通讯**](https://sunilsandhu.substack.com/subscribe) 以便在我创建新内容时得到通知！

*原帖*[*sunilsandhu.com*](http://sunilsandhu.com)