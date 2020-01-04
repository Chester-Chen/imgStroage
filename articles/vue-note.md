---
date: 2019/1/4 21:54:17 
title: vue笔记
categories:
 - vue
 - js
tags: 
 - vue
 - js
---
## v-if和v-show区别

- v-if  会删除dom

- v-show dom占据原来位置，，只是隐藏了
<!--more-->

- 当渲染`<p>{{ msg }}<p/>`时，页面会出现`{{ msg }}`,
为了解决闪烁问题，，在css中加入`[v-cloak] { display: none; }`

## v-on 事件修饰符

- .stop 阻止冒泡
- .prevent 阻止默认事件，如：a标签的跳转href属性
- .capture 事件的捕获模式
- .self 只当事件在该元素本身触发时触发回调
- .once 事件只触发一次

**.self 只会阻止自己身上的冒泡行为，不会阻止冒泡过程的全过程**

## v-model
只能应用于表单标签

## v-bind:class
eg.
```
<style>
	xxx {

	}
</style>

<h1 :class="['xxx','xxx',flag?'xxx'：'xxx']">hello</h1>

var vm = new Vue({
	el: '#app',
	data: {
		flag: true;
	},
	methods: {}
	
});
```
## v-for

在遍历数组时，`list: [234, 4,35,55,6,5654,7647,6,7]` 
```
<p v-for="(item, i) in list"></p>
ps: i为索引值
```
在遍历对象时，`data: { user: 'chester', age: '124' }`
```
<p v-for="(item,vlaue, i) in list"></p>
ps: i为索引值
```
:key属性，，一般绑定id具唯一性，key的值只能是string或者number

## <transition-group>列表过渡

例如使用到v-for时，渲染整个列表，需要使用`<transition-group></transition-group>`

- 不同于 `<transition>`，它会以一个真实元素呈现：默认为一个 <span>。你也可以通过 tag 特性更换为其他元素
- 可在`<transition-group tag="ul">`中用tag来替换默认的span,其中的ul也可是其他标签
- `<transition-group appear tag="ul">`添加appear可以在初始渲染时，有过渡效果

## vue组件

### 组件化与模块化的区别：
- 模块化：是从代码逻辑的角度划分，方便分层开发。
- 组件化：是从ui界面的角度进行划分的，方便ui组件的重用

### 创建组件
注意：
- `Vue.component('组件的名称', { template:'<p>hello component</p>'})`，template中和i能有唯一的根节点元素。
- data 必须是一个function，且必须返回一个对象
```
Vue.component('组件的名称', {
 template:'<p>hello component</p>'
 data() {
     return {
                    
     }
 }
})
```

### 父组件向子组件传值

```
	<div id="app">
        
        <comp v-bind:parentmsg="msg"></comp>
    </div>

    <script>
       var vm =  new Vue({
            el: '#app',
            data: {
                msg: '父组件数据',
            },
            components: {
                comp: {
                    template: "<h1>这是子组件---------{{parentmsg}}</h1>",
                    props: ["parentmsg"]
                }
            }      
        })
  	</script>
```
通过v-bind父组件传过来的parentmsg属性得到父组件的msg，需在子组件的props中定义v-bind定义的属性名，且props是一个数组

## 路由
```
  		var login = {
            template: '<h1>登录组件</h1>'
        }
        var register = {
            template: '<h1>注册组件</h1>'
        }

        const router = new VueRouter({
            routes: [
				{
                    path: '/login',
                    component: login
                },
                {
                    path: '/register',
                    component: register
                }
            ]，
			linkActiveClass: 'myClass'
        })

		var vm = new Vue({
            el: '#app',
            router: router,  // 与router建立联系
        })
```
- <router-view></router-view> 可看作是占位符,根据路由渲染相对应的组件。eg.上面代码中的`	{path: '/login',component: login}`，/login对应的login组件，渲染该组件对应的内容
- <router-link to="/register" tag="p">注册</router-link> **默认使用a标签，可用tag转其他标签**
- 路由传参方式二
```
html:
<router-link to="/login?name=陈志文&age=88" tag="span">登录</router-link>

js:
 {
	path: '/login/:name/:age',
	component: login
 }
```

### linkActiveClass
在router配置里有linkActiveClass属性，可以自定义class，，也可用bs框架

### children实现路由嵌套
```
 		const router = new VueRouter({
            routes: [{
                    path: '/',
                    redirect: '/account'
                },
                {
                    path: '/account',
                    component: account,
                    children: [{
                            path: 'login',
                            component: login
                        },
                        {
                            path: 'register',
                            component: register
                        }
                    ]
                },

            ],
            linkActiveClass: 'myClass'
        })
```

## `watch`、`computed`和`methods`对比
1. `computed`属性结果会被缓存，除非属性发生变化，才会重新计算，主要当作属性来使用
2. `methods`表示一个具体的操作，主要书写业务逻辑
3. `watch`一个对象，键是需要观察的表达式，值是对应的回调函数，主要用于监听特定数据的变化，从而进行具体业务逻辑操作。可以用来监听一些虚拟元素，例如路由地址的变化。

## webpack

### webpack导入vue和script标签导入vue区别
**包的查找规则：**

	1. 找项目根目录有没有node_modules文件夹
	
	2. 在node_modules中根据包名找vue对应的文件夹
	
	3. 在vue文件夹中，找一个叫做 package.js 的包配置文件
	
	4. 在package.js 文件夹中，查找一个mian的属性
	
- 方式一 `import Vue from 'vue'`

在`webpack.config.js`中新增resolve节点
```
resolve: {
	alias: {
		"vue$": "vue/dist/vue.js"
	}
}
```

- 方式二 `import Vue from 'node_module/vue/dist/vue.js'`
 
### 注意：在用webpack使用vue渲染组件的时候，用render函数

### export default 与 export

es6中使用export default 暴露成员
```
export default {
	title: '小明'
}
```
- export default 向外暴露成员，可以用任意变量来接收
- 在一个模块中，exoprt default 只允许向外暴露一次
- 在一个模块中，可以同时使用export default 和 export 向外暴露成员

export var title = '李四'

- 使用export 向外暴露成员，只能使用 { } 的形式来接收，
- export可以向外暴露多个成员， 同时， 如果某些成员在import 的时候不需要，，可以不在 { } 中定义
- export 导出的成员，必须严格按照导出时候的名称，来使用 { } 来接收
- 如果需要起别名接收，，用 as
 
	`import { title as titleas } from './test.js'`

**注意：在操作元素时，，应该在dom树构建完成后，调用钩子函数mounted**