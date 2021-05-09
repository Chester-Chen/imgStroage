date: 2021.04.13

## 防抖与节流

### 节流

核心思想：在一段时间内，不管触发了几次事件，我都只认第一次触发，并且接下来给定的时效内不会被再次触发。
各位都玩过LOL把，就像技能进入 CD 冷却，需等待冷却时间结束才可继续释放技能。

```javascript
function throttle(fn, delay) {
    // 上一次触发回调时间
    let last = 0;

    return function () {
        // 保留调用时上下文
        let context = this;
        // 保留调用时传入的参数
        let args = arguments;
        // 本次调用的时间
        let now  = +new Date();

        // 若本次触发的时间间隔大于或等于 delay ，则执行回调
        if(now - last >= delay) {
            last = now;
            fn.apply(context, args);
        }
    }
}

const throttleHandle = throttle(() => console.log("throttle"), 2000);

document.addEventListener("scroll", throttleHandle);

```

### 防抖

核心思想：在某个时间段内，我只认最后一次触发。目的是为了防止用户开启 `狂暴效果` 疯狂乱点。

```javascript
function debounce(fn, delay) {
  // 定时器
  let timer = null;

  return function () {
    let context = this;
    let args = arguments;

    // 若用户疯狂乱点，则清除上一个定时器
    if (timer) {
      clearTimeout(timer);
    }
    // 设立新的定时器
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, delay);
  };
}

const debounceHandle = debounce(() => console.log("debounce"), 2000);

document.addEventListener("scroll", debounceHandle);
```

对于防抖的实现，不知道你们有没有发现有个问题，当我不停触发debounce时，只要我不停的 scroll ，就永远不会输出log。貌似和初衷有些许出入，我们需要实现，虽然防抖不停的被触发，但是在delay时间后，必须响应一次。


**下面基于 throttle 优化 debounce**

```javascript
function throttle(fn, delay) {
  // 上一次触发回调时间
  let last = 0,
    timer = null;

  return function () {
    // 保留调用时上下文
    let context = this;
    // 保留调用时传入的参数
    let args = arguments;
    // 本次调用的时间
    let now = +new Date();

    // 若本次触发的时间间隔小于 delay ，则设置新的定时器
    if (now - last < delay) {
      clearTimeout(timer);
      timer = setTimeout(() => {
        last = now; // 若回调成功回调，则重置 last 调用的时间值
        fn.apply(context, args);
      }, delay);
    } else {
      // 时间超出了delay间隔，必须给用户响应一次
      last = now;
      fn.apply(context, args);
    }
  };
}

const composeHandle = throttle(() => console.log("触发了滚动事件"), 1000);

document.addEventListener("scroll", composeHandle);
```

上面的栗子，在防抖的基础上加入 节流， 这样就可以在超过delay的阈值后，必须响应一次回调。