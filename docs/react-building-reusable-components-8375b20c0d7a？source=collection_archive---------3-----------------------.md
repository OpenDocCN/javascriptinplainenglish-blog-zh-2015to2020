# 用样式化组件构建可重用的 React 组件

> 原文：<https://javascript.plainenglish.io/react-building-reusable-components-8375b20c0d7a?source=collection_archive---------3----------------------->

![](img/9d400deaa4ec3ba5939f8900d35f95fe.png)

当我开始开发我的第一个大型应用程序时，我第一次使用了`styled-components`，我真的很喜欢它的工作方式。它允许您构建可重用的组件，当您构建不同的布局时，这些组件易于扩展并适应新的情况。

我将介绍一些你可以使用`styled-components`来创建可重用组件的方法，这些组件抽象了 CSS 和 HTML，创建了你自己的定制组件库。

`styled-components`的语法乍一看可能有点奇怪。您可以创建任何类型的 HTML 元素及其 CSS 样式，如下所示:

```
const Container = styled.div`
  display: flex;
`
```

CSS 包含在紧跟在 HTML 元素的键名后面的反勾号中。一开始感觉很奇怪，但我很快就习惯了，因为这种语法真的很方便。您不必在样式表和组件之间切换。所有东西都在同一个文件中声明。

这里的一个危险是你的文件可能很快变得混乱，因为组件的不同部分有它们自己的风格。这是创建简单的、可重用的组件的好理由，您可以将这些组件拼凑成不同的布局。

构建简单的、可重用的组件的另一个很好的理由是，随着应用程序的增长，如果所有的东西都源自相同的组件，那么对布局和设计进行微小的调整会变得更快。

# 柔性框布局

我要构建的第一个组件是一个基本布局，它利用 flex box 来提高响应能力。首先，我们需要一个 flex 容器来放置内容:

```
const Container = styled.div`
  display: flex;
  justify-content: center;
  padding-top: 5rem;
`
```

然后是我们的响应 flex box，其中包含媒体查询:

```
const Flex = styled.div`
  display: flex;
  flex-direction: column; @media(min-width: 768px){
    flex-direction: row;
  }
`
```

将这些放在一起，我们得到了一个`Layout`组件:

```
const Layout ({ children, className }) => (
  <Container className={className}>
    <Flex>
      {children}
    </Flex>
  </Container>
)
```

我还构建了一个在`Layout`内部使用的`Column`组件。这里，我使用了`styled-components`的一个很酷的特性，你可以在 CSS 字符串中使用变量来控制道具的样式:

```
const Container = styled.div`
  display: flex;
  flex-direction: column;
  align-items: ${({ centerAlign }) => centerAlign ? 'center' : 'flex-start'};
  justify-content: ${({ centerJustify }) => centerJustify ? 'center' : 'flex-start'}
`
```

这在纯黑色文本中可能很难阅读，但我正在做的是使用`${}`将一个函数插入 CSS 代码字符串。该功能是通过提供给`Container`的道具实现的。然后，我编写了三元语句，根据 props 值返回正确的 CSS。

最终的`Column`组件如下所示:

```
const Column = ({
  children,
  className,
  centerAlign,
  centerJustify
}) => (
  <Container
    className={className}
    centerAlign={centerAlign}
    centerJustify={centerJustify}
  >
    {children}
  </Container>
)
```

然后，我可以一起使用`Layout`和`Column`组件来构建这样的响应性布局:

```
const Page = () => (
  <Layout>
    <Column>
      // child components
    </Column>
    <Column>
      // child components
    </Column>
  </Layout>
)
```

# 表单元素

现在我想构建一些表单元素。我将从文本输入开始:

```
const Input = styled.input`
  font-size: 1.5rem
`
```

然后是一个标签:

```
const Label = styled.label`
  margin-bottom: 1rem;
`
```

最后，一个容器来装它们:

```
const Container = styled.div`
  display: flex;
  flex-direction: column;
  max-width: 500px;
`
```

然后将它们放在一起，创建一个带有可选标签的可重用输入元素:

```
const TextInput = ({ className, label, onChange, value}) => (
  <Container className={className}>
    {label && <Label>{label}</Label>}
    <Input
      type='text'
      onChange={onChange}
      value={value}
      name={label}
    />
  </Container>
)
```

我要构建的最后一个组件是一个按钮。我喜欢构建一个基本的按钮组件，因为浏览器倾向于给按钮添加一些奇怪的默认样式，每个按钮都需要覆盖这些样式。

```
const Container = styled.button`
  border: none;
  background-color: $(({ disabled }) => disabled ? 'gray' : 'red'};
  padding: 0.5rem;
  width: ${({ fullWidth }) => fullWidth ? '100%' : '300px'}; &:hover {
    cursor: pointer;
    opacity: 0.9;
  }
`
```

在这里，我改变了按钮禁用时的颜色。我还添加了一个选项，使按钮的宽度等于其容器的宽度。否则，默认宽度为 300 像素。

现在我可以保留`<button>`元素的语义特性，同时完全定制它。

```
const Button = ({ children, className, onClick, disabled }) => (
  <Container
    className={className}
    onClick={onClick}
    disabled={disabled}
  >
    {children}
  </Container>
)
```

以上是用`styled-components`构建可重用组件的简要介绍。你会注意到我在每个组件中都包含了一个`className`道具。这是为了允许使用`styled`对象作为功能进行进一步定制:

```
const CustomizedButton = styled(Button)`
  // some CSS
`
```

如果你需要增加更大的页边空白来在页面上留出空间，这很有帮助。

## 进一步阅读

[](https://bit.cloud/blog/how-to-reuse-react-components-across-your-projects-l4pz83f4) [## 如何在项目中重用 React 组件

### 最后，您完成了为应用程序中的表单创建一个奇妙的输入字段的任务。你对……很满意

比特云](https://bit.cloud/blog/how-to-reuse-react-components-across-your-projects-l4pz83f4) 

*更多内容看* [***说白了就是***](https://plainenglish.io/) *。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于* [***推特***](https://twitter.com/inPlainEngHQ) ， [***领英***](https://www.linkedin.com/company/inplainenglish/) ***，***[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查出* [***电路***](https://circuit.ooo/) ***。****