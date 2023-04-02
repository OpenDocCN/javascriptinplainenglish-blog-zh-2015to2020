# 用 JavaScript 解释和实现哈希表

> 原文：<https://javascript.plainenglish.io/hash-tables-explained-and-implemented-in-javascript-9f173a0cd76b?source=collection_archive---------10----------------------->

编码面试最常用的概念之一，哈希表。许多现代编程语言都有内置的哈希表，这使得使用起来非常简单；例如在 Python 中，它是 Python 字典，在 JavaScript 中，它是 JavaScript 对象。

![](img/da5f79a792aee3b1cdf462203bad505f.png)

Photo by [Tom Fisk](https://www.pexels.com/@tomfisk?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/aerial-photography-of-container-van-lot-3063470/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## 什么是哈希表？

哈希表是一种数据结构，它允许我们存储成对的键和值，其中每个键都映射到一个值。在哈希表中，给定一个键就可以访问一个值。这些是非常强大和常见的数据结构，它经常在实践和编码面试中使用。

从根本上说，散列表是相当复杂的，但是为了编写面试代码，我们可以只关注简单的概念。

让我们看一个例子，

```
"act" => 1  
"sun" => 2            
"fly" => 3
```

这里，键“foo”、“bar”、“baz”映射到值 1、2 和 3。给定键“foo ”,我可以访问值 1，但是我们不能使用键“foo”来查找值 1。

哈希表中键值对的插入、搜索和删除平均都在常数时间 O(1)内运行。

我们使用一个散列函数*将一个键变成一个索引*以适合一个数组。当您搜索一个给定键的值时，您将使用这个散列函数将该键转换成一个索引，它映射到。如果你找到了索引，你就可以获取数组中的值，这就是常数时间索引。

## **将字符串转换成索引**

要将字符串转换成索引，我们可以将字符串中的每个元素映射到它的 ASCII 字符编码。每个元素有一个整数，计算所有整数的和来得到数字。

```
 ASCII         ASCII % length of the array - 1
"key1" => 1            134           139 % 3 = 1(1 is the reminder)
"key2" => 2            353           353 % 3 = 2
"key3" => 3            639           639 % 3 = 0indecies   0  1  2
let arr = [3, 1, 2]
```

如果我们将字符串`'key1'`中的每个字符映射到它的 ASCII 编码值，将所有整数相加得到 134。接下来，我们将使用模操作符`%`来获取数组位置。所以，它会看起来像这样，`134 % length of the array`答案会在 0 和数组的长度减 1 之间。假设数组的长度为 3，134 个模块的长度为 3 就等于 1。此时，我们可以在索引 1 处存储 1。对字符串“sun”和“fly”重复相同的公式，并将值 2 和 0 存储在数组中。最后，这给了我们每个键的相关指数。

## 碰撞

现在让我们假设我们的第二个字符串`'key2'`也映射到 1，这意味着我们现在有两个键映射到同一个值 1。这表明哈希表不仅仅是一个数组；这是一个数组，其中每个索引映射到一个潜在值的链表。这主要处理两个键被散列到同一个索引中并发生冲突的情况，这在哈希表中被称为*冲突*。

```
ASCII               
"key1" => 1            139 % 3 = 1 \
                                     > Collision
"key2" => 2            352 % 3 = 1 /
"key3" => 3            639 % 3 = 0
```

为了**解决冲突**，我们简单地将所有具有相同哈希值的键放在一个单独的链表中。在这种情况下，数组将有三个链表，索引 0 处的值 3，索引 1 处的值 1 & 2，`1 -> 2`。现在，您可能想知道如何知道 indices，3 中的两个值中的哪一个与“key1”和“key2”相关。要查找键，每个值还需要指向它们的键。所以，值 3 将指向“key3”，值 2 将指向“key2”，值 1 将同时指向值 2 和“key1”。这个过程清楚地表明了哪个键与哪个值相关。

哈希表支持常数时间、 **O(1)** *插入*、*删除*，以及*平均搜索*。最坏情况下，时间复杂度为 **O(n)** ，线性时间。如果你在一个场景中结束，你有一个长链表，或者说你有 15 个值，其中 12 个相互冲突，其余的是独立的，这个长度为 15 的链表，将是一个长度为 n 的链表； **O(n)** 最坏情况下。我们使用散列函数*来最小化冲突的数量*。事实上，这些散列函数是如此强大，以至于在行业中，我们经常忘记线性时间最坏情况。我们几乎总是认为散列表平均支持常量时间的插入、删除和搜索。

所以，它需要 **O(n)** 运行时间来初始化一个哈希表，其中有 n 个元素。空间复杂度取决于你存储的值，因为我们只存储值，不存储键；我们只指向内存中的键。因此，存储的空间复杂度为 **O(n)** 。

最后，我们来过一个简单的哈希表面试问题。

**问题:**给定一个字符串`str`，判断它是否拥有所有唯一字符。

输入 1:“XYZ”

输出 1:真

输入 2:“xyzz”

输出 2:错误

```
function uniqueString(string){ // create a hash table by assiging it to storage
        let storage = {} //start iteration at index 0
        let i = 0

        //enter the while loop
        while(i < string.length){
            //assign char to the value of the index
            let char = string[i]

            //check if the char is not in the storage hash
           if(!storage[char]){ // add it in the hash table and increment
                storage[char] = true
                i++
            }else{ //if char is already in storage return false
               return false
            }
        }
        //true, if you make your way out of the loop without any duplicate value   

        return true
    }
```

这个程序运行 **O(n)** 次，其中 n 是字符串的长度。空间复杂度是 **O(n)** ，n 是存储在存储中的值，我们的哈希表。

**以前的帖子:**

[在 React 中使用 PropTypes 进行类型检查](https://medium.com/javascript-in-plain-english/type-checking-using-proptypes-in-react-be8c46e7e704?source=your_stories_page-------------------------------------)

[12 种最常见的 JavaScript 数字方法](https://medium.com/swlh/12-most-common-javascript-number-methods-4dfeedb7f2af?source=your_stories_page-------------------------------------)

12 人必须知道下一次面试的数组方法——JavaScript

[数组:左旋转(JavaScript)](https://medium.com/@tanuka.das12/arrays-left-rotation-javascript-2befa87c4c87?source=your_stories_page-------------------------------------)