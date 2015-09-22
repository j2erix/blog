# Wizard-XEBS项目中开发一个组件


### 初始化组件
mkdir ./src/components/example
vim ./src/components/example/index.js

```
import React, {Component} from 'react';

export default class Example extends Component {
    render() {
        return (
            <div>I'm a example component.</div>
        );
    }
}
```

### 在需要的地方引入组件，并传入props
vim ./containers/other/template.jsx

```
// react
import React from 'react';
import Chrome from '../../components/chrome/index.js';
import Example from '../../components/example/index.js';

const render = function() {
    return (
        <Chrome>
            <h1>I am other page</h1>
            <Example {...this.props}/>
        </Chrome>
    );
};

export default render;
```

### 创建同步action creator，并在index中引入
vim ./src/actions/example.js

```
'use strict';

export const SELECT_REDDIT = 'SELECT_REDDIT';
export const INVALIDATE_REDDIT = 'INVALIDATE_REDDIT';
export const REQUEST_POSTS = 'REQUEST_POSTS';
export const RECEIVE_POSTS = 'RECEIVE_POSTS';

export const selectReddit = (reddit) => {
    return {
        type: SELECT_REDDIT,
        reddit
    };
};
export const invalidateReddit = (reddit) => {
    return {
        type: INVALIDATE_REDDIT,
        reddit
    };
};
export const requestPosts = (reddit) => {
    return {
        type: REQUEST_POSTS,
        reddit
    };
};
export const receivePosts = (reddit, json) => {
    return {
        type: RECEIVE_POSTS,
        reddit,
        posts: json.data.children.map(child => child.data),
        receivedAt: Date.now()
    };
};
```

vim ./src/actions/index.js

```
import * from './example'
```

### 重新规划state tree
重新规划state tree，好对reducers中进行编码

```
{
  selectedReddit: 'frontend',
  postsByReddit: {
    frontend: {
      isFetching: true,
      didInvalidate: false,
      items: []
    },
    reactjs: {
      isFetching: false,
      didInvalidate: false,
      lastUpdated: 1439478405547,
      items: [{
        id: 42,
        title: 'Confusion about Flux and Relay'
      }, {
        id: 500,
        title: 'Creating a Simple Application Using React JS and Flux Architecture'
      }]
    }
  }
}
```
### 添加state selector
这里使用了reselector，减少重复计算

vim ./src/selectors/exampleSelector.js

```
'use strict';

import {createSelector} from 'reselect';
const selectedRedditSelector = state => state.selectedReddit;
const postsByRedditSelector = state => state.postsByReddit;

export const exampleSelector = createSelector(
    selectedRedditSelector,
    postsByRedditSelector,
    (selectedReddit, postsByReddit) => {
        return {
            selectedReddit,
            postsByReddit
        };
    }
);
```

vim ./src/selectors/index.js

```
'use strict';

import {createSelector} from 'reselect';
import {todoAppSelector} from './todoSelector';
import {exampleSelector} from './exampleSelector';

export const appSelector = createSelector(
    todoAppSelector,
    exampleSelector,
    (todoApp, example) => {
        return {
            todoApp,
            example
        };
    }
);
```
注意，selector暴露出来的东西，决定了"Dump Components"能够接触到的Store的结构。

### 添加reducer，处理action
vim ./src/reducers/example.js

```
import {
    SELECT_REDDIT, INVALIDATE_REDDIT,
    REQUEST_POSTS, RECEIVE_POSTS
} from '../actions/index';

const selectedReddit = (state = 'reactjs', action) => {
    switch (action.type) {
    case SELECT_REDDIT:
        return action.reddit
    default:
        return state;
    }
};
const posts = (state = {
    isFetching: false,
    didInvalidate: false,
    items: []
}, action) => {
    switch (action.type) {
    case INVALIDATE_REDDIT:
        return Object.assign({}, state, {
            didInvalidate: true
        });
    case REQUEST_POSTS:
        return Object.assign({}, state, {
            isFetching: true,
            didInvalidate: false
        });
    case RECEIVE_POSTS:
        return Object.assign({}, state, {
            isFetching: false,
            didInvalidate: false,
            items: actions.posts,
            lastUpdated: action.receiveAt
        });
    default:
        return state;
    }
};
const postsByReddit(state = {}, action) {
    switch (action.type) {
    case INVALIDATE_REDDIT:
    case RECEIVE_POSTS:
    case REQUEST_POSTS:
        return Object.assign({}, state, {
            [action.reddit]: posts(state[action.reddit], action)
        });
    default:
        return state;
    }
};

export default const example = {
    postsByReddit,
    selectedReddit
});
```

vim ./src/reducers/index.js

```
import {combineReducers} from 'redux';
import todoApp from './todoApp';
import example from './example';

var reducerMap = Object.assign({}, todoApp, example)

const rootReducer = combineReducers(reducerMap);

export default rootReducer;
```

## 让组件异步获取数据

### 创建异步action creator
上面的action执行的都是同步的事情，为了依赖异步数据做一些事情，我们创建一个返回thunk的action。

vim ./src/actions/example.js

```
export const fetchPosts = (reddit) => {
    return dispatch => {

        dispatch(requestPosts(reddit));

        return fetch(`http://www.reddit.com/r/${reddit}.json`)
            .then(response => response.json())
            .then(json => dispatch(receivePosts(reddit, json)));
    };
};
```	

### 引入Redux Thunk middleware
Redux Thunk中间件能够处理返回thunk的action creator，执行这个thunk，并返回action object。

注意：这段不需要重复引入

vim ./src/app.jsx

```
import thunkMiddleware form 'react-thunk';
import {createStore, applyMiddleware} from 'redux';

const createStoreWithMiddleware = applyMiddleware(
    thunkMiddleware,
    loggerMiddleware
)(createStore);

const store = createStoreWithMiddleware(rootReducer);
```