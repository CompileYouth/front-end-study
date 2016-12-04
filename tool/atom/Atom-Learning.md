# Atom -- 插件

## 必装

- `git-control`： atom 中的 git 可视化工具
- `platformio-ide-terminal`： 在 atom 直接打开 terminal

## 提升速度

- `emmet`： 在 atom 中使用 emmet
- `autocomplete-paths`： 自动补全路径
- `autocomplete-modules`： require/import 时自动补全模块名
- `recent-projects`： 打开最近文件夹

## 显示

- `file-icons`： 一眼看穿文件类型
- `auto-fold`： 代码自动折叠
- `color-picker`： 查看颜色
- `pigments`： 实时显示颜色
- `activate-power-mode`： 装逼必备
- `minimap`： 骚年，你还记得 sublime 右上角的预览面板吗


# Atom -- 常见问题解决

## 快捷键冲突

1. 打开 Settings -> Keybingdings；
2. 复制目标快捷键的配置信息，如下图所示，目标是将 `ctrl + alt + o` 快捷键配置为打开或关闭 `git-control`；

    ![](./res/shortcut-conflict.png)

3. 打开 "keymap.cson"(ctrl + shift + p, type "open keymap")；
4. 粘贴配置信息至文件末尾。

## 隐藏特定文件或文件夹

我们在 Settings 的 Ignored Names 中添加的文件或文件夹并不会在左侧文件栏中隐藏，需要我们额外设置。

1. 打开 Settings -> Packages；
2. 找到 tree-view；
3. 勾选 "Hide Ignored Names"，搞定。



# Atom -- 快捷键 Cheat Sheet

### 注： 请谨慎参考，部分插件的快捷键已被修改。


|       快捷键        |         功能          |
| :----------------- | :-------------------- |
| ctrl + alt + o     | toggle git-control    |
| ctrl + shift + o   | open recent-projects  |
