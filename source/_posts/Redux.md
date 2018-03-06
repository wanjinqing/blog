---
title: Redux
date: 2018-03-05 16:10:29
tags:
---


### Redux应用场景 (多交互，多数据源)

1. 用户的使用方式复杂
2. 不同身份的用户有不同的使用方式（比如普通用户和管理员）
3. 多个用户之间可以协作
4. 与服务器大量交互，或者使用了WebSocket
5. View要从多个来源获取数据

### 设计思想

1. web应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态保存在一个对象里面。

### 1. Store

Store就是保存数据的地方，整个应用只能有一个Store。Redux 提供createStore这个函数，用来生成 Store。

```
import { createStore } from 'redux';
const store = createStore(fn);
```

### 2. State 

Store对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。
当前时刻的 State，可以通过store.getState()拿到。

```
import { createStore } from 'redux';
const store = createStore(fn);
const state = store.getState();
```

### 3. Action

State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。
Action 是一个对象。其中的type属性是必须的，表示 Action 的名称。其他属性可以自由设置。

```
const action = {
    type: 'ACTION_ONE',
    other: ''
}
```
改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。

### 4. store.dispatch()

store.dispatch()是 View 发出 Action 的唯一方法.

```
import { createStore } from 'redux';
const store = createStore(fn);
store.dispatch({
  type: 'ACTION_ONE',
  other: 'Learn Redux'
});
```

### 5. Reducer

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

```
import { createStore } from 'redux';
const reducer = function (state, action) {
    switch (action.type) {
        case 'ACTION_ONE': 
            return state;
        default:
            return 0;
    }
}
const store = createStore(reducer);
```
上面代码中，createStore接受 Reducer 作为参数，生成一个新的 Store。以后每当store.dispatch发送过来一个新的 Action，就会自动调用 Reducer，得到新的 State.

### 6. store.subscribe()

Store 允许使用store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。

```
import { createStore } from 'redux';
const store = createStore(reducer);
store.subscribe(listener);
```

只要把 View 的更新函数（对于 React 项目，就是组件的render方法或setState方法）放入listen，就会实现 View 的自动渲染。
store.subscribe方法返回一个函数，调用这个函数就可以解除监听。

```
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);
unsubscribe();
```

### 7. 工作流程

首先用户触发Action（store.dispatch(action)）,然后Store自动调用了Reducer，并且传入了两个参数，当前的State和Action，Reducer返回新的State。State一旦有变化，Store就会调用监听函数。监听函数通过store.getState()得到当前状态，这时候就可以触发重新渲染View。

```
// 创建reducer
const reducer = (state = 0, action) => {
    switch (action.type) {
        case 'INCREMENT': return state + 1;
        case 'DECREMENT': return state - 1;
        default: return state;
    }
};

// reducer作为参数，创建store
const store = createStore(reducer);

// 生产一个组件
class Counter extends Component {
    render () {
        return (
            <div>
                <h1>{this.props.value}</h1>
                <button onClick={this.props.onIncrement}>+</button>
                <button onClick={this.props.onDecrement}>-</button>
            </div>
        )
    }
}


const render = () => {
    ReactDOM.render(
        <Counter
            value={store.getState()}
            onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
            onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
        />,
        document.getElementById('root')
    );
};

// 初始化组件
render();

// 设置监听函数（每当state变化时就会调用render函数，更新组件）
store.subscribe(render);

```

### 8. Reducer的拆分

Redux 提供了一个combineReducers方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。
```
// 增加一个reducer
function todos(state = [], action) {
    switch (action.type) {
        case 'ADD_TODO':
            return state.concat([action.text])
        default:
            return state
    }
}
// reducer修改为
const reducer = combineReducers({
    todos,
    counter
})

// Counter组建修改传入属性
value={store.getState().counter}
```

### 9. 中间件

Action 发出以后，Reducer 立即算出 State，这叫做同步；Action 发出以后，过一段时间再执行 Reducer，这就是异步。怎么才能 Reducer 在异步操作结束后自动执行呢？这就要用到新的工具：中间件（middleware）。

中间件就是一个函数，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能。
```
let next = store.dispatch;
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action);
  next(action);
  console.log('next state', store.getState());
}
```
上面代码中，对store.dispatch进行了重定义，在发送 Action 前后添加了打印功能。这就是中间件的雏形。

　　中间件就是一个函数，对store.dispatch方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能。


```
import { applyMiddleware, createStore } from 'redux';
import createLogger from 'redux-logger'; // 中间件
const logger = createLogger();

const store = createStore(
  reducer,
  applyMiddleware(logger)
);

// createStore方法可以接受整个应用的初始状态作为参数，那样的话，applyMiddleware就是第三个参数了。
const store = createStore(
  reducer,
  initial_state,
  applyMiddleware(logger)
);

// 中间件的次序有讲究
const store = createStore(
  reducer,
  applyMiddleware(thunk, promise, logger)
);
```

### 10. 异步操作

理解了中间件以后，就可以处理异步操作了。
同步操作只要发出一种 Action 即可，异步操作的差别是它要发出三种 Action。
* 操作发起时的 Action
* 操作成功时的 Action
* 操作失败时的 Action

