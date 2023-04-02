# 如何修复“虚拟化列表不应嵌套在普通 ScrollViews 中”警告

> 原文：<https://javascript.plainenglish.io/how-to-fix-virtualizedlists-should-never-be-nested-inside-plain-scrollviews-warning-3a2a887b4ea0?source=collection_archive---------0----------------------->

## 以正确的方式修复常见的反应本机警告

![](img/ddd9e50e686fcfcdfe8c6269118666b7.png)

Photo from [Unsplash](https://unsplash.com/photos/YSZS_nDU8js)

如果您使用 React Native 开发应用程序，并且在普通的 ScrollView 中嵌套了 FlatList 或 SectionList 组件，您可能会在调试器中遇到以下警告:

```
VirtualizedLists should never be nested inside plain ScrollViews with the same orientation - use another VirtualizedList-backed container instead.
```

虽然此警告表明当前配置不理想，但它没有解释为什么会有问题或如何解决问题。在本文中，我们将探讨这个警告背后的原因，并提供修复它的解决方案。

虚拟化列表，如`SectionList`和`FlatList`，旨在优化性能和减少呈现大型内容列表时的内存消耗。这些列表仅呈现当前在屏幕上可见的内容，并用空白替换列表项的其余部分。当用户滚动时，列表根据滚动位置更新并呈现新项目。这种方法允许更平滑、更快速地呈现大型列表，而不会消耗过多的资源。

如果您将这两个列表中的任何一个放在 ScrollView 中，它们将无法计算当前窗口的大小，这会导致性能问题。此外，您将收到前面提到的警告。因此，在使用 ScrollView 时，确保列表能够正确计算当前窗口的大小是很重要的。

# 如何以正确的方式修复这个警告

要修复前面讨论的警告，只需删除 ScrollView，并将 FlatList 周围的所有组件放在 ListFooterComponent 和 ListHeaderComponent 中。

下面的例子演示了这种方法，用户可以滚动浏览不同的配方。主视图由一组组件组成，如页眉、页脚、一些文本和封面照片，所有这些组件都包含在 ListFooterComponent 和 ListHeaderComponent 中。这允许 FlatList 正确计算当前窗口的大小，并消除了对 ScrollView 的需要。

```
const Main = () => {
  return (
    <ScrollView>
      <CoverPhoto *src*={images[0]} />
      <Title>Welcome</Title>
      <Text>Take a look at the list of recipes below:</Text>
      <Footer />
    </ScrollView>
  );
};
```

现在，假设我们想使用`FlatList`在最后一个`Text`元素下面列出所有的食谱，在这之后看起来会像这样:

```
const Main = () => {
  const renderItem= (item, index) => {
    return (
      <Recipe
      *key*={index}
      *recipe*={item}>
    )
  } return (
    <ScrollView>
      <CoverPhoto *src*={images[0]} />
      <Title>Welcome</Title>
      <Text>Take a look at the list of recipes below:</Text>
      <FlatList
        *data*={recipes}
        *renderItem*={renderItem}
      />
      <Footer/>
    </ScrollView>
  );
};
```

当然，这会给你之前提到的警告，你也不能使用 FlatList 的性能特性。这就是为什么我们要完全去掉 ScrollView，而使用 ListFooterComponent 和 ListHeaderComponent 属性，如下所示:

```
const Main = () => {
  const renderItem= (item, index) => {
    return (
      <Recipe
      *key*={index}
      *recipe*={item}>
    )
  } return (
    <FlatList
      *ListHeaderComponent*={
      <>
        <CoverPhoto *src*={images[0]} />
        <Title>Welcome</Title>
        <Text>Take a look at the list of recipes below:</Text>
      </>}
      *data*={recipes}
      *renderItem*={renderItem}
      *ListFooterComponent*={
        <Footer/>
      }/>
  );
};
```

两个 props 都只接受一个组件，所以我们用片段将组件包装在 ListHeaderComponent 中。

## 我们做到了！不再有警告，一切看起来还是一样！