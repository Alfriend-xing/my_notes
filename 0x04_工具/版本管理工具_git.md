# git

## 基本操作
```shell
初次安装git配置用户名和邮箱
git config --global user.name "yourname"
git config --global user.email "youremailaddress"

创建本地库
git init

添加文件
git add readme.md

本地提交
git commit -m 'wrote a readme file'

查看本地仓库状态
git status

查看difference
git diff readme.md
```
## 版本回退
```shell
查看更新日志
git log     （--pretty=oneline）

回退至上个版本
git reset --hard HEAD^
上上个版本:HEAD^^，上100个:HEAD~100

命令记录
git reflog

恢复到新版本
从命令记录中找到版本号 1094a
git reset --hard 1094a

丢弃工作区的修改/恢复误删
git checkout -- file

撤销暂存区的修改
git reset HEAD <file>
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区

删除文件
git rm # 同时从工作区和索引中删除文件。即本地的文件也被删除了。
git rm --cached # 从索引中删除文件。但是本地文件还存在，只是不希望这个文件被版本控制，可以在gitignore文件变更后搭配使用
git rm -r mydir # 删除文件夹
git rm test.md  # 删除文件
git commit -m "remove test.md"  # 上述操作最后要执行git commit才真正提交到git仓库
```
## 远程仓库
```shell
添加远程库
git remote add origin git@github.com:michaelliao/learngit.git
origin只是远程库的默认名称

删除远程库
git remote rm origin

修改远程库
git remote origin set-url [url]

把本地库的所有内容推送到远程库上
git push -u origin master

推送本地master分支的最新修改
git push origin master

从远程库克隆
git clone git@github.com:michaelliao/gitskills.git

    使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https
    如果使用了未被信任的https证书
    git config --global http.sslVerify false

    而使用ssh方式，首次连接的主机需要添加ssh密钥
    生成密钥
    ssh-keygen -t rsa -C "youremail@example.com"
    位置win:c:\user\username\.ssh;linux:~/.ssh内的id_rsa.pub
    之后在网页上添加上述文件中的密钥
```
## 分支管理
```shell
创建分支
git checkout -b dev 创建并切换到dev分支
或者
git branch dev      创建
git checkout dev    切换

查看当前分支
git branch

合并分支
git merge dev       用于合并指定分支(dev)到当前分支

删除分支
git branch -d dev
    因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，
    合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

解决冲突
    两个分支修改了同一个文件,则进行merge快速合并时会发生冲突
    Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
首先手动修改冲突
git add readme.txt
git commit -m "conflict fixed"

禁用Fast forward模式
git merge --no-ff -m "merge with no-ff" dev
    Git默认会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息
```
## 分支策略
    在实际开发中，我们应该按照几个基本原则进行分支管理：
- 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
- 那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
- 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
![](gitbranch.png)

## Bug分支
    设A为游戏软件
1. master 上面发布的是A的1.0版本

1. dev 上开发的是A的2.0版本

1. 这时，用户反映 1.0版本存在漏洞，有人利用这个漏洞开外挂

1. 需要从dev切换到master去填这个漏洞，正常必须先提交dev目前的工作，才能切换。

1. 而dev的工作还未完成，不想提交，所以先把dev的工作stash一下。然后切换到master

1. 在master建立分支issue101并切换.

1. 在issue101上修复漏洞。

1. 修复后，在master上合并issue101

1. 切回dev，恢复原本工作，继续工作。
    
    具体操作
```shell
git stash   把所有未提交的修改（包括暂存的和非暂存的）都保存起来
git checkout master     切换至需要修复bug的分支
git checkout -b issue-101   创建临时分支
修复bug...
git add readme.txt
git commit -m "fix bug 101"     修复完成
git checkout master     切回有bug的分支
git merge --no-ff -m "merged bug fix 101" issue-101 完成合并
git checkout dev    回到开发分支
git stash list  查看保存的工作现场
git stash pop   恢复工作现场，自动删除stash内容
```
## Feature分支
    每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
```shell
git checkout -b feature-vulcan  创建分支
开发过程
git add vulcan.py
git commit -m "add feature"
git checkout dev
git merge --no-ff -m "add feature branch" feature-vulcan
git branch -d feature-vulcan
或者
git branch -D feature-vulcan 强行删除没有合并的分支
```
## 多人协作
    多人协作的工作模式通常是这样：

1. 用`git push origin <branch-name>`推送自己的修改；

1. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

1. 如果合并有冲突，则解决冲突，并在本地提交；

1. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

>如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建 git branch --set-upstream-to=origin/remote_branch your_branch origin为远程仓库名 remote_branch为远程分支名 your_branch本地分支名

小结
- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。
## Rebase
https://git-scm.com/book/zh/v2/Git-分支-变基
- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
- 只对尚未推送或分享给别人的本地修改执行变基操作清理历史；
- 从不对已推送至别处的提交执行变基操作
## 标签管理
```shell
创建分支标签
git branch
git checkout master 切换到需要打标签的分支上
git tag <name>

创建commit标签
git log --pretty=oneline --abbrev-commit
对f52c633这次提交打标签
git tag v0.9 f52c633

创建带有说明的标签
git tag -a v0.1 -m "version 0.1 released" 1094adb

查看所有标签
git tag
查看标签信息
git show v0.9

删除标签
本地
git tag -d v0.1
远程
git push origin :refs/tags/v0.9

推送标签
git push origin v1.0
git push origin --tags      #推送全部
```
参考信息 [Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)




