# 在单个 React Apollo 应用中处理多个 GraphQL 服务器端点

> 原文：<https://javascript.plainenglish.io/handling-multiple-graphql-server-endpoints-in-a-single-react-apollo-app-badda8ccb1e5?source=collection_archive---------7----------------------->

![](img/f1ae35fabcfa5d4554c42b4aa0ff4671.png)

事实往往并非如此，假设 95%的情况下，您的前端应用程序只有一个解析数据的 GraphQL 端点。但是我最近遇到了一个项目，我们的前端应用程序需要与两个不同的 GraphQL 服务器对话。所以我们想出了一个适合我们情况的解决方案。

这实际上相当简单，我们需要做的是用我们自己的提供者围绕 ApolloProvider 创建一个抽象，并控制在该上下文中使用的实际客户端。

让我们创建一个文件***apollocontext . tsx***

```
*import* React, { createContext, useContext, *ComponentProps*, *FC*, useState } *from* 'react';
*import* ApolloClient *from* 'apollo-boost';
*import* { ApolloProvider } *from* '@apollo/react-hooks';*import* { INTERNAL_GRAPHQL, EXTERNAL_GRAPHQL } *from* '.../config';
*import* { ServiceType } *from* '.../constants';

*interface ApoloContext* {
  setCurrentClient: (*type*: ServiceType) => *void* }

*const* ApoloContextProvider = createContext({} *as ApoloContext*);

*export const* useApoloContext = (): *ApoloContext* =>
  useContext(ApoloContextProvider);*export const* ApoloContext: *FC* =
  ({ *children* }: *ComponentProps*<*FC*>): JSX.*Element* => {

  *const* [client, setClient] =
    useState(*new* ApolloClient({ uri: INTERNAL_GRAPHQL }))

  *const* setCurrentClient = (*type*: ServiceType): *void* => {
    *let* url = '';

    *switch* (*type*) {
      *case* ServiceType.*External*:
        url = EXTERNAL_GRAPHQL;
        *break*;
      *default*:
        url = INTERNAL_GRAPHQL
    }

    setClient(*new* ApolloClient({ uri: url }))
  }

  *return* (
    <ApoloContextProvider.Provider *value*={{setCurrentClient}} >
      <ApolloProvider *client*={client}>
        {*children*}
      </ApolloProvider>
    </ApoloContextProvider.Provider>
  )
};
```

这让你可以在应用需要的时候改变客户端。例如，在下一个组件中，我们将切换到一个外部端点，在这个组件内部使用它，当然，在组件卸载时进行清理。

```
...import { useApoloContext } from '...'
*import* { ServiceType } *from* '.../constants';...export const ComponentWithExternalGraphQl = () => { ... const { setCurrentClient } = useApoloContext(); useEffect(() => {
     setCurrentClient(ServiceType.*External*);

     return () => setCurrentClient(ServiceType.*Internal*);
   }, []) ...}
```

就像我在开始所说的，如果你遇到需要两个端点的情况，你需要重新思考你的项目结构，这只是一个快速的解决方案来绕过这个障碍。

*更多内容看* [***说白了就是***](https://plainenglish.io/) *。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于* [***推特***](https://twitter.com/inPlainEngHQ) ，[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*，*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)*，以及* [***不和***](https://discord.gg/GtDtUAvyhW) ***。***

***对缩放您的软件启动感兴趣*** *？检查出* [***电路***](https://circuit.ooo/?utm=publication-post-cta) *。*