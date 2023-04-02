# JavaScript 中的基本中间件模式

> 原文：<https://javascript.plainenglish.io/basic-middleware-pattern-in-javascript-ef8756a75cb1?source=collection_archive---------0----------------------->

![](img/fdde0d9df9d1246c30bec430dc6bad97.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 想知道流行的 web 框架中的中间件是如何工作的吗，例如 [Express](https://expressjs.com/) 或 [Koa](https://koajs.com/) ？

在 Express 中，我们的中间件功能具有以下特征:

```
const middleare = (req, res, next) => {
 // do stuffs
 next()
}
```

在 Koa，我们有这个:

```
const middleware = (ctx, next) => {
  // do stuffs
  next()
}
```

基本上，您有一些对象(`req`、`res`表示 Express 或`ctx`表示 Koa)和一个`next()`函数作为中间件函数的参数。当`next()`被调用时，下一个中间件功能被调用。如果你修改了当前中间件函数中的参数对象，下一个中间件将会收到这些修改后的对象。例如:

```
// Middleware usage in Koa

app.use((ctx, next) => {
  ctx.name = 'Doe'
  next()
})

app.use((ctx, next) => {
  console.log(ctx.name) // will log `Doe`
})

app.use((ctx, next) => {
  // this will not get invoked
})
```

而如果不调用`next()`函数，执行就停止在那里，下一个中间件函数不会被调用。

# 履行

那么，如何实现这样的模式呢？用 30 行 JavaScript 代码:

```
function Pipeline(...middlewares) {
  const stack = middlewares

  const push = (...middlewares) => {
    stack.push(...middlewares)
  }

  const execute = async (context) => {
    let prevIndex = -1

    const runner = async (index) => {
      if (index === prevIndex) {
        throw new Error('next() called multiple times')
      }

      prevIndex = index

      const middleware = stack[index]

      if (middleware) {
        await middleware(context, () => {
          return runner(index + 1)
        })
      }
    }

    await runner(0)
  }

  return { push, execute }
}
```

这种中间件模式的实现几乎和 Koa 一样。如果你想看看 Koa 是怎么做的，可以查看一下`[koa-compose](https://github.com/koajs/compose/blob/master/index.js#L31-L47)`包的源代码。

# 使用

让我们看一个使用它的例子:

```
// create a middleware pipeline
const pipeline = Pipeline(
  // with an initial middleware
  (ctx, next) => {
    console.log(ctx)
    next()
  }
)

// add some more middlewares
pipeline.push(
  (ctx, next) => {
    ctx.value = ctx.value + 21
    next()
  },
  (ctx, next) => {
    ctx.value = ctx.value * 2
    next()
  }
)

// add the terminating middleware
pipeline.push((ctx, next) => {
  console.log(ctx)
  // not calling `next()`
})

// add another one for fun ¯\_(ツ)_/¯
pipeline.push((ctx, next) => {
  console.log('this will not be logged')
})

// execute the pipeline with initial value of `ctx`
pipeline.execute({ value: 0 })
```

如果您运行这段代码，您能猜到输出是什么吗？是的，你猜对了:

```
{ value: 0 }
{ value: 42 }
```

顺便说一句，这也绝对适用于异步中间件功能。

# 以打字打的文件

现在，给它一些打字稿的爱怎么样？

```
type Next = () => Promise<void> | void

type Middleware<T> = (context: T, next: Next) => Promise<void> | void

type Pipeline<T> = {
  push: (...middlewares: Middleware<T>[]) => void
  execute: (context: T) => Promise<void>
}

function Pipeline<T>(...middlewares: Middleware<T>[]): Pipeline<T> {
  const stack: Middleware<T>[] = middlewares

  const push: Pipeline<T>['push'] = (...middlewares) => {
    stack.push(...middlewares)
  }

  const execute: Pipeline<T>['execute'] = async (context) => {
    let prevIndex = -1

    const runner = async (index: number): Promise<void> => {
      if (index === prevIndex) {
        throw new Error('next() called multiple times')
      }

      prevIndex = index

      const middleware = stack[index]

      if (middleware) {
        await middleware(context, () => {
          return runner(index + 1)
        })
      }
    }

    await runner(0)
  }

  return { push, execute }
}
```

随着一切都被类型化，现在您可以为特定的中间件管道声明上下文对象的类型，如下所示:

```
type Context = {
  value: number
}

const pipeline = Pipeline<Context>()
```

好了，暂时就这些了。

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**

*原载于 2020 年 10 月 4 日*[*https://muniftanjim . dev*](https://muniftanjim.dev/blog/basic-middleware-pattern-in-javascript/)*。*