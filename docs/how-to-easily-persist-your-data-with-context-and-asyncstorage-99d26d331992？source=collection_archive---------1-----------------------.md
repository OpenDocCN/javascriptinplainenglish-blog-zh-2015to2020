# 如何通过上下文和异步存储轻松持久化数据

> 原文：<https://javascript.plainenglish.io/how-to-easily-persist-your-data-with-context-and-asyncstorage-99d26d331992?source=collection_archive---------1----------------------->

## 学习在 React Native 中使用钩子和异步存储

![](img/1c4d17804c63bab62c9e89b2906ece2d.png)

cat in a box

在大多数移动应用程序中，在会话之间持久保存用户数据是一种相当常见的做法，使用 [**React Native**](https://reactnative.dev/) 来完成这一点就像使用 [**AsyncStorage**](https://github.com/react-native-community/async-storage) 一样简单。这将允许您保留对任何组件的更改或更新，即使应用程序重新启动、刷新或关闭。

以这个计数器为例，如果您增加或减少计数并重新启动应用程序，计数将重置为零。

```
import React, { useContext, createContext, useState } from 'react';
import { View, Button, StyleSheet, Text } from 'react-native';const CounterContext = createContext(0);const useCounter = () => useContext(CounterContext);const CounterContextProvider = ({ children }) => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  const decrement = () => setCount((value) => value - 1);return (
    <CounterContext.Provider 
      value={{ 
        count, 
        increment, 
        decrement
       }}>
         {children}
    </CounterContext.Provider>
  );
};const App = () => {
  const { count, increment, decrement } = useCounter(); return (
    <View 
      style={styles.container}>
      <Text>{count}</Text>
      <Button title="Increment" onPress={() => increment()} />
      <Button title="Decrement" onPress={() => decrement()} />
    </View>
  );
};const styles = StyleSheet.create({   
  container: {     
    flex: 1,     
    alignItems: 'center',     
    justifyContent: 'center',   
  }
});export default () => (
  <CounterContextProvider>
    <App />
  </CounterContextProvider>
);
```

为了解决这个问题，让我们实现 [**AsyncStorage**](https://github.com/react-native-community/async-storage) 。

首先用 [**useEffect**](https://reactjs.org/docs/hooks-effect.html) 钩子将计数值存储在 [**AsyncStorage**](https://github.com/react-native-community/async-storage) 中，并添加计数作为依赖项。通过这样做，每次计数值被更新时，它将被存储。

```
import React, { useContext, createContext, useState, useEffect } from 'react';
import { View, Button, StyleSheet, Text } from 'react-native';
import AsyncStorage from '[@react](http://twitter.com/react)-native-community/async-storage';// ...const CounterContextProvider = ({ children }) => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  const decrement = () => setCount((value) => value - 1);

  useEffect(() => {
    AsyncStorage.setItem('COUNTER_APP::COUNT_VALUE', `${count}`);
  }, [count]); return (
    <CounterContext.Provider 
      value={{ 
        count, 
        increment, 
        decrement
    }}>
      {children}
    </CounterContext.Provider>
  );
};// ...
```

不要忘记在存储之前将值转换成字符串。

现在从 [**AsyncStorage**](https://github.com/react-native-community/async-storage) 中检索已有数据的状态。

```
const INITIAL_COUNT = 0;const CounterContextProvider = ({ children }) => {
  const [count, setCount] = useState(INITIAL_COUNT);
  const increment = () => setCount((value) => value + 1);
  const decrement = () => setCount((value) => value - 1); useEffect(() => {
    AsyncStorage.getItem('COUNTER_APP::COUNT_VALUE')
      .then((value) =    { 
        if (value) {
        setCount(parseInt(value));
      }
    });
  }, []); useEffect(() => {
    if (count !== INITIAL_COUNT) {
      AsyncStorage.setItem('COUNTER_APP::COUNT_VALUE', `${count}`);
    }
  }, [count]);return (
    <CounterContext.Provider 
      value={{ 
        count, 
        increment, 
        decrement
      }}>
        {children}
    </CounterContext.Provider>
  );
};// ...
```

请注意，新的 [**useEffect**](https://reactjs.org/docs/hooks-effect.html) 钩子从 [**AsyncStorage**](https://github.com/react-native-community/async-storage) 中检索数据将只在挂载时运行，因为它没有依赖关系；它获取现有值，并在用该值更新计数之前将其解析为一个整数。

此外，现有的 [**使用效果**](https://reactjs.org/docs/hooks-effect.html) 将数据存储在 [**异步存储**](https://github.com/react-native-community/async-storage) 中，现在检查以确保存储的值不会被状态中的初始值覆盖。

这种基本模式很容易扩展到存储更复杂的 JSON 对象。

这里是**最终代码**供你参考。

```
// App.js
import React, { useContext, createContext, useState, useEffect } from 'react';
import { View, TouchableOpacity, StyleSheet, Text } from 'react-native';
import AsyncStorage from '[@react](http://twitter.com/react)-native-community/async-storage';const CounterContext = createContext(0);
const useCounter = () => useContext(CounterContext);
const CounterContextProvider = ({ children }) => {
  const [count, setCount] = useState(0); const increment = () => {
    setCount((value) => value + 1);
  }; const decrement = () => setCount((value) => value - 1);

  useEffect(() => {
    if (count !== 0) {
      AsyncStorage.setItem('COUNTER_APP::COUNT_VALUE', `${count}`);
    }
  }, [count]); useEffect(() => {
    AsyncStorage.getItem('COUNTER_APP::COUNT_VALUE')
    .then((value) => {
      if (value) {
        setCount(parseInt(value));
      }
    });
  }, []);

  return (
    <CounterContext.Provider
      value={{
        count,
        increment,
        decrement,
      }}>
      {children}
    </CounterContext.Provider>
  );
};const App = () => {
  const { count, increment, decrement } = useCounter();
  return (
    <View style={styles.container}>
      <Text style={styles.counterText}>{count}</Text>
      <View style={styles.group}>
        <TouchableOpacity 
          onPress={() => increment()} 
          style={styles.button}>
          <Text style={styles.buttonTitle}>+</Text>
        </TouchableOpacity>
        <TouchableOpacity 
          onPress={() => decrement()} 
          style={styles.button}>
          <Text style={styles.buttonTitle}>-</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  button: {
    backgroundColor: '#ff5f00',
    width: 45,
    justifyContent: 'center',
    alignItems: 'center',
    marginVertical: 8,
    marginHorizontal: 8,
  },
  buttonTitle: {
    fontSize: 34,
    color: '#FFF',
  },
  counterText: {
    fontSize: 24,
  },
  group: {
    flexDirection: 'row',
  },
});export default () => (
  <CounterContextProvider>
    <App />
  </CounterContextProvider>
);
```

谢谢，别忘了在 [**Twitter**](https://twitter.com/useEffectReact) 上关注我，你可以对这篇文章发表评论或提出问题。