三种 Action 可以有两种不同的写法:
```
// 写法一：名称相同，参数不同
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }

// 写法二：名称不同
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

除了 Action 种类不同，异步操作的 State 也要进行改造，反映不同的操作状态。下面是 State 的一个例子。

```
let state = {
  // ... 
  isFetching: true,
  didInvalidate: true,
  lastUpdated: 'xxxxxxx'
};
```
上面代码中，State 的属性isFetching表示是否在抓取数据。didInvalidate表示数据是否过时，lastUpdated表示上一次更新时间。
* 操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染
* 操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染

### 11. redux-thunk 中间件

```
const fetchPosts = function (postTitle) {
    return function (dispatch, getState) {
        dispatch(requestPosts(postTitle));
        return fetch(`/some/API/${postTitle}.json`)
            .then(response => response.json())
            .then(json => dispatch(receivePosts(postTitle, json)));
    }
}
// 使用方法一
store.dispatch(fetchPosts('reactjs'));
// 使用方法二
store.dispatch(fetchPosts('reactjs')).then(() =>
  console.log(store.getState())
);
```
上面代码中，fetchPosts是一个Action Creator（动作生成器），返回一个函数。这个函数执行后，先发出一个Action（requestPosts(postTitle)），然后进行异步操作。拿到结果后，先将结果转成 JSON 格式，然后再发出一个 Action（ receivePosts(postTitle, json)）。

上面代码中，有几个地方需要注意。
```
（1）fetchPosts返回了一个函数，而普通的 Action Creator 默认返回一个对象。
（2）返回的函数的参数是dispatch和getState这两个 Redux 方法，普通的 Action Creator 的参数是 Action 的内容。
（3）在返回的函数之中，先发出一个 Action（requestPosts(postTitle)），表示操作开始。
（4）异步操作结束之后，再发出一个 Action（receivePosts(postTitle, json)），表示操作结束。
```
这样的处理，就解决了自动发送第二个 Action 的问题。但是，又带来了一个新的问题，Action 是由store.dispatch方法发送的。而store.dispatch方法正常情况下，参数只能是对象，不能是函数。
这时，就要使用中间件redux-thunk。

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import reducer from './reducers';

// Note: this API requires redux@>=3.1.0
const store = createStore(
  reducer,
  applyMiddleware(thunk)
);
```
上面代码使用redux-thunk中间件，改造store.dispatch，使得后者可以接受函数作为参数。
因此，异步操作的第一种解决方案就是，写出一个返回函数的 Action Creator，然后使用redux-thunk中间件改造store.dispatch。

### 12. redux-promise 中间件

既然 Action Creator 可以返回函数，当然也可以返回其他值。另一种异步操作的解决方案，就是让 Action Creator 返回一个 Promise 对象。
这就需要使用redux-promise中间件。

```
import { createStore, applyMiddleware } from 'redux';
import promiseMiddleware from 'redux-promise';
import reducer from './reducers';

const store = createStore(
  reducer,
  applyMiddleware(promiseMiddleware)
); 
```
这个中间件使得store.dispatch方法可以接受 Promise 对象作为参数。这时，Action Creator 有两种写法。
写法一，返回值是一个 Promise 对象。
```
const fetchPosts = 
  (dispatch, postTitle) => new Promise(function (resolve, reject) {
     dispatch(requestPosts(postTitle));
     return fetch(`/some/API/${postTitle}.json`)
       .then(response => {
         type: 'FETCH_POSTS',
         payload: response.json()
       });
});
```
写法二，Action 对象的payload属性是一个 Promise 对象。这需要从redux-actions模块引入createAction方法，并且写法也要变成下面这样。
```
import { createAction } from 'redux-actions';

class AsyncApp extends Component {
  componentDidMount() {
    const { dispatch, selectedPost } = this.props
    // 发出同步 Action
    dispatch(requestPosts(selectedPost));
    // 发出异步 Action
    dispatch(createAction(
      'FETCH_POSTS'
      , 
      fetch(`/some/API/${postTitle}.json`)
        .then(response => response.json())
    ));
  }
```
上面代码中，第二个dispatch方法发出的是异步 Action，只有等到操作结束，这个 Action 才会实际发出。注意，createAction的第二个参数必须是一个 Promise 对象。

### 13. react-redux

