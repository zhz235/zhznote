# git

!!! note
    一些关于git的基本知识
    
    [参考](https://thu-ios.github.io/tutorials/lecture/git.html)


## 工作区、暂存区与本地仓库
工作区（working directory）是电脑本地的文件，这些文件可以随意修改，或者说，你修改的文件总是工作区的文件。

通过`git add` 的命令，可以将文件添加到暂存区（stage）。通过`git mv` 命令，可以将文件重命名或移动后添加到暂存区。

通过`git commit -m 'commend'` 来把暂存区所有添加/修改放入本地仓库。

`git status` 查看各个区的状态。

通过`git log` 可以查看之前所有commit的信息（q退出），同时可以看到各个commit的版本号。通过 `git reset --hard <commit>`（`<commit>`指版本号）可以回退到指定版本并删除之后的commit。


## 单人工作流程
### 初始化
先初始化 `git init`

然后关联远程仓库 `git remote add origin git@github.com:zhz235/git-practice.git`

把master分支改名为main分支（推荐） `git branch -M main`

### git pull
`git pull`=`git fetch` + `git merge`，用于拉取远程仓库。

使用 `git push -u origin main` 拉取远程仓库。

### 分支
1. `git branch -c <branch-name>` 创建新分支。

2. `git switch <branch-name>` 移动到新分支。

   - `git branch` 查看分支信息和目前所在分支。

3. 切回main分支 : `git switch main`
4. 与原分支合并：`git merge`
5. 删除新建的分支：`git branch -d <branch-name>`

### 提交
`git push -u origin main` 提交修改到GitHub仓库

### 取消关联
`git remote -v` 查看关联的仓库

`git remote remove origin` 取消关联

## 多人工作流程
某些大项目可能没有仓库写入权限。TODO
