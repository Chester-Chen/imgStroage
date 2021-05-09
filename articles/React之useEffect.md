date: 2020.04.28

## useEffect

`useEffect` 该 hook  接受一个包含命令式且可能有副作用的函数。 在函数主体内(指在React 渲染阶段)改变 DOM，添加订阅、设置定时器、记录日志、以及执行其他包含副作用的操作都是不被允许的。

副作用的函数：当调用函数时，除了返回值外，还会对主调函数产生附加的影响，例如，修改了其他作用域的变量的值等。可以设计并使用 Pure Function 减轻程序排查时的负担。

useEffect 函数会在组件渲染到屏幕之后执行。默认情况下，effect 会在每轮渲染结束后执行，但你可以选择让它在某些值改变时才执行，userEffect 的第二个参数接受一个数组， 该数组是一个依赖列表，只有其中的值在发生改变时，才会触发effect ，对于只需要第一次初始化才执行，可以传递空数组，空数组的意思就是告诉React 你的 effect 不依赖于 props 或 state 中的任何值。

```js
useEffect(() => {
    // do something
  }, [count]);
```

## effect 的执行时机

与 componentDidMount、componentDidUpdate 不同的是，传给 useEffect 的函数会在浏览器完成布局与绘制之后，在一个延迟事件中被调用。

虽然 useEffect 会在浏览器绘制后延迟执行，但会保证在任何新的渲染前执行。在开始新的更新前，**React 总会先清除上一轮渲染的 effect**，这里就可以利用这一性质取消订阅及事件处理等操作。

## 清除 useEffect

```js
export default function DemoCount(props) {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // 相当于 componentDidMount
    console.log('count rerender');
    console.log('==========================');
    return () => {
      // 这个返回的函数类似于 componentWillUnmount 
      console.log('effect unmount count:', count);
    };
  }, [count]);

  const increment = () => {
    setCount((count) => {
      console.log('count :>> ', count);
      return count + 1;
    });
  };

  const changeFlag = () => {};

  return (
    <div>
      <div>count: {count}</div>
      <button onClick={increment}>increment</button>
      <button onClick={changeFlag}>changeFlag</button>
    </div>
  );
}
// 下面是打印顺序
// count :>>  0
// Demo.jsx:10 effect unmount count: 0
// Demo.jsx:7 count rerender
// Demo.jsx:8 ==========================
```

可以看到在useEffect 执行前会先执行上次 effect 返回的函数，所以我们可以在该函数里做一些清理工作。

清除函数会在组件卸载前执行，如果组件多次渲染，则在执行下一个 effect 之前清除上一个 effect ，这样做的目的可以防止内存泄露。


