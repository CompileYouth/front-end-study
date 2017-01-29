## 安装 Webpack 2

目前 webpack 2 还处于 beta 版本，所以需要用 `@beta` 来指定安装 2.2 版本。

全局安装：

```
npm install webpack@beta -g
```

但是刚刚也说过，webpack 2 还不是最新稳定版本，所以将一个 beta 版本的软件安装在全局环境下并不是一个好的选择。因为现在的 webpack 2 也是个试用产品，所以我们可以将其安装在本地的一个项目中。

```
npm install webpack@beta --save-dev
```

然后执行 `./node_modules/.bin/webpack --version` 来查看这个项目内的 webpack 的版本，我的是 "2.2.0"。

## Tree Shaking

什么是 Tree Shaking？一言蔽之，就是在项目打包过程中排除那些没有使用到的代码从而达到清理代码目的的技术。

### 什么是 Tree Shaking

### 如何在 Webpack 中配置 Tree Shaking

### Tree Shaking 和 Dead code elimination
