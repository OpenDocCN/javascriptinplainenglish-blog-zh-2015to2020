# 初级开发人员应该知道的 17 个 Git 命令

> 原文：<https://javascript.plainenglish.io/the-17-git-commands-you-should-know-as-a-junior-developer-9bd439adf5cc?source=collection_archive---------6----------------------->

![](img/dff70681404f248240c940b45bc00b15.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在职业生涯的各个阶段，记住 git 命令可能都很棘手，尤其是在初级开发人员起步的时候。我已经开发了近 3 年，但仍然做一个快速的谷歌搜索。

这就是为什么我列出了每个初级开发人员都应该知道的 17 个 git 命令。开始了…

## 1.检查项目中哪些已更改的文件尚未暂存或提交。

```
git status
```

## 2.提交更改之前，请检查更改

```
git diff
```

## 3.准备要提交的文件

添加单个文件

```
git add <file-name>
```

添加所有文件

```
git add .
```

## 4.提交准备推送的文件

```
git commit -m '<add commit message here>'
```

## 5.取消添加您已暂存的文件

```
git reset
```

## 6.保存更改供以后使用，而不提交它们

```
git stash
```

## 7.拿回你藏起来的零钱

```
git stash pop
```

## 8.把你的分行推到远程回购

```
git push
```

## **9。下载最新的更改，而不将它们集成到您的项目中**

```
git fetch
```

## **10。下载最新的变更并将其整合到您的项目中**

```
git pull
```

## **11。检查你在哪个分支**

```
git branch
```

## 12.从当前分支分出一个新分支

```
git branch <new-branch-name>
```

## 13.从一个分支移到另一个分支

```
git checkout <branch-name>
```

## 14.从当前分支分出一个分支，创建一个新分支，然后移动到该分支

```
git checkout -b <new-branch-name>
```

## 15.重命名您的分支

如果重命名一个 ***局部*** 分支:

*   确保你在你的分支上

```
git branch -m <new-branch-name>
```

如果重命名一个 ***远程*** 分支:

*   使用上面的命令
*   推树枝

```
git push origin -u <new-branch-name> **// this resets the upstream branch**
git push origin --delete <old-branch-name> // this deletes the old remote branch
```

## 16.删除分支

如果删除一个 ***本地*** 分支:

```
git branch -D branch_name
```

如果删除一个 ***远程*** 分支:

```
git push origin --delete <old-branch-name>
```

## 17.将另一个分支合并到您的分支中

*   确保你在你想要合并的分支上

```
git merge <name-of-branch-you-want-to-merge-in>
```

# 结论

这些是我每天使用的命令。我希望这有所帮助。如果有其他你认为我应该使用的，请在评论中告诉我。谢谢！

*更多内容请看*[***plain English . io***](https://plainenglish.io/)