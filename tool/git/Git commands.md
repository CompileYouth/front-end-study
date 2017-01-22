## 将本地项目上传至 github

本地项目根目录：

```
git init

git add -A

git commit -m 'Git init'
```

在 Github 上新建项目，复制项目的 URL。

本地项目根目录：

```
git remote add origin <URL>

git pull origin master
```

执行完 `git pull` 后常常会出现 "refusing to merge unrelated histories" 的问题，执行 `git pull origin master --allow-unrelated-histories` 可以解决。

最后：

```
git push -u origin master
```
