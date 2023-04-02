# 为什么您通常更喜欢 TypeScript 接口和自定义类型而不是类

> 原文：<https://javascript.plainenglish.io/why-you-should-generally-prefer-typescript-interfaces-and-custom-types-over-classes-d145814218ce?source=collection_archive---------3----------------------->

在任何新项目中，您首先要做的事情之一就是领域模型，它是所有应用程序的核心。无论您做什么，您都将创建和操作大量属于*域模型*的对象。

在这样做的时候，一个重要的设计决策是你是否将使用类、接口/自定义类型(或者它们的混合)来表示域模型。

拥有 Java/C#背景，当我在 2016 年第一次一头扎进 TypeScript 时，我最初的预感是大量使用类。然而四年后，我改变了主意。

在我目前的项目中，整个领域模型仅使用接口和类型/映射类型来定义，我真的不后悔这个选择。

现在不要曲解这篇文章；我并不是说应该完全禁止上课。我所争论的是这样一个事实，我们实际上可以在很多情况下避免使用类，而不会损失太多。

让我们从几个方面来讨论这个(真正有影响力的)选择的利弊。

# OOP(s)

我反对为您的领域模型使用类的第一点是，尝试并避免在类似 Java 的项目中经常出现的面向对象编程(OOP)的复杂性废话(至少我亲眼目睹过)。

具有很强的面向对象背景的人往往会时不时地对 OOP 结构着迷。他们喜欢创建复杂的继承链，覆盖方法，做各种稀奇古怪的事情。

这很好，而且真的很有益，但是通常这是糟糕设计的根源，并产生了许多可怕的怪物，同时吹嘘所有正在使用的好的 GoF 模式；好像这是某种清单“必须全部使用！”。

俗话说:宁要作文不要继承。如果你限制了类的使用，那么你就摆脱了一整类的设计问题。

另一方面，当不使用类时，你会错过的一件事是封装。保持内部/私有状态的想法很好，如果你完全摆脱了类，你可能会错过它；现在，EcmaScript 私有成员已经登陆，尽管使用了“#”而不是更好的名称。

# 序列化/反序列化

当您正在构建一个与一些后端公开 REST、GraphQL(或任何其他类型的 API)交互的多层应用程序时，您将执行常见的创建/检索/更新/删除(即 CRUD)操作。

例如，当您想要检索元素列表时，您必须调用后端 API 来获取数据。当你这样做的时候，你会得到一个你想要的数据的表示，通常是在 JSON 中:

