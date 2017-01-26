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

## Tree-shaking

### 什么是 Tree-shaking

### 如何在 Webpack 中配置 Tree-shaking

### Tree-shaking 和 Dead code elimination
