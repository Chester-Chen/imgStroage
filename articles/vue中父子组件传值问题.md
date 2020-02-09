date: 2020.02.09

vue中，父组件通过props给子组件下发数据，子组件通过事件给父组件发消息。如下图
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/2020.02.09/01.jpeg?raw=true)


# 先来看看props传值-父传子

从官方API接口给了详细解释，如下。

props的类型：`Array<string> | Object`

> **详细：**
props 可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义验证和设置默认值。
你可以基于对象的语法使用以下选项：
- `type`: 可以是下列原生构造函数中的一种：String、Number、Boolean、Array、Object、Date、Function、- - `Symbol`、任何自定义构造函数、或上述内容组成的数组。会检查一个 prop 是否是给定的类型，否则抛出警告。Prop 类型的更多信息在此。
- `default`: any
为该 prop 指定一个默认值。如果该 prop 没有被传入，则换做用这个值。对象或数组的默认值必须从一个工厂函数返回。
- `required`: Boolean
定义该 prop 是否是必填项。在非生产环境中，如果这个值为 truthy 且该 prop 没有被传入的，则一个控制台警告将会被抛出。
- `validator`: Function
自定义验证函数会将该 prop 的值作为唯一的参数代入。在非生产环境下，如果该函数返回一个 falsy 的值 (也就是验证失败)，一个控制台警告将会被抛出。你可以在这里查阅更多 prop 验证的相关信息。

```javaScript
eg：
Vue.component('props-demo-advanced', {
  props: {
    // 检测类型
    height: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})

```

可以看到`props`支持`Function`，下面来尝试父组件传递方法给子组件。

```js
子组件
<template id="subcomponent">
    <input type='button' value='点击按钮触发父组件方法' @click='parentmsg'>  // 点击触发test()方法
</template>

...
.....
// 全局注册
 Vue.component('subcomponent', {
    template: '#subcomponent',
    props: ["parentmsg"]

})

```


```js
父组件
<div id="app">
    <subcomponent :parentmsg="test"></subcomponent>
</div>
...
.....
var vm = new Vue({
    el: '#app',
    data: {
        msg: '父组件数据',
    },
    methods: {
        test() {
            alert('传递父组件方法');
        }
    }
})


```

可以看到父组件中的`test`方法通过绑定到`parentmsg`属性上传递到了子组件中，子组件中必须使用`parentmsg`来接收。此外，通过props不仅可以接受对象，还可以接收数据。

# $emit-子传父
$emit 是 Vue 中的一个实例方法/事件，父组件可以使用 props 把数据传给子组件，子组件可以使用 $emit触发父组件的自定义事件。

来看看官方解释。

vm.$emit( eventName, […args] )

参数：

- {string} eventName  触发的事件名
- [...args]     
触发当前实例上的事件。附加参数都会传给监听器回调。

```js
子组件
<template id="subcomponent">
    // <input type='button' value='click' @click="test">  亦可
    <input type='button' value='click' @click="$emit('emit-func', this.name)">
</template>

// 全局注册
Vue.component('subcomponent', {
    template: '#subcomponent',
    props: ["parentmsg"],
    data() {
        return {
            name: 'Chester'
        }
    },
    methods: {
        test() {
            this.$emit("emit-fun", this.name);
        }
    },
})

```

子组件中绑定事件名，发射`emit-fun`事件,同时把子组件中的`name`当做参数传到父组件


```js
父组件
<div id="app">
    <subcomponent @emit-fun="parent"></subcomponent>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        msg: '父组件数据',
    },
    methods: {
        parent(sondata) {
            alert(`子组件传过来的值：${sondata}`);
        }
    }
})


```

父组件通过子组件发射的`emit-fun`事件，触发父组件的自定义parent方法，并用`sondata`接收子组件传的`name`。

