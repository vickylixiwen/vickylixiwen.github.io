---
layout: post
title:  "React + Redux 学习笔记"
categories: react,   redux
---

##### 记录一下学习React,   Redux的过程

推荐阅读：[React-Redux的用法](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

Redux is a pattern and library for managing and updating application state,   using events called "actions".

Redux 的工作流程：

1. 用户发出 Action   
2. 然后，Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State
3. State 一旦有变化，Store 就会调用监听函数。   
4. listener可以通过store.getState()得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。


Redux三要素：
store，action，reducer

A "store" is a container that holds your application's global state.


Actions​
An action is a plain JavaScript object that has a type field. You can think of an action as an event that describes something that happened in the application.

When an action is dispatched,   the store runs the root reducer function,   and lets it calculate the new state based on the old state and the action
Finally,   the store notifies subscribers that the state has been updated so the UI can be updated with the new data.

A reducer is a function that receives the current state and an action object,   decides how to update the state if necessary,   and returns the new state: (state,   action) => newState. You can think of a reducer as an event listener which handles events based on the received action (event) type.
Reducers act like event listeners,   and when they hear an action they are interested in,   they update the state in response.


React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。
因为不含有状态，UI 组件又称为"纯组件"，即纯函数一样，纯粹由参数决定它的值。

React-Redux规定，所有的UI组件都由用户提供，容器组件则是由React-Redux自动生成。也就是说，用户负责视觉层，状态管理则是全部交给它。React-Redux提供connect方法，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来。

connect方法接受两个参数：mapStateToProps和mapDispatchToProps。它们定义了UI组件的业务逻辑。前者负责输入逻辑，即将state映射到UI组件的参数（props），后者负责输出逻辑，即将用户对UI组件的操作映射成 Action。connect方法可以省略mapStateToProps参数，那样的话，UI组件就不会订阅Store，就是说Store的更新不会引起UI组件的更新。

mapStateToProps是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）state对象到（UI组件的）props对象的映射关系。mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染

mapDispatchToProps是connect函数的第二个参数，用来建立UI组件的参数到store.dispatch方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。

{% highlight react %}
const AppInitializer = connect(mapStateToProps,   mapDispatchToProps)(_AppInitializer);
{% endhighlight %}

`_AppInitializer`,   这个是个UI组件， `AppInitializer`这个就是个容器组件


React-Redux 提供Provider组件，可以让容器组件拿到state。


createSlice
The string from the name option is used as the first part of each action type,   and the key name of each reducer function is used as the second part. So,   the "counter" name + the "increment" reducer function generated an action type of {type: "counter/increment"}

Selectors are functions that know how to extract specific pieces of information from a store state value.


`useSelector`: read the initial data from the store
`useDispatch`: to dispatch the action to update the state in the store


