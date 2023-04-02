# 在 TypeScript 中编写 React 函数组件和钩子

> 原文：<https://javascript.plainenglish.io/write-react-functional-components-and-hooks-with-typescript-cd1539e2bc2a?source=collection_archive---------5----------------------->

TypeScript 是一种静态类型语言，它符合 JavaScript。静态类型的代码更容易维护，也更难破解。

![](img/219d518f0bf84f1a50608c172a16231e.png)

本文假设您已经对 React 和 TypeScript 有所了解。如果你不是，那么不要担心，我会保护你。在阅读本文之前，请浏览下面的参考资料并尝试一些例子:

*   [React 教程](https://reactjs.org/tutorial/tutorial.html)
*   [15 分钟学会打字稿](https://medium.com/front-end-weekly/learn-typescript-in-15-minutes-bf921cf355f5)

我不会浪费你的时间，现在就直接进入正题。

# 设置项目👨‍💻️

在本文中，我们不讨论如何从零开始用 TypeScript 建立一个 React 项目。开始时保持事情简单总是好的。我已经为您准备了 GitHub 模板，无需任何设置即可开始使用。你可以决定分叉回购或只是使用模板开始一个新的项目。[这个回购已经配备了更漂亮，eslint，TypeScript 和 React 设置。和 CRA 一起成长。在这里找到它。](https://github.com/theBstar/react-typescript-starter)如果你想[做自己的 React +打字稿设置](https://reactjs.org/docs/static-type-checking.html#typescript)，请查看官方 React 文档。

# TypeScript 泛型刷新程序

泛型是构建可重用软件的工具。这给了我们不将片段与类型紧密耦合的灵活性。允许我们在维护类型安全的同时重用不同类型的代码。

在 TypeScript 中，泛型就像是可以动态插入的类型的占位符。举个例子就能把事情说清楚。

我们正在实现一个名为 SimpleStack 的小函数，用于保存一些项目的列表。如果我们在没有泛型的情况下实现它，我们的 SimpleArray 声明将只能保存一种类型的值。

```
type SimpleStackType = {
  data: Array<string>;
  push: (item: string) => number;
  pop: () => string;
};
```

上面的声明只允许我们创建一个字符串堆栈。如果我们想用上面的方法生成一个数字堆栈，我们需要声明一个新的类型，用数字类型替换字符串类型。现在让我们看看通用示例。

```
type SimpleStackType<T> = {
  data: Array<T>;
  push: (item: T) => number;
  pop: () => T;
};
```

上面代码中的 t 是一个类型的占位符。我们可以将任何东西传递给 T，SimpleStackType 将作为 T 的类型工作。

```
let stringStack: SimpleStackType<string>; // this like the first example with strict type of string
// but we are not limitted.let numberStack: SimpleStackType<number>;type Person = {
  name: string;
  age: number;
};let personStack: SimpleStackType<Person>;
```

好的，如果你不熟悉泛型，你可能已经问过了，我们不能使用类型**和**吗？与**任何**有何不同，有何优势？

我再举个例子来回答一下。所以任何类型的堆栈

```
type AnyStack = {
  data: Array<any>;
  push: (item: any) => number;
  pop: () => any:
};
```

实现 AnyStack 允许我们在堆栈中存储任何东西，但是它不提供类型安全。此外，我们不确定数据项上有哪些可用的方法或属性。

```
let anyStack: AnyStack;
// initialized anyStack hereanyStack.data.map(item => { 
  console.log('Item is of type any'); 
  console.log('We are not certain about the type, that means we can access any properties or methods on the item. But if we don' want to break it, we should check the everything before calling it on item.');// We are accessing toLowerCase method on item. We can do it, typescript won't complain because item is of type any.
  // But item can be of anything, what if item is of type nunowmber?});
```

现在您可能确信 any 并不是您真正喜欢的类型，因为它不提供类型安全！这就是泛型出现的原因。

```
let stringStack: SimpleStackType<string>;
// initialized herestringStack.map(item => {
  console.log('Item is of type string here);
  console.log('You can even access .toLowerCase here! Should not be a surprise. You already knew it, right?');
});
```

你一定已经注意到，我们使用的**数组<someType>语法也是泛型的一种实现，你可以看到泛型的强大、安全和灵活性。是时候做出反应了。**

# React 应用程序的构建模块，组件

组件是任何 React 应用程序的构建块。由于本文是关于在 TypeScript 中反应函数组件的，我们将只讨论函数组件和一些其他类型声明。

## 功能组件

一个反应函数组件就像一个返回有效 JSX 的普通函数。下面的类型推理函数是一个非常有效的 React 组件。

```
function SomeComponent() {
  return (
    <div> Hello from a react component!</div>
  );
}
```

如果你想更明确，那么 React 提供了 FC(或者使用 FunctionComponent，二者相同)类型，这是一个泛型。你可以传递一个类型，这个类型是函数将要接受的道具的类型。

```
type Props = {
  name: string;
};const AnotherComponent: FC<Props> = ({name}) => <div>Hi {name}</div>;
```

您可以通过在道具上定义其他字段来在道具上定义它们。您可以通过在它们前面使用问号来使它们可选。

```
type Props = {
  name: string;
  optionalProp?: number;
};const OneMoreComponent: FC<Props> = ({name}) => <div>Hi {name}</div>;
```

## 其他类型声明

现在我们已经熟悉了基本的功能组件，但是有时我们可能需要与类组件交互，React 中还有一些其他重要的类型值得了解。

***做出反应。ReactNode*** 可以用作返回 JSX 的函数的返回类型。用于反应迟钝的儿童。您可以定义一个返回 ReactNode 的函数来输入您的渲染属性。有一个例子可以说明这一点:

```
type Props = {
  nameRenderer: (name: string) => React.ReactNode;
};
```

***做出反应。Component < PropType，StateType >*** 语法用于声明类组件。

***做出反应。如果你只关心组件和它接受的属性，而不关心它是一个函数组件还是一个类组件，那么可以使用 ComponentType*** *。下面是它的声明:*

```
// This is a type from react, use FC for functional component and Component for class components.type React.ComponentType<P = {}> = React.ComponentClass<P, any> | React.FunctionComponent<P>
```

# React 挂钩:打字版本

既然说的是函数组件，不说说钩子肯定是不完整的。但这绝对不是 React hooks 的介绍。

## 使用状态

可以将类型传递给使用状态，就像将类型传递给泛型一样。如果您的状态类型在应用程序的整个过程中没有改变，那么您可以简单地将初始状态传递给 useState，TypeScript 将推断出类型。

```
const [counter, setCounter] = useState(0);
```

这里，计数器的类型总是数字。如果你需要更复杂的类型，比如用户:

```
const [user, setUser] = useState({name: 'Binod', age: 1, ageUnit: 'month' });
```

在这种情况下，TypeScript 也会推断类型。但是，如果您的用户数据是动态设置的，比如在登录事件之后，用户最初为空，您将如何处理它？

```
const [user, setUser] = useState(null);
```

不完全是这样，因为 TypeScript 会将用户的类型推断为 null，并且当您试图将其设置为另一种类型的值时会报错。显然，你可以使用任何类型，但你想要类型安全，不是吗？

如果你想得有点难，或者你已经想通了，我们可以在这里使用一个联合类型。是的，我们现在将使用通用语法。

```
type User = {
  name: string,
  age: number,
  ageUnit: string, // or better 'month' | 'year' | 'day' | 'week' 
};const [user, setUser] = useState<User | null>(null);
```

从技术上来说，我们可以用 ***非空断言*** ***操作符*** 实现类似的东西，但不能完全满足用例。

现在我们还需要知道 useState 返回的 setUser 的类型，如果我们正在传递它的话。传递 setValue 有点酷，因为 React 保证了引用。他们的类型是:

```
React.Dispatch<React.SetStateAction<StateType>
```

***useMemo、useCallback、useDispatch 和 useContext、useRef、*** 在处理类型方面都差不多。它们都接受与它们接受的参数数量相对应的类型。所以说实话，我们不会讨论它们。useCallback 只是返回您传递给它的函数，类型保持不变。如果编译器不知怎么搞混了，你可以使用泛型语法。我见过一些使用 useMemo 的案例，但是还没有使用 useCallback。

# 自定义挂钩

让我们将计数器逻辑写成一个自定义挂钩:

```
function useCounter(initialValue: number) {
  const [counter, setCounter] = useState(initialValue);
  const increment = useCallback(() => setCounter(c => c + 1), []);
  const decrement = useCallback(() => setCounter(c => c - 1), []);
  return [counter, increment, decrement];
}
```

但是 TypeScript 并不能很好的播放上面的代码。挂钩的推断返回类型将是数组元素类型的并集。所以，当你试图调用增量或减量函数时，TypeScript 会报错。因为 TypeScript 将其视为:

```
type Increment: number | () => void | () => void;
```

并且联合中的一个类型是不可调用的。你需要明确定义钩子的返回类型。让我们为一个简单的元组类型做这件事:

```
Type Counter = [number, () => void, () => void];
```

使用计数器类型作为自定义钩子的返回类型，我们就完成了。如果你的钩子返回多个值，比如超过 3 个，那么你应该返回一个对象。它帮助你在更长的时间内维护代码。

这应该足够让你开始了。您可以根据需要查找更多信息。

如果你喜欢这篇文章，并且对前端开发、TypeScript、JavaScript 和 React 感兴趣，你可以考虑关注我。我通常一周发表一篇文章。

你有任何问题或反馈吗？求你了。我会欣喜若狂地回应。😊

留给你打字稿反应操场👋—[https://www.typescriptlang.org/play?jsx=2&esModuleInterop = true&q = 423 # example/typescript-with-react](https://www.typescriptlang.org/play?jsx=2&esModuleInterop=true&q=423#example/typescript-with-react)