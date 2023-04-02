# 使用纯 JavaScript 的可排序数据表

> 原文：<https://javascript.plainenglish.io/sortable-data-tables-using-pure-javascript-3a34f7913478?source=collection_archive---------0----------------------->

写技术文章已经有一段时间了，这就是为什么我决定写一篇新的，但有一个小的不同。我决定用英语写这篇文章。我们开始吧:

![](img/16e452ebfe947a79af6631d3f4a7ac8b.png)

Credit: [https://unsplash.com/@_imkiran](https://unsplash.com/@_imkiran)

在我目前的公司中有一个功能请求，产品团队请求一个表组件，当单击列的标题时，它应该以升序或降序的方式对列进行排序。在这篇文章的最后，你会看到工作的概念证明。在代码质量方面可能有很多需要改进的地方，但不要忘记，这只是一个概念验证。我期待着您对该准则的回应。

我有一个有固定标题的表格。为了简单起见，我有一个只包含四个元素的对象数组。这个表应该按照这些对象的一些属性排序，而不是全部。我必须在不使用任何视图库的情况下构建这个表，只使用普通的 JavaScript！

我通常注意采用单一责任原则来编写我的函数。这种方式为编写和调试提供了极大的便利。

```
<table id="squadTable" class="table table-dark table-striped">
        <thead>
            <tr>
                <th **data-filter-value="no"** class="active">No</th>
                <th>Name</th>
                <th>Position</th>
                <th **data-filter-value="age"**>Age</th>
                <th **data-filter-value="match"**>Match</th>
                <th **data-filter-value="time"**>Min</th>
                <th **data-filter-value="goal"**>Goal</th>
                <th **data-filter-value="assist"**>Ast</th>
                <th **data-filter-value="yellowCard"**>YC</th>
                <th **data-filter-value="doubleYellowCard"**>DYC</th>
                <th **data-filter-value="redCard"**>RC</th>
            </tr>
        </thead>
        <tbody></tbody>
</table>
```

我在帖子最后分享了 Codepen 链接。为了更好的外观和感觉，我实现了 Bootstrap。这就是上面代码中一些泛型类的用途。

我还通过在代码中加粗突出显示了可排序的列。正如我前面提到的，这是一个静态 POC，所以我静态地编写了数据过滤器值。你可以将它们转换成模板文字，并使用数组等生成。

```
th {
  &[data-filter-value] {
            cursor: pointer;
        }  &.active {
            color: red;
        }
}
```

我没有任何 CSS 的顾虑，只是因为我在使用 Bootstrap。我只想区分可排序列和活动列。我在上面添加了 Scss 代码。

以下是我的对象数据数组:

```
const data = [{
            "id": "1233213",
            "no": 17,
            "fullName": "Murat Doğan",
            "position": "CMD",
            "age": 29,
            "match": 12,
            "time": 1080,
            "goal": 12,
            "assist": 4,
            "yellowCard": 3,
            "doubleYellowCard": 0,
            "redCard": 2
        }, {
            "id": "41233213",
            "no": 58,
            "fullName": "Volkan Demirel",
            "position": "GK",
            "age": 28,
            "match": 34,
            "time": 3400,
            "goal": 1,
            "assist": 17,
            "yellowCard": 8,
            "doubleYellowCard": 3,
            "redCard": 5
        }, {
            "id": "51233213",
            "no": 18,
            "fullName": "Soldado",
            "position": "ST",
            "age": 33,
            "match": 5,
            "time": 120,
            "goal": 1,
            "assist": 2,
            "yellowCard": 5,
            "doubleYellowCard": 1,
            "redCard": 4
        }, {
            "id": "61233213",
            "no": 27,
            "fullName": "Hasan Ali Kaldırım",
            "position": "DL",
            "age": 18,
            "match": 2,
            "time": 12,
            "goal": 0,
            "assist": 1,
            "yellowCard": 3,
            "doubleYellowCard": 4,
            "redCard": 5
        }];
```

我不使用 id 属性，但是在将来有一个用于任何目的的唯一键是很好的。

这些是用于排序的全局变量:

```
let currentFilter = "no",
            prevFilter = "",
            orderAsc = true;
```

*   表格的默认排序值将是玩家的号码(否)。我把它设置为当前过滤器。
*   上一个过滤器值保留先前选择的过滤器。保存这些数据以处理应用于同一列的点击事件是很重要的。将其从升序转换为降序，反之亦然。
*   订单升序值保持当前的订单类型。关于点击动作，它将被改变。

```
const toggleOrder = () => {
            if (currentFilter === prevFilter) {
                orderAsc = !orderAsc; // Get same value and assign its opposite.
            } else { 
             orderAsc = true;
            }
        }
```

正如我之前提到的，如果用户反复点击同一个列名，就会改变排序的方向。这里我需要一个 else 情况，因为如果我将一列按降序排序，其他列需要按升序排序。有点重新排序的意思:)

