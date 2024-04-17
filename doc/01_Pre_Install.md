# 开始之前

由于 UEFI 已普及数年，故本教程全部使用 UEFI+GPT 的形式进行，BIOS 方式自行解决。

## 终端编辑器 nano 的使用

在安装 Arch Linux 的过程中，终端编辑器的使用是不可避免的，如果你从来没有使用过终端编辑器，本节做一个简单的介绍，如果你使用过可以直接跳过本节。

与其它教程不同的是，本文推荐使用 nano 编辑器，vim 虽然是著名的终端编辑器，但是其上手难度较大，不适合新手第一次使用。如果你没有一个可用的 Linux 环境用于实践，这里推荐一个在线环境 [copy.sh](https://copy.sh/v86/?profile=archlinux)，由于其是在线环境，故性能较差，执行命令请耐心等待。如无特殊说明，本文终端编辑器默认为 nano。

```bash
nano test.txt		# 创建并编辑名为 test.txt 的文件
```

你可以看到进入了一个空的界面，下方有一些使用提示，这里提供一个翻译图片用作简单对照。其中`^`表示<kbd>Ctrl</kbd>键，比如`^G`表示<kbd>Ctrl</kbd>+<kbd>G</kbd>组合按键；`M`表示<kbd>Alt</kbd>键，比如`M-6`表示<kbd>Alt</kbd>+<kbd>6</kbd>组合按键。

![nano](./01_Pre_Install.assets/nano.png)

## 准备网络环境

