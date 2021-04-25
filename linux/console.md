# 初访终端

> [主页](../README.md) > [Linux](index.md) > #2

___

按下Ctrl+Alt+T，开始我们的终端之旅。

先掌握这些，好在终端中生存下来：

## apt

apt是安装软件包用的。需要安装些啥的话，输入`sudo apt install xxxx`就行。

比如安装git:

```shell
sudo apt install git
```

Kali的话apt会自动导入国内源，但是如果是Ubuntu或者Debian的话apt安装可能会很慢，遇到apt速度太慢可以考虑[换源](https://blog.csdn.net/c417469898/article/details/106412160/)。

如果是像vscode这种deb包不在apt源提供的软件的话可以通过`sudo dpkg -i` + 软件包文件名进行安装。

想要卸载已经安装的软件包，用`sudo apt remove` + 软件名。

## man

man命令可以用来获取帮助。比如想得到apt的帮助信息可以输入`man apt`。

## ls

ls命令用来显示当前文件夹下面的文件和文件夹。

## pwd

pwd命令用来显示当前所在目录的完整路径。

## cd

cd命令用来切换当前目录。返回上一级用`cd ..`。

___

现在差不多能够在终端过活一会了~

___

上一篇：[定制自己的桌面环境](kde.md)
下一篇：[折腾Vim](vim.md)