```
const sortTable = (array, sortKey) => {
           return array.sort((a, b) => {
                let x = a[sortKey],
                    y = b[sortKey];                return orderAsc ? x - y : y - x; // [ternary operator](https://medium.com/@muratdogan/javascript-hap-yaz%C4%B1s%C4%B1-ternary-operator-2788782189fb) 
            });
        }
```

这个函数将两个参数作为一个数组和一个排序变量。我在这里使用 Array.sort 方法来捕捉前一个和后一个变量之间的差异。如果想比较字符串值，可以使用大于号或小于号。如果你想按升序排序，它使用 x-y 或 y-x 来表示相反的情况。

这里涉及到渲染表:

```
const renderTable = tableData => {
            return (`${tableData.map(item => {
                return (
                    `<tr>
                        <td>${item.no}</td>
                        <td>${item.fullName}</td>
                        <td>${item.position}</td>
                        <td>${item.age}</td>
                        <td>${item.match}</td>
                        <td>${item.time}</td>
                        <td>${item.goal}</td>
                        <td>${item.assist}</td>
                        <td>${item.yellowCard}</td>
                        <td>${item.doubleYellowCard}</td>
                        <td>${item.redCard}</td>
                    </tr>`
                )
            }).join('')}`);
        }
```

这个函数的唯一目的是呈现关于给定数据的 tr 元素。它使用映射遍历数组，并为每个数组返回字符串输出。如果你对链尾的 join 方法很好奇，我来解释一下！Map 函数返回一个新数组，join 方法通过在每个数组项的末尾添加一个空字符串来帮助我们**连接**这些数组项。来源: [Mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

```
const appendTable = (table, destination) => {
            document.querySelector(destination).innerHTML = table;
        }
```

这个函数的唯一目的是将给定的元素追加到给定的目的地。这个函数的命名可能更通用，但正如我之前提到的，这只是一个概念验证。您应该将其重命名为 append etc。

```
const handleSortClick = () => {
            const filters = document.querySelectorAll('#squadTable th');
            Array.prototype.forEach.call(filters, filter => {
                filter.addEventListener("click", () => {
                    if (!filter.dataset.filterValue) return false;
                    Array.prototype.forEach.call(filters, filter => {
                        filter.classList.remove('active');
                    });
                    filter.classList.add('active');
                    currentFilter = filter.dataset.filterValue;
                    toggleOrder();
                    init();
                });
            })
        }
```

最好把这一节写得更详细些。我硬编码了列标题的选择器，但是您可以将选择器作为参数传递。没那么难。

我正在查询所有 th 元素，并使用 forEach 进行循环。如果你对我为什么不使用 Array.forEach 感到好奇，让我解释一下:*过滤器*变量是一个列表，但它只是一个*节点列表。如果你正在处理节点列表，你不能使用数组的每一个方法。它提供了浏览器兼容性和对 forEach 的访问。*

我正在检查标题是否有排序属性。如果不是，我在函数的最开始返回 false，并且不执行整个函数。

我要提一下这个令人愉快的特性:**数据集**。如果想要访问 dom 元素的所有数据属性，可以使用 dataset 而不是 getAttribute(…)列出所有属性。你应该注意这种用法。数据属性是 kemap-case，但是您需要使用 camelCase 来访问它们。

我正在做一些课堂作业和触发切换功能。我也在触发初始化函数。

```
const init = () => {
      let newTableData = sortTable(data, currentFilter),
          tableOutput = renderTable(newTableData);appendTable(tableOutput, '#squadTable tbody');
        prevFilter = currentFilter;
        }
```

我在这里做所有的短工。让我们来看看是怎么回事:

*   我正在使用当前的过滤器参数重新格式化我的球员数据。当我在其他函数中更改当前的过滤器时，它可以帮助我轻松地进行排序。
*   为分配给*表输出*的表创建字符串 Html 输出。
*   我把这个输出附加到我声明的地方。
*   我可以在整个过程结束时将当前过滤器指定为前一个过滤器。

```
init();
handleSortClick();
```

我在其他函数中调用 init 函数，所以我不在 init 函数中调用 handleSortClick。这可以防止将事件重新绑定到选择器。你可以分享更好的方法来改进这些代码。

创建这个表只需要大约 70 行代码。如果我们保持数据对象的分离。这取决于模板，但我认为它很短，很容易实现。

 [## CodePen Embed -纯 JavaScript 可排序表

### const data = [{ "id": "1233213 "，" no": 17，" full name ":" Murat doan "，" position": "CMD "，" age": 29，" dateOfBirth"…

codepen.io](https://codepen.io/muratdogan17/embed/qozQVr?height=353&theme-id=0&default-tab=js,result) 

感谢阅读这篇文章直到这一行。我期待着你的回应，使这个代码更好。