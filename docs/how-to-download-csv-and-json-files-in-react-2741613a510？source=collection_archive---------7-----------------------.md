# 如何在 React 中下载 CSV 和 JSON 文件

> 原文：<https://javascript.plainenglish.io/how-to-download-csv-and-json-files-in-react-2741613a510?source=collection_archive---------7----------------------->

![](img/69c49551730a31ebaf0f87929bbec686.png)

*本文原载于* [*企业之路*](https://theroadtoenterprise.com/blog/how-to-download-csv-and-json-files-in-react) *博客。在那里阅读，获得最佳阅读体验。*

有些网站让用户下载 CSV 或 JSON 数据作为文件。这种功能非常有用，因为用户可以下载数据进行进一步处理或共享。在本文中，您将了解如何添加允许用户在 React 中导出表格并以 JSON 和 CSV 格式下载的功能。

你可以在 [GitHub repo](https://github.com/ThomasFindlay/csv-json-files-download-in-react) 中找到完整的代码示例。

# 项目设置

首先，让我们使用 [Vite](https://vitejs.dev) 创建一个新的 React 项目。

```
npm init vite@latest csv-json-files-download -- --template react
```

创建项目后，通过运行`npm install`将 *cd* 放入其中以安装依赖项，然后使用`npm run dev`启动开发服务器。

接下来，我们需要修改 **App.jsx** 和 **App.css** 文件。

**src/App.jsx**

```
import React from 'react'

function App() {
  return (
    <div className='App'>
      <h1>How to download CSV and JSON files in React</h1>
    </div>
  )
}

export default App
```

**src/App.css**

```
.App {
  max-width: 40rem;
  margin: 2rem auto;
}
```

这对于初始设置来说已经足够了。让我们从添加导出到 JSON 的功能开始。

# 导出到 JSON

让我们首先创建一个包含用户数据的文件，该文件将用于下载文件和呈现表格。

**src/users.json**

```
{
  "users": [
    {
      "id": 1,
      "name": "Caitlyn",
      "surname": "Kerluke",
      "age": 24
    },
    {
      "id": 2,
      "name": "Rowan ",
      "surname": "Nikolaus",
      "age": 45
    },
    {
      "id": 3,
      "name": "Kassandra",
      "surname": "Haley",
      "age": 32
    },
    {
      "id": 4,
      "name": "Rusty",
      "surname": "Arne",
      "age": 58
    }
  ]
}
```

接下来，我们需要更新`App`组件，以利用`users`数据并将其显示在表格中。除此之外，我们将添加一个按钮来触发下载。下面你可以看到`App.jsx`组件的代码。除了组件。我们有两个功能:`exportToJson`和`downloadFile`。前者用适当的参数调用后者。`downloadFile`函数接受一个对象作为参数，并期望三个属性:

*   *数据*
*   *文件名*
*   *文件类型*

`data`和`fileType`用于创建下载的`blob`。之后，我们创建一个锚元素，并在其上调度一个`click`事件。

**src/App.jsx**

```
import React from 'react'
import './App.css'
import usersData from './users.json'

const downloadFile = ({ data, fileName, fileType }) => {
  // Create a blob with the data we want to download as a file
  const blob = new Blob([data], { type: fileType })
  // Create an anchor element and dispatch a click event on it
  // to trigger a download
  const a = document.createElement('a')
  a.download = fileName
  a.href = window.URL.createObjectURL(blob)
  const clickEvt = new MouseEvent('click', {
    view: window,
    bubbles: true,
    cancelable: true,
  })
  a.dispatchEvent(clickEvt)
  a.remove()
}

const exportToJson = e => {
  e.preventDefault()
  downloadFile({
    data: JSON.stringify(usersData.users),
    fileName: 'users.json',
    fileType: 'text/json',
  })
}

function App() {
  return (
    <div className='App'>
      <h1>How to download CSV and JSON files in React</h1>

      <table className='usersTable'>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Surname</th>
            <th>Age</th>
          </tr>
        </thead>
        <tbody>
          {usersData.users.map(user => {
            const { id, name, surname, age } = user
            return (
              <tr key={id}>
                <td>{id}</td>
                <td>{name}</td>
                <td>{surname}</td>
                <td>{age}</td>
              </tr>
            )
          })}
        </tbody>
      </table>
      <div className='actionBtns'>
        <button type='button' onClick={exportToJson}>
          Export to JSON
        </button>
      </div>
    </div>
  )
}

export default App
```

我们可以添加一些样式，这样桌子看起来会更好一些。

**src/App.css**

```
.App {
  max-width: 40rem;
  margin: 2rem auto;
}

.usersTable th,
.usersTable td {
  padding: 0.4rem 0.6rem;
  text-align: left;
}

.actionBtns {
  margin-top: 1rem;
}

.actionBtns button + button {
  margin-left: 1rem;
}
```

很好，现在您应该能够通过点击`Export to JSON`按钮将`users`数据下载为 JSON 文件。接下来，我们将添加`Export to CSV`功能。

# 导出到 CSV

我们需要另一个按钮，将用于导出数据到一个 CSV 文件。除此之外，我们还需要一个处理程序。`usersData`是 JSON 格式的，所以我们需要将其转换成 CSV 格式，然后再将其传递给`downloadFile`函数。

**src/App.jsx**

```
import React from 'react'
import './App.css'
import usersData from './users.json'

const downloadFile = ({ data, fileName, fileType }) => {
  const blob = new Blob([data], { type: fileType })

  const a = document.createElement('a')
  a.download = fileName
  a.href = window.URL.createObjectURL(blob)
  const clickEvt = new MouseEvent('click', {
    view: window,
    bubbles: true,
    cancelable: true,
  })
  a.dispatchEvent(clickEvt)
  a.remove()
}

const exportToJson = e => {
  e.preventDefault()
  downloadFile({
    data: JSON.stringify(usersData.users),
    fileName: 'users.json',
    fileType: 'text/json',
  })
}

const exportToCsv = e => {
  e.preventDefault()

  // Headers for each column
  let headers = ['Id,Name,Surname,Age']

  // Convert users data to a csv
  let usersCsv = usersData.users.reduce((acc, user) => {
    const { id, name, surname, age } = user
    acc.push([id, name, surname, age].join(','))
    return acc
  }, [])

  downloadFile({
    data: [...headers, ...usersCsv].join('\n'),
    fileName: 'users.csv',
    fileType: 'text/csv',
  })
}

function App() {
  return (
    <div className='App'>
      <h1>How to download CSV and JSON files in React</h1>

      <table className='usersTable'>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Surname</th>
            <th>Age</th>
          </tr>
        </thead>
        <tbody>
          {usersData.users.map(user => {
            const { id, name, surname, age } = user
            return (
              <tr key={id}>
                <td>{id}</td>
                <td>{name}</td>
                <td>{surname}</td>
                <td>{age}</td>
              </tr>
            )
          })}
        </tbody>
      </table>
      <div className='actionBtns'>
        <button type='button' onClick={exportToJson}>
          Export to JSON
        </button>
        <button type='button' onClick={exportToCsv}>
          Export to CSV
        </button>
      </div>
    </div>
  )
}

export default App
```

# 包裹

我们找到了。我希望你喜欢这篇文章。现在，您应该已经很好地掌握了如何在自己的项目中添加下载文件功能的知识。请记住，即使我使用 React 来演示下载示例，您也可以在其他框架中使用下载逻辑，如 Vue、Svelte 或 Angular。

想了解最新信息并学习更多编程技巧吗？请务必在 [Twitter](https://twitter.com/thomasfindlay94) 上关注我，并订阅[时事通讯](https://theroadtoenterprise.com/blog/subscribe)！

*更多内容请看*[***plain English . io***](http://plainenglish.io/)