date: 2020.02.05
# vuecli3 打包空白问题

网上查过很多文章，大多都是在`vue.config.js`中将`publicPath`字段改为`"./"`, 否则各种资源都是`404`。
并且在`router`目录中，将`index.js`中的路由mode字段改为`hash`。

然后执行`npm run build`生成`dist`目录，把dist文件拉入`Hbuilder`，右键转换成APP应用，右键发行-打包原生安装包即可。

## 本地预览

`npm run build` 后生成的`dist`目录下的`index.html`直接打开是不会工作的。

这里官方文档给了下面的解释。

> `history`模式上线，服务端必须有对应模式
    根据 `vuecli `官方文档dist 目录需要启动一个 `HTTP` 服务器来访问 (除非你已经将 `publicPath` 配置为了一个相对的值)，
    所以以 file:// 协议直接打开 dist/index.html 是不会工作的。
    在本地预览生产环境构建最简单的方式就是使用一个 Node.js 静态文件服务器，例如 [serve](https://github.com/zeit/serve)(点我)。

```sh
npm install -g serve
# -s 参数的意思是将其架设在 Single-Page Application 模式下
# 这个模式会处理即将提到的路由问题
serve -s dist

```

就是说我们要在本地预览，必须在本地起一个服务，否则项目是无法正常工作的。

