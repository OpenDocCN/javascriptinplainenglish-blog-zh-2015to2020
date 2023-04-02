# JavaScript 模块的基础

> 原文：<https://javascript.plainenglish.io/fundamentals-of-javascript-modules-14654e9c9013?source=collection_archive---------7----------------------->

## 学习导入和导出语句的功能

![](img/924527ef75dd527da93c640b83fd3ce8.png)

Photo by Fabian Grohs on Unsplash

今天，我将解释 JavaScript 模块是如何工作的，以及导入和导出模块的不同方式。

一般来说，在早期阶段，JavaScript 是作为一个简单的脚本开发的，以获得更低的性能，但是它的用户日益增加，它急剧演变成一个更大的代码库。为了支持这种增长，它需要一种称为模块的额外机制。即有可能将代码分割成更小的和可重用的单元。

一个模块只不过是一个文件，我们可以通过导入和导出这样的指令从另一个模块导入。

**导入** -允许从其他模块导入。

**导出** -允许从当前模块外部访问的功能。

让我们用模块创建一个简单的应用程序。我们应用程序的基本结构是

```
index.html
scripts/
    index.js
    modules/
        user.js
```

首先让我们用用户类创建一个简单的用户模块。从这个应用程序，它会显示在输出屏幕上的用户。用户名可以从 user 类的实例中实例化。让我们深入研究不同的部分。

# 导入用户模块

import 语句允许我们从模块中导入绑定。除非我们定义 type="module "，否则不能在嵌入式脚本中使用 import 语句。有不同的导入方法，在我们的例子中，我们从指定的模块导入用户。

```
//file: scripts/index.js
import { User } from './modules/user.js'

const user = new User('Juan')

document.getElementById('user-name').innerText = user.name;
```

# 导出用户模块

为了导出到其他模块，我们需要使用导出语句。当创建一个模块来导出它们的值时，使用 export 语句，以便其他模块可以通过 import 语句使用它。在这个例子中，用户类是从模块中导出的。

```
// file: scripts/modules/user.js
export class User {
  constructor(name) {
    this.name = name;
  }
}
```

现在，它可以被导出，我们可以在任何有导入语句的模块中使用它。

# 导出模块的不同方式

导出功能有两种不同的方式。

1.  命名导出(每个模块零个或多个导出)
2.  默认导出(每个模块仅一个导出)

# 命名导出示例

```
// export features declared earlier
export { myFunction, myVariable }; 

// export individual features (can export var, let, const, function, class)
export let myVariable = Math.sqrt(2);
export function myFunction() { ... };
```

# 默认导出

```
// export feature declared earlier as default
export { myFunction as default };

// export individual features as default
export default function () { ... } 
export default class { .. }
```

命名导出用于导出多个值，但是在导入过程中，必须使用与相应对象相同的名称。默认导出可以用任何名称导入。

# 进口

我们已经知道了出口的概念，同样的概念也适用于进口。

# 导入默认导出

```
import something from 'mymodule'

console.log(something)
```

# 导入命名导出

```
import { var1, var2 } from 'mymodule'

console.log(var1)
console.log(var2)
```

# 重命名导入

```
import { var1 as myvar, var2 } from 'mymodule'

// Now myvar will be available instead of var1
console.log(myvar)
console.log(var2)
```

# 导入所有模块

```
import * as anyName from 'mymodule'

console.log(anyName.var1)
console.log(anyName.var2)
console.log(anyName.default)
```

# 动态导入

到目前为止，我们只处理了静态导入，我们把它们放在文件的顶部，它正在被导入。但是在动态导入中，只允许在需要的时候动态加载模块。

新的功能允许您将 import 作为一个函数调用，并将模块的路径作为一个参数传递。

```
import('./modules/myModule.js')
  .then((module) => {
    // Do something with the module.
  });
```

# 组合默认导出和命名导出

我们可以结合默认和命名的出口/进口。让我们看看例子

```
//file: mymodule.js
export const named = 'named export'

export function test() {
  console.log('exported function')
}

export default 'default export';
```

我们也可以在相同的场景中导入它们

```
//another file:
import anyName from './mymodule' // where anyName is the default export

// or both named exports
import { named, test } from './mymodule';

// or just one
import { named } from './mymodule';

// or all of them together
import anyName, { named, test } from './mymodule';
```

# 结论

我希望您喜欢今天的 JavaScript 模块，并从中学习了一些新知识。

感谢阅读！