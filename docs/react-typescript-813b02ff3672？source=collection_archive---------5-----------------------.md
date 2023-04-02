# React + TypeScript 入门

> 原文：<https://javascript.plainenglish.io/react-typescript-813b02ff3672?source=collection_archive---------5----------------------->

## 结合两者的基本好处

![](img/0226fba01d196638451948f4a056ffa6.png)

Source: the author

在本文中，我们将通过几个例子来说明如何使用 React & TypeScript。我们看看道具、界面、一些钩子和表单处理。

当然，我们必须首先建立一个设置。最简单的方法就是使用 **npx。**

```
npm install -g npx npx create-react-app my-app — template typescriptcd my-app/
```

如果您已经有一个 React.js 项目，您可以在以后向它添加 TypeScript 支持。

```
npm install — save typescript @types/node @types/react @types/react-dom @types/jest
```

然后只需将每个文件扩展名从“js”和“jsx”重命名为“tsx”。

好消息是 TypeScript 和 JavaScript 非常兼容。这意味着我们可以在。没有问题的 tsx 文件，其实就是普通的 javascript。

在我们的 app.tsx 中有这样一个例子:

```
function displayCount(userCount : number) : number {
  *return* userCount
}function add(x) {
  console.log(x)
}function App() {
  *return* (
    <div className="App">
      Current Users: {displayCount(24)}
      <button onClick={() => add(24)}>Click me</button>
    </div>
  );
}
```

upper 函数使用类型，这只有在 TypeScript 的帮助下才有可能。add-function 只有在 JavaScript 中才能以同样的方式工作。然而，我们可以两者并用。

您可能需要在 tsconfig.json: `"strict": false`中禁用严格模式

# 创建第一个组件

现在我们可以使用 TypeScript 为 React.js 编写一些代码，让我们创建一个基于函数的组件。

为此，我们可以选择使用下面的语法。所以我们定义了一个 React 类型的组件。FC，代表功能组件。

```
const UserInput : React.FC = () => {
  *return* <input type=”text”/>
}
```

多亏了 TypeScript，我们现在还可以定义我们的 props 需要哪些数据类型。

```
const UserInput : React.FC<{text: string}> = (props) => {
  *return* <input type=”text” placeholder={props.text}/>
}<UserInput text="Please enter"/>
```

如果我们现在将一个数字作为*文本*传递，TypeScript 将报告一个错误——因为我们期望一个名为*文本*的字符串作为道具。

# 为道具使用接口

但是我们也可以用一个*接口*来实现整个事情。

```
interface Props {
  text: string
}const UserInput : React.FC<Props> = (props) => {
  *return* <input type=”text” placeholder={props.text}/>
}
```

我们也可以组合接口。在这种情况下，我们指定我们期望作为道具的接口 car，它由 brand 和 speed 组成，两者都分配了一个数据类型。

```
interface Car {
  brand: string
  speed: number
}interface Props {
  car: Car
}const CarSeller : React.FC<Props> = (props) => {
  *return* <p>The {props.car.brand} is {props.car.speed} fast </p>
}<CarSeller car={{brand: "Mercedes", speed: 150}}/>
```

## 使道具可选

可能对于汽车的潜在买家来说，速度并不感兴趣。在这种情况下，我们不必交出它，我们可以在界面中将它标记为可选的——这是可以用问号实现的。

```
interface Car {
  brand: string
  speed?: number
}
```

通过为我们的道具使用接口，我们可以创建一个清晰易懂的列表，列出组件需要和可选的道具。

为了稍微清理一下，使代码更容易阅读，我们当然可以不使用 TSX 中的 props 关键字:

```
const CarSeller : React.FC<Props> = ({car}) => {
  *return* <p>The {car.brand} is {car.speed} fast </p>
}
```

我们也可以将接口分类到它们自己的文件中。在同一个文件中为每个组件提供一个接口是有意义的，这样你可以直接看到需要哪些道具。

但是对于我们的 props 接口，我们最终访问 car 接口，如果我们在其他几个组件中需要它，我们应该为它创建一个单独的文件。

**CarInterface.ts:**

```
*export* *default* interface Car {
  brand: string
  speed?: number
}
```

**在 App.tsx 中导入:**

```
*import* Car *from* “./CarInterface”
  interface Props {
  car: Car
}
```

# 钩住

对于钩子来说，使用 TypeScripts 特性(比如类型)也是完全可选的——但这是有意义的。但是我们不必为 *useState* 指定数据类型，如下例所示。

```
function App() {
  const [count, changeCount] = useState<number>(0)
  *return* <p>{count}</p>
}
```

同样，我们可以使用接口来定义我们的数据类型。

```
interface userInputInterface {
  count: number
}function App() {
  const [userInput, changeInput] = 
  useState<userInputInterface({count: 0}) *return* <p>{userInput.count}</p>
}
```

## 使用引用

要做到这一点，我们需要访问一些您在 React 或 Vue 等框架之外使用 TypeScript 时已经知道的东西。HTMLInputElement 类型—因为我们现在使用的是 refs。

```
const inputFieldRef = useRef<HTMLInputElement>(null)const getFocus = () => { inputFieldRef.current?.focus() }*return* <>
  <input type=”text” ref={inputFieldRef} />
  <button onClick={getFocus}>Click me</button>
</>
```

我们将空值作为参数传递给 T2 useRef T3，因为在执行时，输入标签还没有在 DOM 中呈现。这样，我们可以确保连接从我们的输入标签开始。

# 用 refs & TypeScript 处理表单

我们刚刚让 HTMLInputElement 用 ref 访问 DOM。现在让我们来看看在处理表单时使用引用的类型脚本。

形状总是有一个“提交”按钮，它确保形状被发送。如果没有框架，表单会有一些属性，例如，数据应该发送到哪个 URL 以及使用哪种方法。

但是我们想用 JavaScript 风格的 React.js 手动解决所有这些事情。因此，我们编写了一个函数，当表单被发送时触发这个函数，然后用 *preventDefault()* 阻止标准过程。

```
const App : React.FC = () => { const submitHandler = (event: React.FormEvent) => {
    event.preventDefault()
  }  return (
    <form onSubmit={submitHandler}>
      <input type=”text” />
      <input type=”submit” value=”Submit”/>
   </form>
  )
}
```

作为 *submitHandler* 函数的参数，我们传递事件，它必须是类型 *FormEvent* ，因为它是我们表单的一个动作。

```
const App : React.FC = () => {
  const textInputRef = useRef<HTMLInputElement>(null) const submitHandler = (event: React.FormEvent) => {
    event.preventDefault()
    const enteredText = textInputRef.current!.value
    console.log(“submitted:”, enteredText)
  } *return* (
  <form onSubmit={submitHandler}>
    <input type=”text” ref={textInputRef} />
    <input type=”submit” value=”Submit”/>
   </form>
  )
}
```

作为 *useRef、*的默认值，我们再次取 *null* ，作为类型，我们重新取 HTMLInputElement。

我们处理提交的函数现在访问 ref。利用电流！语法，我们告诉 TypeScript 如果值是 *null* 就没问题。最后，还没有输入任何内容。

在实际的输入字段中，我们只需要连接到 ref，然后一切都准备好了。如果我们现在按 submit，就会执行处理函数，从 ref 获取用户输入，并将其输出到控制台。

所以我们可以用 TypeScript & React 来评估一个表单。