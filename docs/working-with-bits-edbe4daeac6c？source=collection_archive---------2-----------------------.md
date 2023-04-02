# 使用 JavaScript 中的位

> 原文：<https://javascript.plainenglish.io/working-with-bits-edbe4daeac6c?source=collection_archive---------2----------------------->

![](img/b1549b3b6b5de613f29a11e113a7afdf.png)

01000110 01101001 01110110 01100101 00100000 01110000 01100001 01100011 00101101 01100100 01101111 01110100 01110011 00100000 01100001 01110111 01100001 01111001 00100000 01100110 01110010 01101111 01101101 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101111 01101110 01110101 01110011 00100000 01100110 01110010 01110101 01101001 01110100 00100001

## 我最近做了一个叫做 [*翻转位*](https://www.hackerrank.com/challenges/flipping-bits/problem) 的黑客银行代码挑战，玩得很开心！

这是一个非常简单的问题，但是它引发了对[二进制数据](https://en.wikipedia.org/wiki/Binary_data#:~:text=Binary%20data%20is%20data%20whose,numeral%20system%20and%20Boolean%20algebra.)、[代数](https://en.wikipedia.org/wiki/Algebra)、[位运算](https://en.wikipedia.org/wiki/Bitwise_operation)的一些探索和实现。我在做这个挑战的时候在`JavaScript`中编程。因此，本文中的代码示例将出现在`JavaScript`和特定于该语言的按位运算符中。然而，按位运算大多是通用的。许多编程语言都以相同的方式使用按位运算符，或者主要在语法上有所不同，所以这些知识应该很容易转移。

二进制数据在计算机科学中被称为位，由两种状态之一表示:`0`和`1`。您可以将其视为开关:`OFF`和`ON`。在一个非常低的水平上，二进制数据负责几乎每一个甜蜜的计算机消遣你参与。比特类似于身体的细胞。我们用手指打字，用眼睛阅读，但是，在我们内心深处，细胞构成了产生我们身体过程、运动等等的引擎。

比特是有用的，强大的数据。大多数程序员都从事高层次的工作，尤其是框架和 API 所提供的出色的抽象和便利很容易被采用和构建。在底层，代码需要有坚实的基础，按位运算对于涉及硬件通信、网络协议、图形、加密和权限的现实场景非常有用。力量来自二进制的简单逻辑。在二进制级别，解释或改变一段数据就像打开一些 0 和 1 一样简单。

我们不会跟随蚁人到量子领域，不会太远……但是我们会深入到二进制领域来解决这个代码挑战，并学习按位运算。对于这个解决方案，我将演示一种编程方法，然后继续优化代码，直到函数缩减为 27 个字符长的单行代码。

# 翻转位

## 问题陈述:

给定`n`，一个 32 位无符号整数，翻转其二进制表示(`0 -> 1 | 1 -> 0`)的位，并将结果打印为无符号整数。例如:

```
n = 12345600000000000000011110001001000000*₂* = 123456*₁₀*
11111111111111100001110110111111*₂* = 4294843839*₁₀*result = 4294843839
```

**每个整数的基数表示其与十进制(基数* `*₁₀*`)或二进制(基数 `*₂*` *)的关系。十位数字，像十进制，或者两位，如二进制:* `0` *和* `1` *。*

**无符号整数[](https://www.ibm.com/support/knowledgecenter/ssw_aix_72/commprogramming/int_dat_typ.html)** T37 是一个 32 位数据，用于编码介于* `0` *至* `4294967295` *之间的非负整数。出于这个挑战的目的，只需考虑我们将使用 32 位整数，并且只使用正值，而不是负值。**

## *功能描述:*

*`flippingBits()`函数应该返回一个无符号的十进制整数。*

***输入参数:***

*   *`n`:整数*

***约束:***

*   *`0 ≤ n < 2²³`*

## *编程解决方案:*

*逻辑非常简单:将一个十进制整数转换成二进制，将它的 0 和 1 反相，将反相转换成十进制整数。让我们查看算法，然后解释每个过程。*

```
***function flippingBits(n) {** *// declare variables for binary, inverse binary, and decimal result* **let lowBin = ''
  let highBin= ''
  let result = 0***// convert input decimal to binary*
 ** while (n >= 1) {
    const rem = n % 2
    lowBin += rem
    rem === 1 ?
      n = Math.floor(n / 2) :
        n /= 2
  }***// adjust binary to 32 bits* **while (lowBin.length < 32) {
    lowBin += 0
  }***// reverse and invert each bit of binary* **for (let i = lowBin.length - 1; i >= 0; i--) {
    highBin += lowBin[i] === '0' ? '1' : '0'
  }***// convert binary to decimal output
 ***for (let i = 0; i < highBin.length; i++) {
    const expo = highBin.length - 1 - i
    result += highBin[i] * (2 ** expo)
  }** **return result
}***
```

1.  ***为二进制、逆二进制和十进制结果声明变量:***

```
 ***let lowBin = ‘’
 let highBin= ‘’
 let result = 0***
```

*二进制整数从左到右表示为*高阶*位到*低阶*位。变量`lowBin`将表示二进制的第一个状态，从低到高，这是因为我们是如何用代数方法转换输入的。变量`highBin`将被分配一个与`lowBin`相反的副本，因此我们有一个正确的*高阶*到*低阶*位结构。`lowBin`、`highBin`均为`string`型，使用方便。变量`result`将被赋予`highBin`所保存的值的十进制转换，并最终用作返回值。`result`属于类型号，在`0`实例化，因为它将被相加计算。*

*2.**将输入的十进制转换为二进制:***

*首先让我们讨论如何用代数方法来执行这个过程。十进制整数可以通过不断地用整数除以二进制数来转换成二进制数，直到数值达到二进制数，并将余数按位顺序保留。用`2`除任何一个数时，余数必须是`0`或`1`。考虑这个例子:*

```
*Divide 21 by 2 until the result is zero. The remainders of these divisions will be the bits used to construct the binary:21 / 2 = 10 (+) rem 1 *low order bit*
10 / 2 =  5 (+) rem 0
 5 / 2 =  2 (+) rem 1
 2 / 2 =  1 (+) rem 0
 1 / 2 =  0 (+) rem 1 *h**igh order bit*Organize the remainder bits into the binary integer: 10101*
```

*`1 / 2 = 0 (+) rem 1`有点令人困惑，但是考虑到我们正在处理整数，并且，一旦值减少到`1`，它就不能作为一个整数被均匀地划分，同时保持大于`0`，所以值本身——`1`——成为余数。*

```
**// perform the last operation when the value 1 is passed in* **while (n >= 1) {***// the* [*remainder operator*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Remainder) *(modulo operator) extracts 
// the remainder of n / 2 and assigns the value, the bit, to rem* **const rem = n % 2***// rem is appended to the lowBin string
// note the importance of maintaining the binary sequence 
// as a string, which would be more difficult numerically
// rem, a number, is coerced to a string and can be concatenated* **lowBin += rem***// a conditional operator is used to adjust the value of n
// for the next iteration
// if rem is equal to 1, n is an odd value, so n is assigned
// the value of n / 2 rounded down with Math.floor()
// otherwise, rem must be equal to 0, indicating an even number,
// and n is assigned the value of n / 2
// this always ensures a whole integer is passed into the loop* **rem === 1 ?
      n = Math.floor(n / 2) :
        n /= 2
  }***
```

*3.**调整二进制为 32 位:***

```
 ***while (lowBin.length < 32) {
    lowBin += 0
  }***
```

*前面构造二进制字符串的 while 循环只创建转换十进制输入所需的位数。其余的进程需要一个 32 位的值，所以我们简单地使用一个带有限制条件`32`的`while loop`集合来将零添加到现有的二进制字符串中。`lowBin += 0`起作用是因为`0`被强制变成了`string`，但你也可以直接使用`lowBin += ‘0’`。*

*请注意，由于前面的`while loop`执行的操作的线性顺序，二进制字符串在这一点上的顺序是相反的——从*低位*到*高位*——因此可以通过追加零从右侧填充二进制值，而不是通常的通过预先添加零从左侧填充二进制值的过程。*

*4.**反转二进制的每一位:***

```
 ***for (let i = lowBin.length - 1; i >= 0; i--) {
    highBin += lowBin[i] === '0' ? '1' : '0'
  }***
```

*我们使用一个递减的`for loop`来迭代由`lowBin`保存的二进制字符串——从结束到开始——`i = 31 -> i = 0`。在每一次迭代中，`highBin`被追加当前值的倒数(`lowBin[i]`，由条件运算符决定:如果`0`，返回`1`；如果`1`，返回`0`。这个过程在反转每个位的同时有效地反转二进制串。*

*5.**将二进制转换为十进制输出:***

*再一次，让我们讨论如何用代数方法来执行这个过程。二进制整数可以转换为十进制整数，方法是将每个数字乘以其自身的`2`次方，然后将结果求和，*即*每个位，一个`0`或`1`，可以乘以`2`的`n`次方，然后求和。`2`对应于基数`₂`，而`n`对应于位的位置，就像一个索引。例如:*

```
*Convert 1001₂ to a decimal integer.
__________________________________
|        bit | 1  | 0  | 0  | 1  |
|————————————|————|————|————|————|
| power of 2 | 2³ | 2² | 2¹ | 2⁰ |
——————————————————————————————————
Note that the powers of 2 reflect a zero index, and *descend* because
binary integers are sequenced from high order to low order.1001₂ = (1 * 2³) + (0 * 2²) + (0 * 2¹) + (1 * 2⁰) = 9₁₀This can be further dissected like this: 1 * (2 * 2 * 2) = 8
 + 0 * (2 * 2)     = 0
 + 0 * (2)         = 0
 + 1 * (1)         = 1  (*n⁰ is always equal to 1*)
—–––––––––––––––––––––—    
                     9* 
```

*通过使用`for loop`，我们可以方便地访问每一位的值和位置，或者索引。*

```
**// instead of setting the limit condition to highBin.length - 1
// we set it to highBin.length (32) so we can accommodate a
// decrementing bit index all the way to 0 for the variable expo
// highBin.length - 1 would end the formula at n * 2¹
// if the last evaluated bit was 0 (0 * 2⁰ = 0), the last addend 
// could be ignored, however, this condition is necessary because
// the last bit evaluated could be 1 (1 * 2⁰ = 1), impacting the
// result with a final addend of 1* **for (let i = 0; i < highBin.length; i++) {***// expo is assigned a number which represents each bit's position
// on the first iteration this would be: const expo = 32 - 1 - 0 = 31* **const expo = highBin.length - 1 - i***// if the first bit was 0 (highBin[i] = 0), then the first 
// addend of the conversion formula would be: 0 * 2³¹
// on each iteration the addend value is summed into result* **result += highBin[i] * (2 ** expo)
  }***
```

*6.**返回结果:***

*最后，但同样重要的是，`result`将由函数返回。*

```
*const n = 123456 flippingBits(n)// lowBin = 00000010010001111000000000000000
// flip bits and reverse string
// highBin = 11111111111111100001110110111111//=> 4294843839*
```

## *优化的编程解决方案:*

*我用优化这个词。我的意思当然是精简的，但是我还没有测试这三种解决方案的性能差异，所以我还不能报告真正的优化。然而，我粗略的评估是，是的，这个解决方案是优化的，即将推出的超紧凑的按位解决方案似乎相当快！*

*前面的解决方案看起来需要很多代码来翻转一些位，对吗？碰巧`JavaScript`和大多数其他语言都有一些内置的方法和函数可以精简我们的算法。*

```
***function flippingBits(n) {**
*// the* [Number.toString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toString) *method conveniently transforms a number
// into a string AND it can accept a radix argument to convert
// the number to a string of specific base, like base ₂ for binary***let lowBin = n.toString(2)
  let highBin = ''***//* Number.toString() *produces a binary string representation in
// the correct high order to low order sequence, so zeros are 
// padded from the left by prepending them*
  **while (lowBin.length < 32) {
    lowBin = 0 + lowBin
  }***// since the high order to low order sequence is already correct
// this for loop only needs to flip bits, not reverse the string*
  **for (let i = 0; i < lowBin.length; i++) {
    highBin += lowBin[i] === '0' ? '1' : '0'
  }***// the* parseInt() *function conveniently transforms a string to a* // number AND it can accept a second argument of a radix to 
// convert the string to a number of specific base*, like base ₂* **return parseInt(highBin, 2)
}***
```

*撇开评论不谈，这是一个比前一个更短、更简洁的函数。它利用了内置的能力，需要更少的内存，执行更少的迭代。有没有可能将这种算法压缩成一行性能代码，并获得相同的结果？请击鼓。*

*![](img/e7532aa1e974be1935e16c7ccc67ef9f.png)*

*Thank you, jazz messenger!*

## *逐位解决方案:*

```
***const flippingBits = n => ~n >>> 0***
```

*你能相信吗？我差点没！`ES6`胖箭头函数有一点帮助(没有双关语)，但是函数语句/返回值是如此简洁，令人惊叹。该函数接收一个参数，一个数字`n`，并返回逆二进制序列`n`的十进制整数表示。但是怎么做呢？*

1.  ***按位非运算符:***

*`Bitwise NOT`操作符被符号化为`~`,它反转操作数每个位的二进制值。这意味着在引擎盖下，一个数的二进制表示是颠倒的。每个`0`被翻转成一个`1`，每个`1`被翻转成一个`0`。`010100`会变成`101011`。不过，有一点需要注意。*

*产生的值是一个[有符号整数](https://www.ibm.com/support/knowledgecenter/ssw_aix_72/commprogramming/int_dat_typ.html)。有符号整数的*高位*位、*即*最左边的位也称为[符号位](https://en.wikipedia.org/wiki/Sign_bit#:~:text=In%20computer%20science%2C%20the%20sign,significant%20bit%22%20in%20some%20contexts.)，表示一个数的符号，*即*表示该数是正(`0`)还是负(`1`)。就我们的目的而言，我们的理解是符号位与所有其他位一起反转，将正输入转换为负输出。*

*我们的代码挑战要求我们返回一个正的、无符号的 32 位整数。在使用了`Bitwise NOT`操作符之后，我们就没有这些了。不用担心，还有另一个按位运算符可以解决问题。*

*2.**补零右移运算符:***

*`Zero-fill Right Shift operator`被符号化为`>>>`，并接受一个指定移动次数的参数。它将高位*位向右移动指定的位数。多余的位被丢弃到右边，而零(`0`)位被填充到左边。由于符号位变成零，所以值变成正的。**

*对于这个解决方案，指定的移位数是`0`，这实质上创建了这样一种情况:我们的二进制序列用零填充，直到它变成 32 位序列，并且`Zero-fill Right Shift operator`总是返回 32 位*无符号*整数。全垒打。*

*二进制数据和按位操作让人摸不着头脑，可能很少直接使用，但在我们最喜欢的机器的肚子里有一个能力的源泉，可以帮助构建伟大的软件和硬件。我鼓励你深入二进制领域，仔细阅读[位操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)，享受零和一的乐趣！*

*【github.com/dangrammer
T5[linked.com/in/danieljromans](https://github.com/dangrammer)
danromans.com*