接下来的问题是如何在客户端表示这些数据(例如，您将什么设置为？？?"在上面的例子中——*任何*都是大忌)。

如果您决定使用类来表示您的数据模型，那么您必须将接收到的 JSON 数据反序列化(即转换)成您的类的实例；那就是:

*   解析返回的原始 JSON 数据
*   将产生的对象转换成类实例，要么手工*新建*这些实例，要么使用其他方法

这是一件你可以用不同方式去做的事情。例如，你*可以*去“像兰博一样”和[手动转换使用自制的实用功能](https://stackoverflow.com/questions/29758765/json-to-typescript-class-instance)(请不要)。或者你可以使用更优雅/可靠的库，如 [cerialize](https://github.com/weichx/cerialize) 或 [class-transformer](https://github.com/typestack/class-transformer) 。

那些库非常相似；您通常可以修饰您的类/字段(如果您愿意),然后使用实用程序方法从类实例转换到 JSON，或者相反。

例如，对于 class-transformer，您可以这样做:

在这种情况下，plainToClass 实用函数将返回类实例。在我关于 TypeScript 的书[中，我用了相当多的篇幅来解释如何使用这些库。](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)

例如，NestJS 似乎偏爱类，因为它提供的多个特性依赖于类:[https://docs.nestjs.com/techniques/serialization](https://docs.nestjs.com/techniques/serialization)。

缺点是你必须花费时间和精力来装饰你的类，并且一直在考虑那些转换。那里的事情时不时会变得棘手。例如，如果您在数据模型中使用泛型，或者想要使用特定的构造函数、带有访问器的私有字段、继承和多态性，那么这些库往往需要特定的知识和大量的摆弄。

例如，假设您想要使用不可变的数据结构(向您致敬！)，那么您应该只允许通过构造函数初始化数据，而不允许任何修改；你如何处理像 cerialize 和 class-transformer 这样的库呢？提示:你没有。

我只想说:有无数的边缘案例和陷阱可以落入其中。如果你不幸仍然需要瞄准 ES5，那就更糟了…

相反，如果您只是简单地定义接口/自定义类型，那么您可以简单地解析收到的 JSON(例如，使用 JSON.parse(…))并继续您的生活；不需要转换，没有黑暗魔法。向后端发送数据时也是如此。

注意，无论您使用类还是简单的接口/类型，您最好确保您正确地验证了您接收的数据确实具有您期望的“形状”。为此，你可以利用诸如 [io-ts](https://github.com/gcanti/io-ts) 之类的库(我的书中也有涉及)，这使得验证/确保你收到的东西确实具有预期的*形状*变得简单。这真的很强大，我可能会写一篇文章来演示这一点。

当然，有利也有弊，因为让类只序列化您在客户端使用的数据结构的一部分会让您受益。不过，使用接口和自定义类型以类型安全的方式实现这一点也是可能的。例如，您可以使用[映射类型](https://mariusschulz.com/blog/mapped-types-in-typescript)轻松定义和使用派生类型来满足这种需求。

我想在这里传达的信息是，类比简单地解析/字符串化 JSON 数据/JavaScript 对象更“昂贵”和“复杂”(所有事情都是相对的)。

领域驱动设计(DDD) 狂热爱好者可能不喜欢我在这里看待事物的方式，但是我真的更喜欢避免不必要的转换。现代 Web 技术栈已经足够复杂，因为它不需要添加新的间接层。

# 确认

使用 TS 类的一个很酷的事情是，你可以使用像 [class-validator](https://github.com/typestack/class-validator) 这样的库来修饰东西。例如，NestJS [支持开箱即用](https://docs.nestjs.com/techniques/validation)。

就序列化而言，正如我上面所说的，我认为它比任何东西都更增加了复杂性。但是在验证的情况下，这可能很好，因为它允许将数据结构的定义和它们的验证规则放在一起。

另一方面，当处理接口和自定义类型时，您只能以某种形式将这些验证规则存储在其他地方。

首先，你可以依靠编译器来帮助你确保你没有误用类型。如果你在项目中输入东西时足够严格，编译器不会让你做傻事。仅此一点就几乎消除了问题。但是，这仅包括基本的验证，例如“这个字段是否存在，它的类型是否正确”。

对于更复杂的验证规则(例如，开始日期在结束日期之前)，您只能靠自己了。对于这些，你可以想象不同的方法。

第一个是函数式编程，您可以定义简单的验证例程，以便在需要时重用。这通常很有效，而且也不难实现。一个好处是它是类型安全的，易于测试和维护。为了以这种方式实现验证，您可以利用像 [fp-ts](https://github.com/gcanti/fp-ts) 这样的库。

另一种方法是仍然利用 class-validator，它提供了“ValidationSchema”类型，允许您在代码或 JSON 文件中定义[一个验证模式](https://github.com/typestack/class-validator#defining-validation-schema-without-decorators) (heh)，并在以后使用该模式通过“validate”方法来验证对象。

这里有一个来自他们文档的例子:

```
import {ValidationSchema} from "class-validator";
export let UserValidationSchema: ValidationSchema = { // using interface here is not required, its just for type-safety
    name: "myUserSchema", // this is required, and must be unique
    properties: {
        firstName: [{
            type: "minLength", // validation type. All validation types are listed in ValidationTypes class.
            constraints: [2]
        }, {
            type: "maxLength",
            constraints: [20]
        }],
        lastName: [{
            type: "minLength",
            constraints: [2]
        }, {
            type: "maxLength",
            constraints: [20]
        }],
        email: [{
            type: "isEmail"
        }]
    }
};...// Validation
import {validate} from "class-validator";
const user = { firstName: "Johny", secondName: "Cage", email: "johny@cage.com" };
validate("myUserSchema", user).then(errors => {
    if (errors.length > 0) {
        console.log("Validation failed: ", errors);
    } else {
        console.log("Validation succeed.");
    }
});
```

正如你在上面看到的，这非常简单，但是有一个*主要的*缺点:它与你的域模型接口/定制类型完全断开。对我来说，这确实是个问题，因为它很难维护。

最后一种方法，我倾向于选择本文中的方法:定义 JSON 模式，并使用 [ajv](https://github.com/epoberezkin/ajv) 、[JSON-schema-to-TypeScript](https://github.com/bcherny/json-schema-to-typescript)和类型保护将这些模式与 TypeScript 类型紧密联系起来。另一个我还没有尝试过的候选是 [ts-json-validator](https://github.com/ostrowr/ts-json-validator) 。

# 不变

甚至在开始使用 TypeScriptFor 之前，我就已经在我的设计中倾向于不变性。我不会详细介绍使用不可变对象和不可变数据结构的许多优点，但可以说有很多优点。

在 TypeScript 中，定义不可变类非常简单；您可以在字段上使用 [*readonly*](https://www.typescriptlang.org/docs/handbook/classes.html#readonly-modifier) 修饰符，只公开 get 访问器。readonly 修饰符很酷的一点是，您可以将它用于字段、索引签名等。

您还可以利用 ReadonlyArray、ReadonlyMap 等数据结构。最后两个是它们的非只读变体的超类型；提供方法的子集，并在试图修改数据时抛出错误。

尽管这些不是防弹的；举个例子，readonly，Readonly<t class="ae mk" href="https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#a-new-syntax-for-readonlyarray" rel="noopener ugc nofollow" target="_blank">Readonly array<T>，Readonly mapK，T >之类的都是**仅限编译时的限制**。没有什么可以阻止运行时修改的发生。虽然这通常不是一个重要的问题，但是也不经常需要运行时不变性..</t>

当使用接口和自定义类型并用相关类型标记对象时，您不能做那么多来保护您的数据免受突变。但是还是有解决办法的。

首先想到的是 [Object.freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) API，它可以*部分*冻结一个对象。我之所以强调这一点，部分是因为，不幸的是，它只冻结了顶级属性，使得嵌套的对象/属性可变。

有像 [ImmutableJS](https://immutable-js.github.io/immutable-js/) 这样的库，但是我真的不喜欢它，因为它们不适合与 TypeScript 一起使用。

但是，您可以使用和组合其他方法来在您的系统中推进不变性。首先，您可以使用 spread 语法来提取数据并创建新的&“隔离的”引用。

您可以使用的另一个很酷的特性是 TS 3.4 中引入的 [const assertions](https://mariusschulz.com/blog/const-assertions-in-literal-expressions-in-typescript) ，它有效地允许创建尽可能多的只读类型。

这里有一个例子:

```
const example = {
  name: 'JohnDoe',
  isHereToStay: true,
  mother: {
    name: 'JaneDoe',
  },
} as const;
```

以上基本上被转换成这种类型:

```
{
  readonly name: 'JohnDoe';
  readonly isHereToStay: true;
  readonly mother: {
    readonly name: 'JaneDoe';
  };
}
```

那么“as const”在 TypeScript 中做什么呢？

*   它将字符串之类的原语缩小到精确的文字类型(例如，“JohnDoe”字符串被更改为只能以“JohnDoe”作为值的类型)。
*   它将 readonly 修饰符应用于一切(包括嵌套的数据结构)
*   它将数组文字转换成只读元组
*   (可能还有更多我不知道的)

基本上，使用“as const”会创建只读类型，不允许对象发生任何变化。是不是很酷？

由于 TS 3.7 及其新增的对[递归类型别名](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#more-recursive-type-aliases)的支持，也可以定义和利用这样的类型来确保不变性:

```
type Immutable<T> = {
  readonly [K in keyof T]: Immutable<T[K]>;
};
```

这种类型允许有效地将整个数据结构(包括嵌套数据结构)设置为只读。

正如您所看到的，无论您使用的是类还是“简单的”接口/自定义类型，您都可以创建不可变的数据结构。

# 作文

我认为接口和自定义类型的一个主要优点是，您可以根据自己的需要轻松地创建类型的变体。

我将很快发表另一篇文章，用一些例子更详细地解释下面的想法，但我已经给了你它的要点。

假设您从领域模型的核心开始。您有一些实体类型，它们是您将保存在数据存储中的元素类型。这是您的系统处理的数据的规范表示。

首先，如果您创建一个公开 RESTful API 的后端，那么您将需要创建该数据模型的表示。根据您的 API 的能力，您可以决定只公开信息的一个子集(例如，不公开您的用户的密码散列、他们的出生日期等)。为此，您可以派生类型，我们称之为数据传输对象(dto)。

如果你只使用类，那么你可能会通过不相交的结构复制类型信息，这意味着无论何时一个类型改变，你都必须*考虑*对其他类型的影响，调整它们并处理转换/变换(我们现在已经走了一整圈)。

如果你是接口/自定义类型，那么你的数据类型更具可塑性。例如，您可以创建一个规范类型来表示将在您的系统中公开和重用的“基本”属性。然后，您可以通过使用内置的[映射类型](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)来创建变体，例如:

*   部分<t>:让 T 中的类型可选</t>
*   Readonly <t>:将 T 中的类型设为只读</t>
*   挑选<t>:从一个类型中只挑选你需要的</t>
*   排除<t>:从 T 中排除可赋给 U 的类型</t>
*   省略<t>:只省略你不需要的类型</t>
*   提取<t>:从 T 中提取可分配给 U 的类型</t>
*   不可空<t>:使 T 中的类型不可空(移除空值)</t>
*   必需的<t>:强制 T 中的类型</t>
*   …

您甚至可以将它们结合起来，创建自己的类型，从而将类型系统推向极限。

你也可以使用像 [utility-types](https://github.com/piotrwitek/utility-types) 这样的库，它提供了大量的实用类型(heh ),比如 Primitive、SetIntersection、SetDifference、NonUndefined 等等。

你可能会说，类也可以在 TypeScript 中用作接口，实现同样的事情，但是请[不要](https://www.stevefenton.co.uk/2017/11/typescript-using-classes-interfaces/)。

# 有角的

在角度项目中，我几乎只为我的角度组件控制器使用类，因为这是推荐的方法。

在这种情况下，类确实是有意义的。您可以从封装中受益，因为您可以将一些东西保留给控制器本身(不是所有东西都需要向模板公开)。

这允许很好地分离关注点，因为我们需要在控制器中放入逻辑，它必须放在某个地方。

所以不用担心，课程很好。

# 反应

在 React 项目中，我感觉现在的班级都是老派。它们在过去更强大，但是现在 React 有了钩子，类就不再有用了。

如果我开始一个新的 React 项目，我可能不会那么频繁地使用类。

# 某视频剪辑软件

AFAIK，Vue 的下一个主要版本将完全用 TypeScript 编写，也不会使用类。

# 结论

在这篇文章中，我试图让你相信你可能不需要在你的 TypeScript 项目中到处使用类。

虽然类很酷，很强大，并且提供了封装数据和逻辑(甚至在运行时保护数据)的方法，但是它们也受到许多问题(一些人为的，一些技术的)的影响，这些问题往往会增加代码库的复杂性，限制数据模型的表达能力。正如我所解释的，使用类创建规范模型的变体并不有趣，而映射类型却非常强大。

对于特定的需求，类仍然是非常相关和非常有用的，但是它们不一定要在系统的中心。

当然，您可以(应该)将类和接口/自定义类型结合起来，以便最大限度地利用 TypeScript 出色的类型系统。

今天到此为止！

PS:如果你想学习大量关于 TypeScript、Angular、React、Vue 和其他酷主题的其他酷东西，那么不要犹豫，去[拿一本我的书](https://www.amazon.com/Learn-TypeScript-Building-Applications-understanding-ebook/dp/B081FB89BL)并订阅[我的简讯](https://mailchi.mp/fb661753d54a/developassion-newsletter)！