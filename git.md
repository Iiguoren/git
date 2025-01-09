# git学习
## 工作区、暂存区、版本库
工作区： 用户电脑系统中的文件
暂存区： 用户区与版本库的中间状态，用户选择自己文件版本上传
版本库(repo): 版本库包含Git存储库的所有历史记录和元数据。它是Git存储库的核心组成部分，是由Git自动维护的
## 初始化
在repo中：
`git init`初始化一个仓库，执行 git init 后，Git 会在你的项目目录中创建一个 .git 文件夹，这是 Git 用来管理版本控制信息的地方。
**只是一个本地仓库，还没有与 GitHub 上的任何仓库关联**
### 暂存区
将文件添加到git仓库的暂存区： `git add git.md`
vscode中，git.md状态由U转为A
`git add .`将所有文件添加到暂存区
### 提交
提交暂存区中的变更到本地仓库，并添加一个描述信息：
`git commit -m "your commit"`
vscode中，git.md的状态消失；终端提示：`1 file changed`

### 管理
`git status`：查看当前工作区和暂存区的状态。
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   git.md

no changes added to commit (use "git add" and/or "git commit -a")
```
Changes not staged for commit：有一些修改尚未加入暂存区（即这些修改还没有准备好被提交）。对文件进行了修改，但这些修改还没有使用 git add 命令加入到 Git 的暂存区。
 no changes added to commit (use "git add" and/or "git commit -a")：没有任何修改被加入到提交中
`git log`：查看提交记录。
q退出
`git branch`：管理分支。
`git remote`：管理远程仓库。

## 工作区回退
`git checkout -- <filename>`
恢复工作区的文件到最近一次仓库的状态
使用提交哈希来恢复到指定提交的状态
`git checkout <commit-hash> -- <file>`
如果你执行了 git rm 删除了一个文件，但还没有提交，可以使用 `git checkout -- <filename>` 来恢复被删除的文件。
### 暂存区/工作区删除文件
将文件从暂存区和工作区中删除然后提交：
```
git rm <filename>
git commit -m "Remove file"
```

只删除暂存区保存工作区，使用--cached参数，
`git rm --cached <filename>`
可看到工作区状态为U:未跟踪的；
**这样并没有修改仓库，需要手动提交**
```
PS D:\c project\git_text> git rm --cached 1.c   
rm '1.c'
PS D:\c project\git_text> git ls-tree -r HEAD   
100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391    1.c
100644 blob 0a7cd39a1184806c6bf689322f408e179999491e    git.md
PS D:\c project\git_text> git commit -m "rmcached1.c"
[master 5840638] rmcached1.c
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 1.c
PS D:\c project\git_text> git ls-tree -r HEAD        
100644 blob 0a7cd39a1184806c6bf689322f408e179999491e    git.md
```
### 展示
`git ls-files`展示的是 暂存区（staging area）和工作目录中 Git 正在跟踪的文件。
git ls-files 只显示 Git 跟踪的文件。未跟踪的文件（即那些没有被 git add 的文件）不会出现在 git ls-files 的输出中。如果你想查看未跟踪的文件，可以使用 git status

## 远程仓库
1. 在git中创建一个仓库
2. 在本地仓库中：
   `git remote add origin https://github.com/LZX-ZNRA/learngit.git`将本地仓库与远程仓库关联
3. 将某个分支(master)推送到远程仓库
   `git push -u origin master`
   -u参数的作用：由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

   使用 git push 推送某个本地分支时，Git 会在远程仓库（例如 origin）创建一个与**本地分支同名**的远程分支
   如果远程仓库已经有一个同名的分支，Git 会将本地分支的提交推送到远程仓库中的该分支。
简化后：`git push`   
 
### 不同分支推送
如果你想将本地分支推送到远程的一个不同名称的分支，你可以通过指定远程分支的名称来实现。
`git push origin my-branch:remote-branch`
如果远程的 remote-branch 分支不存在，Git 会创建它

### 删除远程分支
`git push origin --delete my-branch`

## 分支
### 查看
`git branch`
### 切换到指定分支
git checkout <branchname>
### 删除分支
git branch -d <branchname>