这个库是可以选用的。实际项目中，你应该权衡一下，是直接使用 Redux，还是使用 [React-Redux](https://github.com/reactjs/react-redux)。
后者虽然提供了便利，但是需要掌握额外的 API，并且要遵守它的组件拆分规范。

##### 13.1 UI组件

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。
* 只负责 UI 的呈现，不带有任何业务逻辑
* 没有状态（即不使用this.state这个变量）
* 所有数据都由参数（this.props）提供
* 不使用任何 Redux 的 API

##### 13.2 容器组件

* 负责管理数据和业务逻辑，不负责 UI 的呈现
* 带有内部状态
* 使用 Redux 的 API

UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。

##### 13.3 connect

React-Redux 提供connect方法，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来。
```
import { connect } from 'react-redux'
const VisibleTodoList = connect()(TodoList);
```
上面代码中，TodoList是 UI 组件，VisibleTodoList就是由 React-Redux 通过connect方法自动生成的容器组件。
但是，因为没有定义业务逻辑，上面这个容器组件毫无意义，只是 UI 组件的一个单纯的包装层。为了定义业务逻辑，需要给出下面两方面的信息。
（1）输入逻辑：外部的数据（即state对象）如何转换为 UI 组件的参数
（2）输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去。

因此，connect方法的完整 API 如下。

```
import { connect } from 'react-redux'

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```
上面代码中，connect方法接受两个参数：mapStateToProps和mapDispatchToProps。它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将state映射到 UI 组件的参数（props），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。

##### 13.4 mapStateToProps()

mapStateToProps是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）state对象到（UI 组件的）props对象的映射关系。
作为函数，mapStateToProps执行后应该返回一个对象，里面的每一个键值对就是一个映射。请看下面的例子.

```
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
    default:
      throw new Error('Unknown filter: ' + filter)
  }
}
```
mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

mapStateToProps的第一个参数总是state对象，还可以使用第二个参数，代表容器组件的props对象。

connect方法可以省略mapStateToProps参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。

##### 13.5 mapDispatchToProps()

mapDispatchToProps是connect函数的第二个参数，用来建立 UI 组件的参数到store.dispatch方法的映射。
也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。

如果mapDispatchToProps是一个函数，会得到dispatch和ownProps（容器组件的props对象）两个参数。
```
const mapDispatchToProps = (
  dispatch,
  ownProps
) => {
  return {
    onClick: () => {
      dispatch({
        type: 'SET_VISIBILITY_FILTER',
        filter: ownProps.filter
      });
    }
  };
}
```
从上面代码可以看到，mapDispatchToProps作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action。
如果mapDispatchToProps是一个对象，它的每个键名也是对应 UI 组件的同名参数，键值应该是一个函数，会被当作 Action creator ，返回的 Action 会由 Redux 自动发出。举例来说，上面的mapDispatchToProps写成对象就是下面这样。
```
const mapDispatchToProps = {
  onClick: (filter) => {
    type: 'SET_VISIBILITY_FILTER',
    filter: filter
  };
}
```

##### 13.6 <Provider\> 组件
connect方法生成容器组件以后，需要让容器组件拿到state对象，才能生成 UI 组件的参数。
一种解决方法是将state对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将state传下去就很麻烦。
React-Redux 提供Provider组件，可以让容器组件拿到state。
```
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp);

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
上面代码中，Provider在根组件外面包了一层，这样一来，App的所有子组件就默认都可以拿到state了。

它的原理是React组件的context属性，请看源码。

```
class Provider extends Component {
  getChildContext() {
    return {
      store: this.props.store
    };
  }
  render() {
    return this.props.children;
  }
}

Provider.childContextTypes = {
  store: React.PropTypes.object
}
```
上面代码中，store放在了上下文对象context上面。然后，子组件就可以从context拿到store，代码大致如下。
```
class VisibleTodoList extends Component {
  componentDidMount() {
    const { store } = this.context;
    this.unsubscribe = store.subscribe(() =>
      this.forceUpdate()
    );
  }

  render() {
    const props = this.props;
    const { store } = this.context;
    const state = store.getState();
    // ...
  }
}

VisibleTodoList.contextTypes = {
  store: React.PropTypes.object
}
```

##### 13.7 实例

UI组件

```
class Counter extends Component {
  render() {
    const { value, onIncreaseClick } = this.props
    return (
      <div>
        <span>{value}</span>
        <button onClick={onIncreaseClick}>Increase</button>
      </div>
    )
  }
}
```
上面代码中，这个 UI 组件有两个参数：value和onIncreaseClick。前者需要从state计算得到，后者需要向外发出 Action。
接着，定义value到state的映射，以及onIncreaseClick到dispatch的映射。
```
function mapStateToProps(state) {
  return {
    value: state.count
  }
}
function mapDispatchToProps(dispatch) {
  return {
    onIncreaseClick: () => dispatch(increaseAction)
  }
}
// Action Creator
const increaseAction = { type: 'increase' }
```
然后，使用connect方法生成容器组件。

```
const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter)
```

然后，定义这个组件的 Reducer。
```
// Reducer
function counter(state = { count: 0 }, action) {
  const count = state.count
  switch (action.type) {
    case 'increase':
      return { count: count + 1 }
    default:
      return state
  }
}
```
最后，生成store对象，并使用Provider在根组件外面包一层。

```
import { loadState, saveState } from './localStorage';

const persistedState = loadState();
const store = createStore(
  todoApp,
  persistedState
);

store.subscribe(throttle(() => {
  saveState({
    todos: store.getState().todos,
  })
}, 1000))

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```
完整的代码看这里
[https://github.com/wanjinqing/redux](https://github.com/wanjinqing/redux)

使用React-Router的项目，与其他项目没有不同之处，也是使用Provider在Router外面包一层，毕竟Provider的唯一功能就是传入store对象。
```
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
```