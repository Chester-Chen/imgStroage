date: 2020.04.27

## setState 

`setState()` 会对一个组件的 `state` 对象安排一次更新，当 state 改变了，该组件就会重新渲染。

## state 和 props 的区别

props 和 state 都是普通的 js 对象， 都是用来保存信息的。
- props：传递给组件的(类似函数的形参)
- state：组件内部数据，用于自己管理的(函数内声明的变量)

## setState 是异步的

setState 什么时候是异步的？目前，在事件处理函数内部的 setState 是异步的。为什么不同步更新 this.state ？ 在重新渲染前，React 会“等待”，直到所有组件中的事件处理函数内调用的 setState() 完成之后。这样可以避免不必要的重新渲染来提升性能。

调用 setState 是异步的，所以当 this.setState 后，不会立即映射新的值。如果要基于当前的 state 计算出新的值，应该传递一个函数，而不是一个对象。

```js
export default class Demo extends Component {
  state = { a: 0 }
...
  increment = () => {
    const { a } = this.state;
    this.setState({ a: this.state.a + 1 });
    this.setState({ a: this.state.a + 1 });
  };
  ...
}
```

上述代码中，increment 的调用并不会使得 a 增加2。因为increment 函数是从 this.state.a 中读取数据的，但是React 不会更新立即更新 this.state.a 的值，直至组件重新渲染。所以在两次调用 this.setState 时，获取到的 a 的值都是 0 ，且两次调用都是将值设为 1

解决上面的问题，我们可以给 setState 传递一个函数而不是一个对象，传递函数可以让函数访问到当前的state 的值。因为 setState 的调用是分批的，所以可以链式的更新，并确保它们是一个建立在另一个之上的，这样才不会冲突。

```js
increment() {
  this.setState((state) => {
    // 重要：在更新的时候读取 `state`，而不是 `this.state`。
    return {a: state.a + 1}
  });
}

handleSomething() {
  // 假设 `this.state.a` 从 0 开始。
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();

  // 如果你现在在这里读取 `this.state.a`，它还是会为 0。
  // 但是，当 React 重新渲染该组件时，它会变为 3。
}

```

## 对比函数组件中 useState

```js
 setCount((count) => {
      console.log('count :>> ', count);
      return count + 1;
 });
```

setCount 传递一个函数的话，和类组件效果一致，参数 `count` 是最新的state值。

**不同的是**函数组件中的 useState 不会像类组件中的 setState 一样自动合并更新对象 ，不过可以手动返回一个新的 对象，可以用 `Object.assign`  或者 `spread` 运算符

```js
const [state, setState] = useState({});
setState(prevState => {
  // 也可以使用 Object.assign
  return {...prevState, ...updatedValues};
});
```

### 惰性初始 state

`useState(initialState)`, 这个 `initialState` 只会在组件初始渲染中起作用，**后续渲染会被忽略**。初始化state 若要进行复杂的计算获得，可以传入一个函数， 在函数中计算并返回初始 state，此函数也是只在初始渲染时被调用。 

```js
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```