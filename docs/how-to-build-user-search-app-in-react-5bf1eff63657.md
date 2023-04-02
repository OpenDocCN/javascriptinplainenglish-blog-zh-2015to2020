# 如何在 React 中构建用户搜索 App

> 原文：<https://javascript.plainenglish.io/how-to-build-user-search-app-in-react-5bf1eff63657?source=collection_archive---------4----------------------->

## 创建具有高效排序和过滤功能的应用程序

![](img/2f5ea014e907ba0cd3406f2fcb665fcc.png)

Search App

在本文中，我们将使用 React 创建一个添加了排序和过滤功能的搜索应用程序。

你可以在这里看到应用程序[的现场演示](https://user-search-app.now.sh/)

*我们将在本文中使用 React 钩子，如果你不熟悉它，请查看本文*[](https://levelup.gitconnected.com/an-introduction-to-react-hooks-50281fd961fe?source=friends_link&sk=89baff89ec8bc637e7c13b7554904e54)**以获得 React 钩子的介绍。**

## *我们开始吧*

*通过运行以下命令创建一个新的 React 项目*

```
*create-react-app user-search-app*
```

*项目创建完成后，删除`src`文件夹中的所有文件，并在`src`文件夹中创建`index.js`文件。同样，在`src`文件夹中创建`css`、`components,`、`reducers,`、`store`文件夹。*

*安装所需的 npm 软件包*

```
*yarn add axios@0.19.2 bootstrap@4.5.0 react-bootstrap@1.0.1 lodash@4.17.15 node-sass@4.14.1 react-redux@7.2.0 react-select@3.1.0 redux@4.0.5 redux-thunk@2.3.0*
```

*在`css`文件夹中创建一个新文件`styles.scss`，并添加以下代码*

```
*$body-background-color: #212a38;
$body-color: #949ca5;
$border-color: #080a0e;

html {
  font-size: 62.5%;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-size: 1.6rem;
  background-color: $body-background-color;
  color: $body-color;
  min-height: 100vh;
  letter-spacing: 1px;
}

.header {
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2rem;

  &__title {
    font-weight: 600;
    font-size: 3rem;
    color: #fff;
  }

  &__search {
    input {
      background: lighten($body-background-color, 10%);
      margin-left: 10rem;
      border: 1px solid lighten($body-background-color, 10%);
      padding: 1rem 2rem;
      border-radius: 10rem;
      width: 25rem;
      outline: none;
      color: $body-color;
    }
  }
}

.sortBy {
  border: 2px solid #566273;
  padding: 1rem;
  max-width: 25rem;
  margin: auto;

  .select-filter {
    margin-top: 1rem;
  }
}

.loading {
  text-align: center;
  margin-top: 10rem;
  font-size: 2rem;
}

.users-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 2rem;

  .user {
    width: 30rem;
    margin: 1rem;
    background: $body-background-color;
    padding: 0.8rem;
    box-shadow: 0 2px 7px 3px $body-color;

    &__name {
      font-size: 2rem;
      color: #fff;
      letter-spacing: 1px;
    }

    &__image {
      width: 100%;
      height: auto;
      border: none;
      margin-bottom: 2rem;
    }

    &__details {
      span {
        display: block;
        padding: 0;
        margin: 0;
        line-height: 1.5;
      }
    }
  }
}

@media only screen and (max-width: 600px) {
  .header {
    flex-direction: column;

    &__title {
      margin-bottom: 1rem;
    }

    &__search {
      input {
        margin-left: 0;
        margin-top: 1rem;
      }
    }
  }
}*
```

*在`public`文件夹中添加新的`users.json`文件，代码来自[这里的](https://github.com/myogeshchavan97/user-search-app/blob/master/public/users.json)*

*在`actions`文件夹中创建一个新文件`users.js`，并添加以下代码:*

```
*import axios from 'axios';

export const loadUsers = () => {
  return async (dispatch) => {
    try {
      const users = await axios.get('./users.json');
      return dispatch(setUsers(users.data));
    } catch (error) {
      console.log('error:', error);
    }
  };
};

export const setUsers = (users) => ({
  type: 'SET_USERS',
  users
});*
```

*在`components`文件夹中创建一个新文件`Header.js`，并添加以下代码:*

```
*import React from 'react';

const Header = ({ handleSearch }) => {
  return (
    <header className="header">
      <div className="header__title">User Search App</div>
      <div className="header__search">
        <input
          type="search"
          placeholder="Search users by country"
          onChange={handleSearch}
        />
      </div>
    </header>
  );
};

export default Header;*
```

*在`components`文件夹中创建一个新文件`Filters.js`，并添加以下代码:*

```
*import React from 'react';
import Select from 'react-select';

const Filters = ({ handleSort, sortOrder }) => {
  const options = [
    { value: '', label: 'None' },
    { value: 'asc', label: 'Ascending' },
    { value: 'desc', label: 'Descending' }
  ];

  return (
    <div className="sortBy">
      Sort by age
      <Select
        value={sortOrder}
        className="select-filter"
        onChange={handleSort}
        options={options}
      />
    </div>
  );
};

export default Filters;*
```

*在`components`文件夹中创建一个新文件`UserItem.js`并添加以下代码:*

```
*import React from 'react';
import { Card } from 'react-bootstrap';

const UserItem = ({ name, email, country, photo, gender, age }) => {
  return (
    <Card className="user">
      <Card.Img variant="top" src={photo} className="user__image" alt={name} />
      <Card.Body>
        <Card.Title className="user__name">{name}</Card.Title>
        <Card.Text className="user__details">
          <span>
            <strong>Email:</strong> {email}
          </span>
          <span>
            <strong>Country:</strong> {country}
          </span>
          <span>
            <strong>Gender:</strong> {gender}
          </span>
          <span>
            <strong>Age:</strong> {age}
          </span>
        </Card.Text>
      </Card.Body>
    </Card>
  );
};

export default UserItem;*
```

*在`components`文件夹中创建一个新文件`UsersList.js`，并添加以下代码:*

```
*import React from 'react';
import UserItem from './UserItem';

const UsersList = ({ users, isLoading }) => {
  return (
    <div className="users-list">
      {isLoading ? (
        <p className="loading">Loading...</p>
      ) : (
        <React.Fragment>
          {users.map((user, index) => (
            <UserItem key={index} {...user} />
          ))}
        </React.Fragment>
      )}
    </div>
  );
};

export default UsersList;*
```

*在`components`文件夹中创建一个新文件`HomePage.js`，并添加以下代码:*

```
*/* eslint-disable react-hooks/exhaustive-deps */
import React, { useState, useEffect, useRef } from 'react';
import _ from 'lodash';
import { connect } from 'react-redux';
import { loadUsers } from '../actions/users';
import UsersList from './UsersList';
import Header from './Header';
import Filters from './Filters';

const HomePage = (props) => {
  const [users, setUsers] = useState(props.users);
  const [sortOrder, setSortOrder] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const inputRef = useRef();

  // call api to get list of users
  useEffect(() => {
    setIsLoading(true);
    props.dispatch(loadUsers());

    // initialize debounce function to search once user has stopped typing every half second
    inputRef.current = _.debounce(onSearchText, 500);
  }, []);

  useEffect(() => {
    if (props.users.length > 0) {
      setUsers(props.users);
      setIsLoading(false);
    }
  }, [props.users]);

  function onSearchText(text, props) {
    let filtered;
    if (text) {
      filtered = props.users.filter((user) =>
        user.country.toLowerCase().includes(text.toLowerCase())
      );
    } else {
      filtered = props.users;
    }
    setUsers(filtered);
    setSortOrder('');
  }

  function handleSearch(event) {
    inputRef.current(event.target.value, props);
  }

  function handleSort(sortOrder) {
    setSortOrder(sortOrder);
    if (sortOrder.value) {
      setUsers(_.orderBy(users, ['age'], [sortOrder.value]));
    }
  }

  return (
    <React.Fragment>
      <Header handleSearch={handleSearch} />
      <Filters handleSort={handleSort} sortOrder={sortOrder} />
      <UsersList users={users} isLoading={isLoading} />
    </React.Fragment>
  );
};

const mapStateToProps = (state) => ({
  users: state.users
});

export default connect(mapStateToProps)(HomePage);*
```

*在`reducers`文件夹中创建一个新文件`users.js`，并添加以下代码:*

```
*const usersReducer = (state = [], action) => {
  switch (action.type) {
    case 'SET_USERS':
      return [...state, ...action.users];
    default:
      return state;
  }
};

export default usersReducer;*
```

*在`store`文件夹中创建一个新文件`store.js`，并添加以下代码:*

```
*import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
import usersReducer from '../reducers/users';

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
  combineReducers({
    users: usersReducer
  }),
  composeEnhancers(applyMiddleware(thunk))
);

export default store;*
```

*在`src/index.js`内部添加以下代码:*

```
*import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store/store';
import HomePage from './components/HomePage';
import 'bootstrap/dist/css/bootstrap.min.css';
import './css/styles.scss';

ReactDOM.render(
  <Provider store={store}>
    <HomePage />
  </Provider>,
  document.getElementById('root')
);*
```

*现在，通过从终端运行`yarn start`命令来启动应用程序，一切就绪*

*![](img/8057eec3b01d3ce2d64eaf734000f791.png)*

*现在让我们来理解代码。*

*在`HomePage.js`中，在第一个`useEffect`调用中，我们调用`loadUsers`动作生成器方法来获取用户列表。*

*因为我们已经通过传递`mapStateToProps`将`HomePage`组件附加到了`react-redux`的`connect`方法，所以每当 redux 存储发生变化时，`users`对象将被填充一个用户列表，我们可以使用`props.users`在组件中访问该列表*

```
*const mapStateToProps = (state) => ({
 users: state.users
});*
```

*因此，第二个`useEffect`将被调用，因为`users`属性已经更改，它将使用用户列表更新状态*

```
*useEffect(() => {
 if (props.users.length > 0) {
  setUsers(props.users);
  setIsLoading(false);
 }
}, [props.users]);*
```

*在`Filters.js`中，我们正在使用`react-select`库中的`Select`组件来显示下拉列表，以便按年龄排序，并且一旦 use 从下拉列表中选择了任何值，我们就调用在`HomePage.js`中定义的作为道具传递的`handleSort`方法*

```
*function handleSort(sortOrder) {
 setSortOrder(sortOrder);
 if (sortOrder.value) {
  setUsers(_.orderBy(users, ['age'], [sortOrder.value]));
 }
}*
```

*这里，`sortOrder`参数将包含格式为`{ value: ‘asc’, label: ‘Ascending’ }`的对象*

*我们通过调用`setSortOrder`方法将这个值设置为`sortOrder`状态对象。如果选择的值不为空，那么我们调用由`lodash`提供的`orderBy`方法。`orderBy`方法接受*

*1.我们要排序的那个`collection(array or object)`*

*2.应该用于排序的字段*

*3.升序排序顺序`asc`和降序排序顺序`desc`如下*

```
*_.orderBy(users, ['age'], ['asc'])*
```

*所有的`lodash`方法都针对性能进行了优化，因此与手动排序相比，排序总是更快。*

*如果我们在不使用`lodash`库的情况下编写排序功能，排序将如下所示*

```
*function handleSort(sortOrder) {
    setSortOrder(sortOrder);
    const order = sortOrder.value;
    if (order == 'asc') {
      setUsers(
        users.slice().sort((firstUser, secondUser) => {
          if (firstUser.age < secondUser.age) return -1;
          if (firstUser.age > secondUser.age) return 1;
          return 0;
        })
      );
    } else if (order == 'desc') {
      setUsers(
        users.slice().sort((firstUser, secondUser) => {
          if (firstUser.age < secondUser.age) return 1;
          if (firstUser.age > secondUser.age) return -1;
          return 0;
        })
      );
    }
  }*
```

*这是更多的代码，也可能没有`lodash`的`orderBy`方法快。*

*如果你想了解更多关于数组排序的知识，请查阅我的前一篇文章。*

***搜索功能:***

*如果我们正在进行 API 请求，那么为了避免对每个键入的字符进行 API 请求，我们可以使用由`lodash`提供的`debounce`方法。*

*`debounce`方法允许我们在几毫秒后调用一个函数。如果有大量数据，这将避免应用程序变慢。因此，一旦用户停止输入，我们将在 500 毫秒后进行 API 调用。*

*为了通过演示了解普通搜索和去抖搜索的区别，请点击这里查看我以前的文章*

*在`HomePage.js`中，当组件在`useEffect`调用中被挂载时，我们只初始化了一次`debounce`方法，因为空数组`[]`作为第二个参数被传递。*

```
*inputRef.current = _.debounce(onSearchText, 500);*
```

*`debounce`方法返回一个我们存储在`inputRef.current`中的函数，并在`handleSearch`处理程序中调用它，当用户在输入搜索文本框中键入时，这个处理程序就会被调用。当我们调用`inputRef.current`方法时，它在内部调用`onSearchText`方法，在那里我们基于输入搜索过滤掉用户。*

*你可以在这里找到这篇文章[的完整源代码，在这里](https://github.com/myogeshchavan97/user-search-app)找到现场演示*

*今天到此为止。我希望你学到了新东西。*

*不要忘记订阅我的每周简讯，里面有惊人的技巧、窍门和文章，直接在这里的收件箱里。*