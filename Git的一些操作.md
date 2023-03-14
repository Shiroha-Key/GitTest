# Git的一些操作

**配置基本用户信息**

```markdown
git config --global user.name <用户名>
git config --global user.email <邮箱地址>
```

**创建一个新仓库**

```markdown
git init
```

**从远程服务器克隆一个仓库**

```markdown
git clone <远程仓库的Url>
```

**显示当前的工作目录下的提交文件状态**

<!--类似于GitKraken右方窗口显示的信息-->

```markdown
git status
```

**将指定文件Stage（标记为将要被提交的文件）**

```markdown
git add <文件路径>
```

**将指定文件Unstage（取消标记为将要被提交的文件）**

```markdown
git reset<文件路径>
```

**创建一个提交并提供提交信息**

```markdown
git commit -m "提交信息"
```

**显示提交历史**

<!--类似于Gitkraken中间串口显示的提交历史-->

```
git log
```

**向远程仓库推送**(Push)

```markdown
git push
```

**从远程仓库拉取**(Pull)

```markdown
git pull
```



***



**修改(Amend)上一个提交**

```markdown
git commit --amend -m "<新的提交信息>"
```

**查看所有分支**

```mark
git branch
```

**创建新分支**

```markdown
git branch <分支名字>
```

**切换分支**

```markdown
git checkout <分支名字>
```

**重命名分支**

```markdown
git branch -m <旧名字> <新名字>
```

**删除分支**

```markdown
git branch -d <分支名字>
```

**将分支变基(Rebase)到master**

<!--需要先切换到分支之后，再完成变基-->

```markdown
git checkout <分支名字>
git rebase master
```

**使用快进(Fast-Forward)将分支合并到master**

```markdown
git checkout <分支名字>
git merge --ff-only master
```

**中止这一次提交的合并（当遇到冲突时）**

```markdown
git merge --about
```

**将未提交的修改暂存(Stash)**

```markdown
git stash save "<可以输入一个信息>"
```

**将上一个暂存的修改回复并从暂存列表中删除**

```markdown
git stash pop
```

**签出指定的提交**

```markdown
git checckout <提交的hash>
```

**撤销旧提交**

<!--Revert并不会修改旧提交历史，而是在工作树中生成与之前提交完全相反的修改-->

```markdown
git revert <旧提交的hash>
```

**利用reflog查看本地仓库中的所有操作**

```markdown
git reflog
```

++
