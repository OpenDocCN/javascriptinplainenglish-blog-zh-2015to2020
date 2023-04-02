# 17 个对编码人员有用的终端命令

> 原文：<https://javascript.plainenglish.io/most-useful-unix-commands-youll-ever-need-b11862eca354?source=collection_archive---------5----------------------->

## 让您的生活更轻松的终端命令

![](img/a74ade84d7a82420062aca94418341b6.png)

Wikimedia commons

Unix 是 20 世纪 70 年代由贝尔实验室开发的一系列操作系统。它的一些最著名的变种包括:Ubuntu、MacOS 和 Linux。它们都有一个交互式命令行解释器，可以通过名为 shell 的终端访问，允许您执行各种任务，从创建文件和文件夹，到运行脚本，甚至关闭计算机。

使用鼠标和键盘可以做的几乎所有事情都可以通过 Unix shell 来完成(比如 bash、csh、ksh)。在本文中，我们将探索每个代码初学者都应该知道的 17 个终端命令。

在开始之前，我们应该提到，每个命令都可以与一个修饰符结合使用，以增强它的工作方式。命令修饰符遵循以下格式:

```
Command_name -modifier
Example: ls -a
```

命令`*ls*` 与修饰符`*-a*` 结合，列出*目录下的所有文件*。

此外，多个修饰词可以结合起来，在同一行实现多种功能。其格式为:

```
Command_name -ab (where a, b are separate modifiers)
```

**命令行编辑**

当您键入这些命令时，您可能会发现自己一路出错。以下是一些帮助您编辑它们的命令:

```
Backspace: Deletes the previous character
CTRL + D: Deletes the next character.
CTRL + K: Deletes the rest of the line (in front of the cursor)
CTRL + U: This will delete the line behind the cursor
CTRL + A: Goes to the start of the line
CTRL + E: Goes to the end of the line
Tab: Completes the filename or command
CTRL + P: Displays your last commands going back (repeat)
CTRL + N: Displays your previous commands going forward
CTRL + L: Clears the screen
CMD + K: Clears the screen
```

其中一些命令也可以在你的文本编辑器或文字处理器中使用。

事不宜迟，下面是我为编码初学者列出的 17 个最有用的终端命令。

# **目录**

## **PWD**

这个命令代表“当前工作目录”,它显示你当前所在的目录。

```
Format: pwd
```

## **ls**

ls 用于“列出”给定目录中的文件和文件夹。

```
Format: ls *file_path*
Modifiers:
```-l : Lists your files in “long format”```
-a : Lists all files, including hidden ones (folders and files that begin with a ‘.’ (dotfiles))
-S: Sorts by file size
```

## **光盘**

cd 代表“更改目录”,允许您浏览文件夹。

```
Format: cd *directory_path*
cd ~: Takes you to the home directory
cd folder_path: Takes you to that folder in your path
cd ..: Is for moving one level up in your directory tree
cd …/…/…: Takes you 3 directory levels up.
```

## **mkdir**

这个命令为你“创建一个目录”。

```
Format: mkdir *folder_name*
Modifier:
-v: Stands for ‘verbose’ and forces mkdir to give feedback on the folder(s) that it creates.
```

## **打开**

您可以使用此命令在 Finder 中打开给定的目录。

```
Format: open *directory_path*
Example: open . (opens current directory in Finder)
```

# **文件**

## **触摸**

创建或保存文件。也用于更改文件最后一次被访问的时间。

```
Format: touch *filename*
Modifier:
-a: Updates the access time of a file
```

## **猫**

显示文件的内容，而不打开它。

```
Format: cat *filename*
Modifier:
-n: This will number each line displayed
```

## **少了**

Less 允许您在不修改文件的情况下查看文件。默认情况下，许多显示信息的命令将使用较少的资源打印到屏幕上。要退出，请按“Q”键。

```
Format: less *filename*
```

## 丙酸纤维素

该命令将一个文件的内容“复制”到另一个文件。格式如下:

```
Format: cp *file1 file2*
Modifiers:
-n: Negates or prevents overwriting an existing file
-v: Verbose, forces feedback when copying the file
```

## **mv**

它用于移动文件或文件夹。

```
Format: mv *dir1 dir2* (moves dir1 to dir2)
Modifiers:
-n: prevents overwriting existing files/folders
-v: for feedback showing that the file/folder has been moved
```

## **rm**

代表“移除”。它删除文件或文件夹。

```
Format: rm *filename/directory*
Modifiers:
-r: Does a ‘recursive’ deletion, removing files within subfolders first, then top level files. It can be used to delete a directory.
-f: Removes the file/folder without prompting for confirmation, regardless of the permission set.
-i: Prompts for confirmation prior to deletion.
```

## **差异**

它比较两个文件，显示它们不同的地方。

```
Format: diff *file1 file2*
Modifiers:
-w: Ignore white spaces
-i: Ignore differences in case capitalization
```

## **wc**

计算文件中的字符数、字数和行数。

```
Format: wc *filename*
```

# **搜索**

## **grep**

在给定文件中搜索字符串。它有许多形式(grep、egrep 和 fgrep ),允许在搜索字符串参数时使用正则表达式。正则表达式可以与 grep 结合使用，以获得更精确的搜索结果。

```
Format: grep *string filename*
```

## **找到**

就像前面的命令一样，它在您的计算机上搜索文件

```
Format: find *directory filename*
Example: find . *-name ruby* This will match all files with “ruby” as part of their name in the current directory.
```

# **杂项**

## **历史**

显示给定会话中所有过去命令的历史记录。

```
Format: history
```

## **男人**

Man 代表“manual ”,它提供了每个 shell 命令的非常详细的描述。

```
Format: man *command_name*
Example: man ps
```

我希望这些命令能让你在终端导航时更轻松。请随意查阅手册(“手册”)以了解每一项的更多详细信息。

另外，如果你喜欢关闭你的电脑，你可以使用“关机”命令。它会在一分钟内关闭你的电脑。赶时间的话可以加上关键词“现在”。试试看！

```
Format: sudo shutdown -h now
```