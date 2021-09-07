# 编写和调试bash脚本

> [主页](../README.md) > [bash](./index.md) > #1

- [编写和调试bash脚本](#编写和调试bash脚本)
  - [准备工作](#准备工作)
  - [编写第一个脚本](#编写第一个脚本)
  - [执行脚本](#执行脚本)
  - [注释](#注释)
  - [调试脚本](#调试脚本)
    - [调试整个脚本](#调试整个脚本)
    - [调试脚本的一部分](#调试脚本的一部分)

___

## 准备工作

有一个文本编辑器即可，我写bash脚本的话一般用vim，顺便推荐一个很不错的 [vim配置](https://github.com/amix/vimrc) 。

bash脚本是一个纯文本文档，但为了清楚起见应该带有 .sh拓展名。在命名的时候还应该给脚本起一个清晰的名字，例如 Minecraft 启动脚本被我命名为 mc .sh。

___

## 编写第一个脚本

首先打开终端，到主文件夹创建自己的文件夹和文件，然后开始编辑：

```bash
cd ~
mkdir bash_study && cd bash_study
touch study1.sh
vim study1.sh
```

之后进入了vim，按一下i键进入编辑模式，输入脚本内容：

```bash
#!/bin/bash

echo "Hello, $USER!"

COLOUR="green"
echo "This is a string: $COLOUR"
```

脚本写完了，按一下Esc键，输入:wq退出。

___

## 执行脚本

为了执行脚本，首先要给脚本添加可执行权限（确保自己处于脚本文件所在的文件夹中），然后直接执行即可。

```bash
notify@npc:~/bash_study$ chmod +x study1.sh
notify@npc:~/bash_study$ ls -l
总用量 4
-rwxr-xr-x 1 notify notify 84  9月  5 10:44 study1.sh
notify@npc:~/bash_study$ ./study1.sh
Hello, notify!
This is a string: green
notify@npc:~/bash_study$ echo $COLOUR

notify@npc:~/bash_study$ 
```

一般来说脚本都是这样执行的。在脚本文件的开头，有一行`#!/bin/bash`，表示当脚本文件被直接执行时，使用的shell为/bin/bash。（我自己日常使用的shell是zsh，与bash有颇多不同，但脚本指定了特定shell执行的话则没有影响）

像这样执行的时候，shell会fork一下，然后产生一个新的bash进程。我们脚本的内容在新的bash进程中执行。另外一种执行脚本的方式是像这样：

```bash
bash study1.sh
```

效果完全一致。从上面贴的运行结果可以看出，由于脚本在新的bash进程中运行，它定义的变量并不会在当前使用的bash中生效。如果你想在当前shell中直接执行脚本的话，可以：

```bash
notify@npc:~/bash_study$ source study1.sh
Hello, notify!
This is a string: green
notify@npc:~/bash_study$ echo $COLOUR
green
```

> 关于source命令：source相当于sh中的.（点）命令。可以开/bin/sh，分别运行`source study1.sh`和`. study1.sh`试试看。

___

## 注释

在各种shell脚本中，以"#"开头的内容会被当成注释。shell在执行的时候会忽略它们。

```bash
#!/bin/bash

# This is my first bash script
echo "Hello, $USER!"

COLOUR="green"  # define a variable named "COLOUR"
echo "This is a string: $COLOUR"
```

添加注释相当有必要，这样可以方便自己和他人阅读，避免造成许多不必要的麻烦。

___

## 调试脚本

任何程序总是难免出错，bash脚本也不例外，因此具有调试手段也十分必要。

### 调试整个脚本

一般来说在启动脚本时加上`-x`选项就可以达到调试整个脚本的效果。例如：

```bash
notify@npc:~/bash_study$ bash -x study1.sh
+ echo 'Hello, notify!'
Hello, notify!
+ COLOUR=green
+ echo 'This is a string: green'
This is a string: green
```

正如我们看到的，脚本的详细运行过程会被显示在屏幕上面。我们后来加的注释则完全不显示。

### 调试脚本的一部分

使用bash内置的`set`命令可以让我们正常运行没有错误的部分，而只对出错的部分进行调试。

```bash
#!/bin/bash

echo "Hello, $USER!"

set -x # activate debugging from here
COLOUR="green"
echo "This is a string: $COLOUR"
set +x # stop debugging
```

将会如此输出：

```bash
notify@npc:~/bash_study$ ./study1.sh 
Hello, notify!
+ COLOUR=green
+ echo 'This is a string: green'
This is a string: green
+ set +x
```

顺便再提一下set的几个常用用途，完整用途请自行`set --help`。

命令 | 结果
-|-
set -f | 对文件名禁用模式匹配
set -v | 读取 shell 输入行时将它们输出
set -x | 执行命令时打印它们和它们的参数

```bash
notify@npc:~/bash_study$ set -v
notify@npc:~/bash_study$ ls
ls
study1.sh
notify@npc:~/bash_study$ set +v
set +v
notify@npc:~/bash_study$ ls
study1.sh
notify@npc:~/bash_study$ touch *
notify@npc:~/bash_study$ ls
study1.sh
notify@npc:~/bash_study$ set -f
notify@npc:~/bash_study$ touch *
notify@npc:~/bash_study$ ls
'*'   study1.sh
notify@npc:~/bash_study$ rm *
notify@npc:~/bash_study$ ls
study1.sh
notify@npc:~/bash_study$ set +f
```

此外，还能通过在需要调试的地方适当使用`echo`命令来达到调试的效果。

___

下一篇：[bash的运行环境](bash-environment.md)
