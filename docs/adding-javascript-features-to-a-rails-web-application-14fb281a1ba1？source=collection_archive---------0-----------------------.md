# 向 Rails web 应用程序添加 JavaScript 特性

> 原文：<https://javascript.plainenglish.io/adding-javascript-features-to-a-rails-web-application-14fb281a1ba1?source=collection_archive---------0----------------------->

## 在我为 Flatiron School 进行的第四个项目中，我的任务是向我为第三个项目构建的 Ruby on Rails 应用程序添加 JavaScript 动态特性。

![](img/f07906a6287c5122507f759958e659f3.png)

要求如下所列。

## 不要在此应用程序中使用“远程:真”

这让我有点不安，因为我不知道' **remote: true'** 做了什么。一个非常快速的谷歌搜索帮助了我，我找到了[这篇文章](https://medium.com/@AdamKing0126/ajax-and-rails-demystifying-remote-true-fe51ba2ce819)，它相当清楚地解释了它是如何工作的。这基本上是一个非常酷的 Rails *automagic* ,它:

*   防止默认动作，*即*提交表单或以*老式的*方式跟随链接。
*   发送一个 JavaScript 视图文件，而不是执行一段在 Rails 资产管道中编译的 JavaScript 代码。执行视图文件并执行 AJAX 调用。
*   控制器动作完成时会发送第二个视图文件，更新浏览器中的页面，无需重新加载。

所有这些都可以通过使用' **remote: true** '而不是编写完整的 AJAX 调用来轻松完成。以下示例来自主体上的[导轨](https://guides.rubyonrails.org/working_with_javascript_in_rails.html):

```
<%= link_to *"an article"*, **@article**, remote: true %>
```

因此，这不是用于这个特定的项目，但我期待着尝试它，因为，让我们诚实地说，它似乎真的很酷。

## 使用类或构造函数语法将来自 Rails 应用程序的 JSON 响应转换成 JavaScript 模型对象

到目前为止，在 Ruby 中，我们一直使用对象来帮助我们表示现实。这叫做面向对象。它关注两件事:**数据**存储为对象**属性**，以及**动作**存储为对象**方法**。

我们来想一个*自行车*物体。在一个处理自行车的应用程序中，你会期望有不止一个*自行车*对象。我们可以**硬编码**每个对象:

```
**const** b1 = { **bicycleType**: "Trekking", **size**: "M", **colour**: "Green" };**const** b2 = { **bicycleType**: "Road", **size**: "L", **colour**: "Red" };**const** b3 = { **bicycleType**: "Dutch", **size**: "S", **colour**: "Blue" };**const** b4 = { **bicycleType**: "Folding", **size**: "M", **colour**: "Yellow" };
```

真好。不过，这就是我们自己干的原则。由于每个*自行车*对象看起来具有相同的属性，我们需要的是一个**模板**。

```
**function** Bicycle(bicycleType, size, colour) {
   **this**.bicycleType = bicycleType;
   **this**.size = size;
   **this**.colour = colour;
};**const** b5 = **new** Bicycle("Mountain", "L", "Burgundy");
```

*自行车*函数是**构造器**函数的一个例子。每当一个对象被**实例化**时，它就会被调用，使用 *this* 关键字创建对象的属性，并为每个属性分配作为参数传入的值。

现在，如果我们想拥有一个所有实例化对象都拥有的函数，我们可以这样做:

```
**function** Bicycle(bicycleType, size, colour) {
   **this**.bicycleType = bicycleType;
   **this**.size = size;
   **this**.colour = colour;
   **this**.displayFeatures = **function**() {
      console.log(`${**this**.colour} ${**this**.bicycleType} bicycle
      available in size ${**this**.size}`);
   };
};
```

这太棒了！因为我们可以在创建的任何*自行车*对象上调用那个 *displayFeatures()* 方法。只有一个问题。使用该语法，每次实例化一个对象时，都会为该特定对象创建一个新的 *displayFeatures()* 方法，并将其存储在**内存**中。因此，如果我们创建 263 个自行车对象，我们也可以在内存中存储 263 个相同的 *displayFeatures()* 方法。事实上，它们做同样的事情，但是声明它们的*这个*是不同的，因为它对应于**新实例化的对象**。

为了避免这个问题，我们可以使用**原型**。如果我们将 *displayFeatures()* 方法的声明移到构造函数之外，那么我们将阻止它在每个对象实例化上的声明。但是，有必要让每个构造的对象访问该函数。这是通过访问自行车原型属性来完成的。

```
**function** Bicycle(bicycleType, size, colour) {
   **this**.bicycleType = bicycleType;
   **this**.size = size;
   **this**.colour = colour;
};Bicycle.prototype.displayFeatures = **function**() {
   console.log(`${**this**.colour} ${**this**.bicycleType} bicycle available
   in size ${**this**.size}`);
};
```

原型只是一个可以存储特定属性的 JavaScript 对象。使用构造函数创建的每个新的*自行车*对象都引用了自行车函数的原型对象上定义的属性。这样，无论用构造函数创建多少个自行车对象，内存中只有一个 *displayFeatures()* 方法，所有*自行车*对象都可以访问它。

对于 **ES6** ，语法中引入了*类*的概念。*类*函数创建一个**模板**来帮助我们在未来的某个时刻创建对象。*构造器*方法本质上与我们上面创建的*自行车*函数做同样的事情。*类方法*可以用同样的方式构建。

```
**class** Bicycle {
   constructor(bicycleType, size, colour) {
      **this**.bicycleType = bicycleType;
      **this**.size = size;
      **this**.colour = colour;
   }; addOwner(name) {
      **this**.owner = name;
   };
};**const** b6 = **new** Bicycle("Single Speed", "S", "Orange");b6.addOwner("Anne-Laure");
```

对于这个特定的任务，使用构造函数语法或类语法没有区别。注意，类语法的引入并不意味着 JavaScript 语言的本质:它仍然是一种基于原型的语言，而不是像 Ruby 一样的基于类的语言。

我选择使用**构造函数**语法来构建*城市*和*自行车*函数:

```
**function** City(object) {
  **this**.id = object.id;
  **this**.name = object.name;
  **this**.country = object.country;
};**function** Bicycle(object) {
  **this**.id = object.id;
  **this**.bicycle_type = object.bicycle_type;
  **this**.size = object.size;
  **this**.colour = object.colour;
  **this**.title = object.title;
  **this**.description = object.description;
  **this**.price = object.price;
  **this**.neighborhood = object.neighborhood;
  **this**.owner = object.owner;
  **this**.country = object.country;
  **this**.city = object.city;
}
```

使用 *fetch()* 和 jQuery，我从后端 API 获得了 JSON **响应**。然后我创建了用使用 JSON 数据作为参数的**构造函数**构建的 **JavaScript 对象模型**。

为了在用户的浏览器上显示我想要的东西，我需要能够格式化这些 JavaScript 对象模型。这就是**原型**上的方法出现的地方。为此，我构建了格式化程序来帮助我在用户浏览器上只显示我需要的信息，用于我的*自行车*和*城市*索引和显示页面。

## 通过 JavaScript 和活动模型序列化 JSON 后端呈现至少一个索引页面

首先，要做到这一点，我们需要一个后端 API **端点**。端点基本上是通信信道的一端。在我们的例子中，API 端点是在**控制器动作**中创建的，在那里我们需要获取 JSON 数据而不是 HTML。事实上，在这种情况下，我们不需要 HTML 带来的**格式**。下面是*城市*控制器的指标动作:

```
**def** index
   @cities = **City**.all
   @countries = Country.alphabetically
   respond_to **do** |f|
      f.html
      f.json { render json: @cities}
   **end**
**end**
```

这样，我们可以选择让**呈现**相应的**视图文件**中存在的常规 HTML，或者呈现 **JSON** 中只包含没有任何格式的数据。在某些情况下，我们可能只需要获取包含在 *@cities* 变量中的部分数据，并且能够从活动记录关系的灵活性中获益。这就是**序列化器**的用武之地。它们允许我们根据需要构造返回的数据。

```
**class** CitySerializer < ActiveModel::Serializer
   attributes :id, :name belongs_to :country
   has_many :neighborhoods
   has_many :bicycles
   has_many :bicycles, through: :neighborhoods
**end**
```

如果我们查看城市表的*模式*，我们会发现每行不仅仅只有一个 *id* 和一个*名称*。但是在我们的例子中，我们只想要 *id* 和*名称*。为此，我们仅将 *id* 和*名称*列为**属性**。其次，为了能够访问城市所属国家的数据，我们需要指定模型之间存在的**关系**，就像我们在*模型*文件中指定它们一样。

最后，一旦我们准备好了序列化程序和后端 API 端点，我们就可以使用 **JavaScript** 来呈现作为响应得到的数据。我将 JavaScript 代码分成了三个文件:

*   ***main.js*** 文件:我在其中编写了将被所有其他文件使用的通用 JavaScript
*   ***bicycle.js*** 文件:项目中与*自行车*对象相关的所有函数都在这里
*   ***city.js*** 文件:存放所有与*城市*对象相关的函数。

我在这个项目中创建了两个 JavaScript 渲染的索引。第一个事件发生在单击显示所有城市的按钮时。该按钮调用 *allCitiesClick()* 函数，该函数调用 *getCities()* 函数。通过使用 *fetch()* 和创建的 JavaScript 对象模型，我设法显示了应用程序**中存在的所有城市的列表，而无需重新加载页面**。第二个允许我在城市的显示页面上显示特定城市的所有自行车。

## 通过 JavaScript 和活动模型序列化 JSON 后端呈现至少一个显示页面

与我在没有重新加载页面的情况下显示的索引非常相似，通过使用 JavaScript，我做了同样的事情来显示关于每个列出的城市的所有信息。对于每个城市，它的名称是一个链接，加载并显示关于这个城市的详细信息，而不需要完全重新加载页面。

这是通过创建一个名为 *cityClick()* 的函数来完成的。这个函数使用 *fetch()* 来获取所需的数据，然后**将收到的响应转换为**JSON。然后，通过 *getCity()* 函数，我能够使用 JSON 创建 JavaScript 对象模型，并使用我在开始时构建的**构造函数**在原型上显示用**格式化程序**函数格式化的适当信息，这在第一节中已经解释过了。最后，使用 *getCityBicycles()* 函数和在 *City* 序列化程序中声明的**关系**，我可以显示城市中列出的自行车列表。

## 使用 JavaScript 通过 JSON 在页面上动态呈现至少一个序列化的' *has_many* '关系

如上所述，在用 JavaScript 显示的城市展示页面中，有一个在城市中列出的自行车的**列表**。在*城市*序列化器中，我列出了*城市*模型和其他模型之间的**关系**。在应用中，一个*城市* **有很多** *自行车*。这意味着在我们从 *fetch()* 得到的**响应**中，有一个**列表**是属于这个城市的自行车。

使用我在单击城市名称时收集的数据和序列化程序中列出的关系，接收到的**响应**转换成 JSON，给出了我正在寻找的列表。然后我只需要**遍历列表**，用**构造器**为每个*自行车*项目创建一个新的 JavaScript 模型对象，并在自行车**原型**上用**格式化器**方法以适合我的方式格式化它。

这个任务的关键是通过查询另一个模型(这里是*城市*)而不是*自行车*模型本身来获取事物的索引(这里是自行车)。我通过查询*城市*展览网址获得了自行车列表:

```
fetch(`/cities/${id}.json`)
   .then(response => response.json()) 
```

## 呈现一个表单，用于创建一个通过 JavaScript 和 JSON 动态提交并显示的资源，无需重新加载页面

对于这个任务，我们需要使用 JavaScript 动态提交和显示新创建的资源。在这个应用程序中，主要表单用于列出一辆新自行车。所以我很自然地选择了这个来完成任务。

要做的第一件事是确保在提交正确填写的表单时，防止了**默认行为**。这可以通过使用 *preventDefault()* 内置的 JavaScript 方法轻松完成。然后，我们需要轻松地操作表单中包含的数据。为此，我们使用了内置的 *serialize()* 方法。该方法接收可执行代码，将其转换成可以在任何地方使用的字符串，然后将其重新构建成可用的代码。

使用由 *serialize()* 方法返回的值和当前用户的 id，我们能够使用 **jQuery** *post* 方法创建一个新的对象，该对象在应用程序的数据库中被**持久化**。然后，有了可用的数据，这个过程与其他任务的过程非常相似。使用**构造器**函数创建一个新的 JavaScript 对象，并使用原型上的**格式化器**函数进行格式化。