## 安装 Webpack 2

全局安装：

```
npm install webpack -g
```

我们也可以将其安装在本地的一个项目中。

```
npm install webpack --save-dev
```

然后执行 `./node_modules/.bin/webpack --version` 来查看这个项目内的 webpack 的版本，我的是 "2.2.1"。

## Tree Shaking

什么是 Tree Shaking？一言蔽之，就是在项目打包过程中排除那些没有使用到的代码从而达到清理代码目的的技术。

### 什么是 Tree Shaking

### 如何在 Webpack 中配置 Tree Shaking

### Tree Shaking 和 Dead code elimination
