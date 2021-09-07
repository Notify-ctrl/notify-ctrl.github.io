# bash的运行环境

> [主页](../README.md) > [bash](./index.md) > #2

- [bash的运行环境](#bash的运行环境)
  - [shell的初始化文件](#shell的初始化文件)
    - [全局配置文件](#全局配置文件)
    - [用户个人配置文件](#用户个人配置文件)
  - [变量](#变量)
    - [变量类型](#变量类型)
    - [创建变量](#创建变量)
    - [export](#export)
    - [内部变量](#内部变量)
    - [特殊参数](#特殊参数)

## shell的初始化文件

### 全局配置文件

当执行`bash --login`或者`sh`时，shell会执行`/etc/profile`下面的语句。`/etc/profile`保存着所有用户共同的bash配置。

```bash
# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).

if [ "$(id -u)" -eq 0 ]; then
  PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
else
  PATH="/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games"
fi
export PATH

if [ "${PS1-}" ]; then
  if [ "${BASH-}" ] && [ "$BASH" != "/bin/sh" ]; then
    # The file bash.bashrc already sets the default PS1.
    # PS1='\h:\w\$ '
    if [ -f /etc/bash.bashrc ]; then
      . /etc/bash.bashrc
    fi
  else
    if [ "$(id -u)" -eq 0 ]; then
      PS1='# '
    else
      PS1='$ '
    fi
  fi
fi

if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
```

在我的电脑中，这个文件设置了PATH变量和提示符样式，执行了`/etc/bash.bashrc`和`/etc/profile.d`文件夹下面的所有脚本。

由于系统可能提供了许多shell，而`/etc/profile`是各种共用的配置文件，bash自己特定的配置需要保存在别的地方。在我的电脑中，bash全局配置在`/etc/bash.bashrc`中。

### 用户个人配置文件

一般来说是`~/.bashrc`。

___

## 变量

### 变量类型

习惯上来说，bash中的变量名称都是以大写字母组成。bash中有两大类变量：全局变量（所有shell都能使用）和局部变量（仅bash可用，其他shell无法确定）。

全局变量，也可称为环境变量，在所有shell都能使用。输入`env`命令或者`printenv`命令可以查看全局变量。

局部变量只在当前shell才能使用。使用不带任何参数的`set`命令可以查看局部变量（全局变量也会显示）。

除了把变量分为全局和局部两大类之外，我们也可以按照变量的内容进行分类。这么一来变量有4种类型：

- 数字
- 字符串
- 常量
- 数组

这些后面再说，我们目前知道数字和字符串就够了。

### 创建变量

按照习惯，变量的名字均由大写字母组成。当然实际上变量的命名规则与C语言是基本一致的。

```bash
notify@npc:~/bash_study$ _myvar="15"
notify@npc:~/bash_study$ echo $_myvar
15
```

要在shell中定义变量的话，可以使用

```bash
VARNAME="value"
```

注意了，等号两侧不能存在空格，否则会出错。为变量赋值时也可以不加双引号，但添加上引号总会是一种好习惯。

使用

```bash
unset VARNAME
```

可以清除对应变量。

### export

用上述方式定义的变量只会在当前shell生效，而无法影响shell的子进程。为了向子shell传播变量，我们需要用bash内置的`export`命令来*export* 它们，像这样：

```bash
export VARNAME="value"
```

子shell可以自由改变这种变量的值，但不会影响父shell。下面是一些例子：

```bash
notify@npc:~/bash_study$ MY_NAME="notify"
notify@npc:~/bash_study$ echo $MY_NAME
notify
notify@npc:~/bash_study$ bash
notify@npc:~/bash_study$ echo $MY_NAME

notify@npc:~/bash_study$ exit
exit
notify@npc:~/bash_study$ export MY_NAME
notify@npc:~/bash_study$ bash
notify@npc:~/bash_study$ echo $MY_NAME
notify
notify@npc:~/bash_study$ export MY_NAME="my_name"
notify@npc:~/bash_study$ echo $MY_NAME
my_name
notify@npc:~/bash_study$ exit
exit
notify@npc:~/bash_study$ echo $MY_NAME
notify
notify@npc:~/bash_study$ 
```

### 内部变量

各个shell都使用的内部变量:

变量名 | 功能
-|-
HOME | 主文件夹路径，cd命令默认目标，'~'符号展开用
IFS | 需要分开词时的分隔符
PATH | 用冒号分割的一系列文件夹，shell寻找命令的路径
PS1 | 主命令提示符
PS2 | 副命令提示符

bash特有内部变量：（略）

### 特殊参数

符号 | 功能
-|-
$*
$@
$#
$?
$-
\$$
$!
$0
$_
