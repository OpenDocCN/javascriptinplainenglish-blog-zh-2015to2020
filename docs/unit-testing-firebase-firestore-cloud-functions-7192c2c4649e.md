# 单元测试 Firebase Firestore 和云功能

> 原文：<https://javascript.plainenglish.io/unit-testing-firebase-firestore-cloud-functions-7192c2c4649e?source=collection_archive---------2----------------------->

![](img/9bde5b36d6a089f4c86a0b9609ca01d8.png)

我的一个个人项目让我打开了充满乐趣和新技术的潘多拉魔盒。我通常无法在白天工作时使用 Firebase Firestore 和云功能。🤖

我挑战自己，编写了一个函数，它接受有效载荷的数据，并在 Firebase 中创建了一个记录。伴随着这个挑战，我想用单元测试来包装我的功能，作为一个新的延伸目标。

官方的 [Firebase Cloud Functions 文档](https://firebase.google.com/docs/functions)易于阅读和理解非常基本的用例。我想在主要的例子之外再进一步。😄

# 密码

这里我有一个简单的函数来监听 Firestore 文档创建事件。它将调用云函数来获取数据，检查它是否存在，如果不存在，则创建一个关联记录。

```
const functions = require('firebase-functions');
const admin = require('firebase-admin');admin.initializeApp();
let db = admin.firestore();exports.onEpisodeTrackCreated = functions.firestore.document('episodes/{episodeId}/tracks/{trackIndex}')
  .onCreate((snap, context) => {
    const data = snap.data() if (!data.name) throw new Error('Missing `name` parameter') const name = data.name.trim()
    let tracksRef = db.collection('tracks') return tracksRef.where('name', '==', name).get()
      .then(snapshot => {
        if (snapshot.empty) {
          return tracksRef.add({
            name: name
          })
        }
        let doc
        snapshot.forEach(snapDoc => {
          doc = snapDoc
        })
        return doc
      })
        .then((doc) => {
          snap.ref.set({
            trackId: doc.id
          }, { merge: true })
          return doc
        })
  })
```

# 测试设置

安装`firebase-functions-test`和 Jest 一个流行的“包含电池”测试框架。

```
npm install --save-dev firebase-functions-test jest
```

我们需要创建一个`test`文件夹，在那里存储我们函数的单元测试。

接下来，我用要调用的测试脚本更新了`package.json`。

"脚本":{
"测试":" jest test/"
}

Firebase 云功能可以在线和离线模式下运行。在线模式意味着它将与你的 Firebase 帐户互动，创建/销毁数据。离线模式会导致我们中断通话，在我看来这是撰写本文的首选。

通过不定义任何配置选项，在离线模式下初始化 SDK。

```
const test = require('firebase-functions-test')();
```

让我们继续编写调用该函数的单元测试，并且应该成功地用 async/await 进行解析，否则它将抛出一个错误。

```
const test = require('firebase-functions-test')();
const functions = require('../index.js');describe('onEpisodeTrackCreated', () => {
  it('successfully invokes function', async () => {
    const wrapped = test.wrap(functions.onEpisodeTrackCreated);
    const data = { name: 'hello - world', broadcastAt: new Date() }
    await wrapped({
      data: () => ({
        name: 'hello - world'
      }),
      ref:{
        set: jest.fn()
      }
    })
  })
})
```

当我们现在运行测试时会发生什么？🤔

```
FAIL  tests/index.test.js
  onEpisodeTrackCreated
    ✕ successfully invokes function (832ms) ● onEpisodeTrackCreated › successfully invokes function Could not load the default credentials. Browse to https://cloud.google.com/docs/authenti
cation/getting-started for more information. at GoogleAuth.getApplicationDefaultAsync (node_modules/google-auth-library/build/src/auth/googleauth.js:161:19)
      at GoogleAuth.getClient (node_modules/google-auth-library/build/src/auth/googleauth.js:503:17)
      at GrpcClient._getCredentials (node_modules/google-gax/src/grpc.ts:150:20)
      at GrpcClient.createStub (node_modules/google-gax/src/grpc.ts:295:19)Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        1.91s
```

😢这可不好。

仔细想想这个错误，我们的代码中确实有很多错误。这个错误告诉我们一些关于凭证的信息。或许是与`firebase-admin`号上的`initializeApp`有关？🤔

我们将对此进行模拟，看看接下来会发生什么。

```
jest.mock('firebase-admin', () => ({
  initializeApp: jest.fn()
}))
```

而结果…

```
FAIL  tests/index.test.js
  ● Test suite failed to run TypeError: admin.firestore is not a function 3 | 
      4 | admin.initializeApp();
    > 5 | let db = admin.firestore();
        |                ^
      6 | 
      7 | exports.onEpisodeTrackCreated = functions.firestore.document('episodes/{episodeId}/tracks/{trackIndex}')
      8 |   .onCreate((snap, context) => { at Object.firestore (index.js:5:16)
      at Object.require (tests/index.test.js:23:19)Test Suites: 1 failed, 1 total
Tests:       0 total
Snapshots:   0 total
Time:        1.757s
```

太好了，这是一个更好的位置。因为我们正在调用`firestore`，但是我们已经完全模拟了实现，这是意料之中的。

现在来完成这个测试的模拟。😅

```
const mockQueryResponse = jest.fn()
mockQueryResponse.mockResolvedValue([
  {
    id: 1
  }
])jest.mock('firebase-admin', () => ({
  initializeApp: jest.fn(),
  firestore: () => ({
   collection: jest.fn(path => ({
     where: jest.fn(queryString => ({
       get: mockQueryResponse
     }))
   })) 
  })
}))
```

最后一轮。😬

```
PASS  tests/index.test.js
  onEpisodeTrackCreated
    ✓ successfully invokes function (3ms)Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.026s
```

太棒了。🙌

我真的希望这个解决方案能帮助你测试你的下一个项目。

# 来源

当然，这个结果并不是自然产生的，而是通过编码阶段在互联网上大量搜索相关解决方案的结果。

*   [用 Gompro 的 Jest 测试 Firebase 云函数](https://medium.com/@leejh3224/testing-firebase-cloud-functions-with-jest-4156e65c7d29#0369)
*   [Google 测试后台(非 HTTP)功能](https://firebase.google.com/docs/functions/unit-testing#testing_background_non-http_functions)
*   [异步/等待应答](https://jestjs.io/docs/en/asynchronous.html#async-await)