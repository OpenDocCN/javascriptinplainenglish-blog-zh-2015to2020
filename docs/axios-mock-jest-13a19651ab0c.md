# 在 Jest 中创建 Axios 模拟请求

> 原文：<https://javascript.plainenglish.io/axios-mock-jest-13a19651ab0c?source=collection_archive---------0----------------------->

## 如何为 Jest 创建自己的可重用 axios 模拟请求函数？

![](img/6641fa8aa07f5108d75a916422bc27a5.png)

# 假设我有一个名为 **loadData** 的方法，它负责从 REST get 端点加载数据。

```
loadData = () => {
    Axios.get(“https://jsonplaceholder.typicode.com/users")
}
```

现在我们想在上面写测试。

让我们写一些关于这个方法的测试。

## **测试 1** :方法已实施

```
expect(instance.loadData).toBeDefined()//Instance is your class or component shallow instance
```

## 测试 2:现在让我们创建一个模拟 axios 响应，看看这个方法返回了什么。

```
import axios from "axios";
jest.mock("axios") //Add this on top of your test file.
```

现在让我们将**的 loadData** 设为**异步**

```
loadData = async ()=>{
    Axios.get(“https://jsonplaceholder.typicode.com/users")
}
```

让我们考虑我们的 mockAxios 返回一些数据，如

```
 {data:”some data”}
```

因此，我们的方法 **loadData** 将返回与响应相同的数据。

我们的测试应该是这样的:

```
axios.get.mockResolvedValue({data:"some data"});
const result = await instance.loadData()
expect(result).toEqual({data:”some data”})
```

## 测试 3:这个测试覆盖了带有**mockreptedvalue 的 axios 的 catch 情况。**

```
axios.get.mockRejectedValue({error:"some error"});
  await instance.loadData().catch(err => {
  expect(err).toEqual({error:”some error”});
})
```

现在完成了！