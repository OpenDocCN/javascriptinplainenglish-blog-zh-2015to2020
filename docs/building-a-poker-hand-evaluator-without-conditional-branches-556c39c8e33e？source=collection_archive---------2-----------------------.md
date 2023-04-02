# 构建一个没有条件分支的扑克手评估器

> 原文：<https://javascript.plainenglish.io/building-a-poker-hand-evaluator-without-conditional-branches-556c39c8e33e?source=collection_archive---------2----------------------->

## JavaScript All In:一个没有条件分支的扑克手评估器

![](img/82f7d6d8d741461abab3a68ad2764647.png)

Photo by [Esteban Lopez](https://unsplash.com/@exxteban?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

建立一个开发团队需要一种方法来衡量候选人的天赋和技能。在一家公司的早期阶段，建立一个核心高层团队需要一种方法来淘汰骗子和印象深刻者。使用技术编码挑战是实现这一目标的一个途径。

我的首选测试包括我最喜欢的消遣之一，扑克。具体来说，结合一个扑克手评估机制。如果不将候选人引向某个特定的实现，这个挑战就是开放式的，允许他们展示自己的基础编程知识和经验水平。

我目睹了各种解决方案，从优雅的复制/粘贴代码到跨越数百行的可怕的冗余代码。有些完全是抄袭，没有注明原作者。但是贯穿这些提交的共同线索总是包含导致混乱或无关代码的条件分支。

我面临的一个挑战是编写一个不使用大型查找矩阵或任何条件分支的评估器，如 **if/else** 或 **switch** 块。也不鼓励使用链式三元运算符的代码。

这是我迎接挑战的旅程。

## 背景

在我早年编程的时候，在 Timex Sinclair ZX-81 上，在资源极其有限的情况下，跳入机器语言是代码优化的极限。使用按位运算符进行数学运算对我来说很神奇。这些按位运算或逻辑运算符的结果可以用作内联决策树，为寄存器分配一个或另一个值。

我继续研究 Commodore 64，然后是 Apple II，在 6502 芯片上用机器语言编码。随着计算机变得更强大、更快、内存和存储更便宜，程序员转向更高级的语言并开始懈怠，他们的代码变得更加冗长和草率。

每次迁移到另一个计算平台，我都坚持用更少的代码来执行相同的操作。我试图从每一行代码中挤出最多的内容，有时会因为混淆而损害可读性。

在这个练习中，我将从头到尾使用逻辑操作符，并将解释代码是如何完成多分支机制的工作的。除了逻辑操作符，使用对象映射也有助于这项工作。我更喜欢自顶向下读取和操作的代码，而不需要创建深度分支树。

我们开始工作吧。

## 规划

在写出代码之前，总是需要少量的计划。我经常回到纸笔这个屡试不爽的模拟工具上，快速勾画出我的想法。绘图纸再多也不嫌多；这是制定计划的最终手段。我想将一手牌的评估嵌入到二进制映射矩阵中，下一张牌的每个值都比前一张牌高一个二进制数值。

使用二进制映射表示法会导致每种卡片组合在加在一起时都有唯一的值。获胜者由“得分”最高的一手牌决定

```
A: 8192
K: 4096
Q: 2048
J: 1024
T:  512
9:  256
8:  128
7:   64
6:   32
5:   16
4:    8
3:    4
2:    2
A:    1
```

为什么王牌反反复复？在一手扑克中，a 通常被视为最高牌值，但在低 a 顺子中，a 是最低值；因此，它在地图上有双重位置。

扑克中有九个级别，其中皇家同花顺是排名第九的牌中最高的(在这种情况下，为了清楚起见，我们给它打 10 分)。现在让我们把它们列出来，指定一个排名号。

```
10: Royal Flush
 9: Straight Flush
 8: Quads (4 of a kind)
 7: Full House
 6: Flush
 5: Straight
 4: Trips (3 of a kind)
 3: Two Pairs
 2: Pair
 1: High Card
```

等级的对象映射将用于保存标志，在映射中最高等级排在最前面。默认情况下， **high_card** 等级设置为“真”。在评估这手牌的过程中，相应的匹配等级将被翻转为“真”遍历对象图并在第一个“真”值上停止将确定手的等级。

```
const ranks = {
  royal_flush: false,
  straight_flush: false,
  quads: false,
  full_house: false,
  flush: false,
  straight: false,
  trips: false,
  two_pairs: false,
  pair: false,
  high_card: true,
};
```

*在我们开始编码之前，我想先说明一下，这不是生产就绪的代码。它不包括错误处理或验证，并假设您熟悉扑克、排名牌，并对 JavaScript 语法有基本的了解。*

## 常数

现在计划已经完成，让我们分配一些将在整个练习中使用的常量。其中一些是不言自明的，对那些可能需要额外细节的评论。

```
**ACE_VALUE** equates to 8192, but let's use the *Math.pow* function to calculate that for us, which is 2^13, the 13th position from our map above (using a zero index).A way to visualize a hand in the binary matrix, is how the **STRAIGHT_LOW_ACE_INDICATOR** is constructed as a binary number. It represents a low-ace-straight with the Ace in the high position. The ace will effectively be moved to a lower position during the conversion, which we will see later in the exercise.Without counting the low Ace position, using a zero-index, the "Ten" card is in the 8th position, hence the **TEN_CARD_POSITION** constant. This will be used later to detect a Royal Flush.A high value is desired for a **RANK_BASE_VALUE** which will be added to the total score of the hand. It should be higher than the highest sum that can be calculated, which equates to a hand with quad Aces and a King: AAAAK. This value is 268,439,552\. I chose 10^9 (1 billion) since it gives a visual clue to its rank by looking at the most significant digit of the final total score.10,000,000,000 < Royal Flush
9,000,000,000 < Straight Flush < 10,000,000
8,000,000,000 < Quads < 7,000,000,000
...
1,000,000,000 < High Card < 2,000,000,000
```

## 甲板和洗牌

我们需要做一副牌，然后洗牌。参考我以前写的关于 JavaScript 洗牌的文章，我们可以使用。

[](https://medium.com/swlh/the-javascript-shuffle-62660df19a5d) [## Javascript 洗牌

### 编写洗牌算法的旅程。

medium.com](https://medium.com/swlh/the-javascript-shuffle-62660df19a5d) 

这副牌是一组从 0 到 51 的数字。下面显示的矩阵将牌分成四种花色:梅花、方块、红心和黑桃。使用改进的 Fisher-Yates 洗牌算法，对于这个练习应该足够了。来自 *buildDeck.js* 的第 5 行从未洗牌的牌池中随机选择一张牌，并将其从牌堆中拉出，推回牌堆顶部，然后继续处理其余的牌。

```
**Card Matrix
.  2  3  4  5  6  7  8  9  T  J  Q  K  A**
**C** 00 01 02 03 04 05 06 07 08 09 10 11 12
**D** 13 14 15 16 17 18 19 20 21 22 23 24 25
**H** 26 27 28 29 30 31 32 33 34 35 36 37 38
**S** 39 40 41 42 43 44 45 46 47 48 49 50 51
```

## 展示卡

对于一个人来说，仅使用数字来确定手里有哪些牌是困难的。让我们写一个函数来显示扑克术语中的牌；更易于阅读的格式。

我最喜欢的 JavaScript 数组函数是**。减少**。当遍历一组条目以返回单个值或构建一个列表时，使用**。reduce** 实际上减少了所需的代码量。在这种情况下，它将基于数字牌值矩阵构建一个可读扑克牌面的数组。最终，**。join** 将把它连接成一个字符串。

```
It takes an array like this [ 12, 16, 33, 7, 49 ] and displays it like A♣︎ 5♦︎ 9♥︎ 9♣︎ Q♠︎
```

## 给手排序

这段代码是练习的核心，也是编写起来最复杂、最有趣的。该函数假设基于上面定义的卡片矩阵的 5 张卡片的阵列是该函数的输入。如前所述，没有错误处理或输入验证。假设该函数的使用者正在传递有效信息。

```
Outline of the rankHand function:
• Build and count occurences of suits and values
• Calculate the total value of the cards
• Determine the rank of the hand
• Handle a Low-Ace Straight
• Build the description and human readable version
• Return an object with all properties
```

首先，此函数构建一个花色和值的数组映射，以计算这手牌中出现的次数。

```
A Straight Flush would have the resulting array maps:
suits: [ 0, 5, 0, 0 ]
values: [ 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0 ]A Full House would look like this:
suits: [ 1, 2, 2, 0 ]
values: [ 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 3, 0 ]
```

当遍历值数组时，执行求和。这段代码是通过在 reducer 方法中使用逻辑运算符来消除条件分支的一个例子。两次计算加在一起。

```
Line 11:
  ((val === 1 && Math.pow(2, index + 1)) || 0)
```

第一次计算将使用 **Math.pow(2，index + 1)** 基于其位置+ 1 的二进制扩展来添加卡的值。表达式 **val === 1** 需要为真才能继续数学表达式；否则，由于逻辑 OR (||)运算符，它将为零。

```
Line 12:
  ((val > 1 && Math.pow(2, index + 1) * ACE_VALUE * val) || 0))
```

第二个计算具有相同的结构，但仅适用于出现次数大于 1 的情况。这一次，二进制扩展乘以 **ACE_VALUE** 值，然后再乘以该值出现的次数。它将通过相应增加等级值来调整双人、三人、满屋和四人。

```
Line 15:
  const firstCardIndex = values.findIndex((index) => index === 1);
```

常数**first car index**有双重用途。它用于查找第一个非零计数的位置，然后用于确定这手牌是否被列为顺子。

第 16–34 行为每个等级设置了标志，该标志由每个等级属性上设置的条件和逻辑决定。最后执行同花顺和同花等级的逻辑，以利用已经为同花顺和同花设置的标志。每个表达式相当于一个布尔值。这个代码块通过使用对象映射和布尔表达式完全消除了条件分支的使用。

```
Line 37:
  Object.keys(ranks).every((key, index) => {
```

我使用了 *Array.every* 来在发现第一个真标志时使循环短路，也就是当**！【关键等级】**是假的。这消除了必须构建一个循环和初始化跟踪的额外变量。我使用这种方法来减少代码量，这也减少了可能引入的 bug 的数量。

第 42–44 行将 **RANK_BASE_VALUE** 乘以在前一个区块中找到的 **rankIndex，**的数值相加，然后减去 **ACE_VALUE** (如果是低牌顺子)。这里再次使用条件逻辑操作符，而不是使用任何分支逻辑。

## 扑克派对

既然我们已经完成了困难的部分，让我们玩得开心点。我们首先需要有一个方法来比较手找到赢家。

此函数将返回按排名顺序排序的手牌，第一手牌为“赢家”如果编写一个完整的扑克游戏，在分彩池的情况下确定多手牌的平等性。

## 得克萨斯扑克

玩扑克有很多种方式，有些变化可能有点复杂，但最受欢迎的是德州扑克。世界扑克锦标赛(WSOP)巡回赛基于无限注德州扑克规则。假设这种类型的扑克游戏正在进行，让我们编写一个函数来发牌。

我没有把棋盘上的发牌包在一个循环中，而是把它展开，让它非常清楚地显示出正在发生的事情。将牌发给玩家后，第一张 **deck.pop()** 是烧牌，然后连续三张 **board.push(deck.pop())** 是翻牌。接下来的 **deck.pop()** 又是一张烧卡，然后是转卡，最后对河卡重复同样的模式。

该函数还可以处理一种叫做奥马哈的扑克游戏的发牌，在这种游戏中，玩家收到四张底牌，而不是两张。他们需要使用其中两张底牌和三张牌来完成这手牌。

## 找到最好的牌

棋盘上有五张公共牌和两张底牌，我们需要一个程序来找出所有牌中最好的一手。在德州扑克中，你可以使用 1 张、2 张底牌或不使用底牌来凑成一手牌。

纸牌被放在棋盘的复制品上，然后被推到一排手牌中。包括作为可能的一手牌的棋盘，该函数产生 21 种可能的组合。我相信可能有一个更优雅的方式来完成这一点，但在这个时候，我想不起来了。所以现在，对循环的强力*就足够了。*

```
For Texas Hold 'Em rules:The board is 1 possible handThe first set of loops iterates through each index and replaces it with each hole card, producing 10 handsThe second set of loops replaces the board with 2 hole cards, resulting in another 10 hands
```

以下是寻找奥马哈规则最佳牌型的方法。

```
With Omaha, the loops iterates through the 4 hole cards and replaces 2 of the board cards, resulting in 60 different hands. This increases the difficulty of identifying combinations of hands, causing Omaha players to adopt a different betting strategy.
```

这些函数将在我们进行一轮游戏并决定获胜者时使用。

## 发牌

既然我们已经有了发牌和确定最佳手牌的方法，那么让我们一起来发牌吧。这个函数应该能够处理玩家数量、底牌和评估最佳手牌组合的变化。

```
The **findBestHand** parameter is assuming that a function will be passed in based on the required evaluation for the game being played. With this strategy, the dealRound function can support various rules for different variations of poker.
```

现在让我们用一个函数把它们放在一起，为一个特定类型的游戏发一轮扑克。第一次德州扑克。注意到 *holeCards* 属性被设置为 **2** 并且 *findBestHand* 属性被设置为 **findBestHandTexasHoldEm** 。

这是一轮奥马哈的玩法。注意 *holeCards* 属性被设置为 **4** 并且 *findBestHand* 属性被切换为 **findBestHandOmaha** 。

```
If you copy all of the gists code into a single file and then add either:dealTexasHoldEm(9)ordealOmaha(9)Passing in 9 equates to 9 players. If you execute the file, you should see something like this for Texas Hold 'Em:[
  {
    name: "Player 5",
    hole: "8♣︎ 3♣︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 6, 19],
      suits: [2, 1, 0, 2],
      values: [0, 0, 0, 0, 0, 0, 3, 0, 1, 1, 0, 0, 0],
      rankValue: 4003147264,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: true,
        two_pairs: false,
        pair: false,
        high_card: true,
      },
      rankDescription: "Trips",
      display: "J♠︎ T♣︎ 8♠︎ 8♣︎ 8♦︎",
    },
  },
  {
    name: "Player 1",
    hole: "J♥︎ 3♠︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 35, 19],
      suits: [1, 1, 1, 2],
      values: [0, 0, 0, 0, 0, 0, 2, 0, 1, 2, 0, 0, 0],
      rankValue: 3018874880,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: true,
        pair: false,
        high_card: true,
      },
      rankDescription: "Two Pairs",
      display: "J♠︎ T♣︎ 8♠︎ J♥︎ 8♦︎",
    },
  },
  {
    name: "Player 8",
    hole: "5♠︎ 7♣︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 42, 45, 3, 19],
      suits: [1, 1, 0, 3],
      values: [0, 0, 0, 2, 0, 0, 2, 0, 0, 1, 0, 0, 0],
      rankValue: 3002360320,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: true,
        pair: false,
        high_card: true,
      },
      rankDescription: "Two Pairs",
      display: "J♠︎ 5♠︎ 8♠︎ 5♣︎ 8♦︎",
    },
  },
  {
    name: "Player 2",
    hole: "A♣︎ 6♥︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 12, 19],
      suits: [2, 1, 0, 2],
      values: [0, 0, 0, 0, 0, 0, 2, 0, 1, 1, 0, 0, 1],
      rankValue: 2002106880,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ A♣︎ 8♦︎",
    },
  },
  {
    name: "Player 3",
    hole: "9♥︎ A♦︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 25, 19],
      suits: [1, 2, 0, 2],
      values: [0, 0, 0, 0, 0, 0, 2, 0, 1, 1, 0, 0, 1],
      rankValue: 2002106880,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ A♦︎ 8♦︎",
    },
  },
  {
    name: "Player 7",
    hole: "2♥︎ Q♣︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 10, 19],
      suits: [2, 1, 0, 2],
      values: [0, 0, 0, 0, 0, 0, 2, 0, 1, 1, 1, 0, 0],
      rankValue: 2002100736,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ Q♣︎ 8♦︎",
    },
  },
  {
    name: "Player 9",
    hole: "Q♦︎ 7♠︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 23, 19],
      suits: [1, 2, 0, 2],
      values: [0, 0, 0, 0, 0, 0, 2, 0, 1, 1, 1, 0, 0],
      rankValue: 2002100736,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ Q♦︎ 8♦︎",
    },
  },
  {
    name: "Player 4",
    hole: "2♦︎ 6♠︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 43, 19],
      suits: [1, 1, 0, 3],
      values: [0, 0, 0, 0, 1, 0, 2, 0, 1, 1, 0, 0, 0],
      rankValue: 2002098720,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ 6♠︎ 8♦︎",
    },
  },
  {
    name: "Player 6",
    hole: "3♦︎ 2♠︎",
    board: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    bestHand: {
      hand: [48, 8, 45, 3, 19],
      suits: [2, 1, 0, 2],
      values: [0, 0, 0, 1, 0, 0, 2, 0, 1, 1, 0, 0, 0],
      rankValue: 2002098704,
      ranks: {
        royal_flush: false,
        straight_flush: false,
        quads: false,
        full_house: false,
        flush: false,
        straight: false,
        trips: false,
        two_pairs: false,
        pair: true,
        high_card: true,
      },
      rankDescription: "Pair",
      display: "J♠︎ T♣︎ 8♠︎ 5♣︎ 8♦︎",
    },
  },
]
```

## 后续步骤

如果您能够通读发布的全部代码，您会注意到没有使用一个“If”、“switch”或三元语句。改变设计以利用对象映射和逻辑操作符消除了嵌套条件分支的混乱，反过来减少了行数，形成了一个干净的自顶向下的执行结构。

这里写的函数，可以作为一个跳板，开个好头，建立一个处理卡和等级的核心。要构建一个全功能的扑克游戏，需要额外的功能和机制:下注回合、底池管理、筹码管理、玩家管理和用户界面。如果在互联网上操作，那么一个成熟的扑克应用程序需要网络、API 和状态管理代码。

在编程中，解决一个问题总是有多种方法，有些是非传统的。在编写一行代码之前，花一些时间来计划一种方法。敢于超越你最初的倾向。

我持续的挑战是编写最少的代码，利用通过例子、文章和大量实验学到的策略。走出你的舒适区，你可能会发现一条捷径或新技术，并在此过程中成为一名更好的软件开发人员。