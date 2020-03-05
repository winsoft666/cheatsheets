# 配置文件
```bash
.git/config   #当前工作区的配置文件
~/.gitconfig  #当前用户的配置文件
```

# 配置
```bash
git config --global "Your Name"
git config --global "Email Address"
git config --global log.date format: %Y-%m-%d %H:%M:%S
git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080     #设置HTTP代理
git config --global http.proxy socks5://proxyuser:proxypwd@proxy.server.com:8080   #设置Socks5代理
git config --global --unset http.proxy  #移除HTTP代理设置
git config --global --get http.proxy    #查看HTTP代理设置
git config [--global] alias.<alias> '<original command>'  #为所有工作区/当前工作区配置别名
git config --list
```

# 仓库初始化
```bash
git init
```

# 克隆
```bash
git clone <remote address>
git clone --depth=1 <remote address>  #克隆最新一次提交
```

# 暂存区
```bash
git add <file>
git add -f <file>       #强制添加，尽管在.gitignore文件中已被忽略
git add -u              #添加：已被Track且被修改的文件
git add -A              #添加所有文件，包含删除文件、新增文件、修改文件
git add .               #添加：已被Track且被修改的文件、新增文件，但不包括被删除的文件
git add --all <dir>     #递归添加整个目录
```

# 提交
```bash
git commit -m "descriptions"   #提交，并附带提交描述
git commit --amend             #对最近一次的提交做内容修改
git commit --amend --author "user_name <user_email>"  #修改最近提交用户名和邮箱
```

# 状态
```bash
git status                  #显示文件状态
git status -s               #已精简的方式显示文件状态, A:新增; M:文件变更; ?:未track; D:删除
git diff <file>             #查看工作区与暂存区中的文件差异
git diff HEAD -- <file>     #查看工作区和仓库中的文件差异
git diff --cached <file>    #查看暂存区和仓库中的文件差异
git diff --check <file>     #检查是否有空白错误(regex:' \{1,\}$')
git diff commit-id [<file>...]  #比较工作区与指定commit-id的差异　　　　　　
git diff --cached [<commit-id>] [<file>...]   #比较暂存区与指定commit-id的差异
git diff [<commit-id>] [<commit-id>]          #比较两个commit-id之间的差异
```

# 查看历史版本、历史操作
```bash
git log
git reflog
git log -n                  #最近n条的提交历史
git log <branch_name> -n    #分支branch_name最近n条的提交历史
git log --stat              #历次commit的文件变化
git log --shortstat         #对比--stat只显示最后的总文件和行数变化统计(n file changed, n insertions(+), n deletion(-))
git log --name-status       #显示新增、修改、删除的文件清单
git log lhs_hash..rhs_hash  #对比两次commit的变化(增删的主语为lhs, 如git log HEAD~2..HEAD == git log HEAD -3)
git log -p                  #历次commit的内容增删
git log -p -W               #历次commit的内容增删, 同时显示变更内容的上下文
git log origin/EI-1024 -1 --stat -p -W  #查看远端分支EI-1024前一次修改的详细内容
git log origin/master..dev --stat -p -W #查看本地dev分支比远端master分支变化(修改)的详细内容
git log --pretty=oneline --abbrev-commit --decorate=full	#在log中显示标签
git log <branch_name> --oneline   #对提交历史单行排列
git log <branch_name> --graph     #对提交历史图形化排列
git log <branch_name> --decorate  #对提交历史关联相关引用, 如tag, 本地远程分支等
git log <branch_name> --oneline --graph --decorate #树形化显示历史
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen%ai(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit #同上, 建议alais保存
git log --since 或 --after    #显示时间之后的提交
git log --until 或 --before   #显示时间之前的提交
git --author                  #显示指定作者的提交
git --committer               #显示指定committer的提交(注:committer不一定是author)
git log -S [keyword]          #仅显示添加或移除了某个关键字的提交(某些场景比单独git log -p | grep [keyword] 好用很多)
git log origin/b3.3/master --author=yx-ren --since="2019-10-01" --before="2019-11-01" #查看某作者在某发布版本最近一个月的提交
git log --since=1.weeks     #过去一周的提交
git log --since=1.days      #过去一天的提交
git log --since="1 weeks 2 days 3 hours 40 minutes 50 seconds ago" #过去1周2天3小时40分50秒之内的提交
```

# 查看Commit详情
```bash
git show                         #查看最新的commit
git show <commitId>              #查看指定commit hashID的所有修改
git show <commitId> <fileName>   #查看某次commit中具体某个文件的修改
git show <commitId>:<fileName>   #查看某次commit中具体某个文件的内容
git show <commitId>:<fileName> > <new_fileName>   #将某次commit中具体某个文件的内容导出到新文件中
git cherry -v                    #查看已经提交但是未传送到远程代码库的提交描述/说明
git log master ^origin/master    #查看已经提交但是未传送到远程代码库的详情
git diff <commit-id>^!             #查看指定commit-id的修改
```

# 版本回退、前进
```bash
git reset --hard HEAD^		#回退到上1版本
git reset --hard HEAD~5		#回退到上5个版本
git reset --hard id		    #回退到指定版本
```

# 撤销修改
```bash
git checkout -- <file>    #撤销修改：误修改工作区文件，未git add/commit
git reset HEAD <file>     #撤销git add：误将文件加入暂存区（git add），未git commit
git reset --hard HEAD^    #撤销git commit：误将文件提交（一旦提交，只能通过版本回退进行撤销）
```

# 删除与恢复
```bash
git rm <file>                     #删除工作区的文件，并提交删除操作到暂存区
git rm --cached <file>            #删除暂存区的文件，但不影响工作区
git checkout -- <file>            #用暂存区中的文件替换工作区的文件
git checkout HEAD -- <file>       #用仓库中的文件替换工作区的文件
```

# 清理工作区
```bash
git clean -i    #交互式清理, 不常用
git clean -n    #查看清理文件列表(不包括文件夹), 不执行实际清理动作
git clean -n -d #查看清理文件列表(包括文件夹), 不执行实际清理动作
git clean -f    #清理所有未track文件
git clean -df   #清理所有未track文件和文件夹, 使用前确保新增加的文件或文件夹已add, 否则新创建的文件或者文件夹也会被强制删除
```

# 分支管理
```bash
git branch <branch name>          #创建<branch name>分支
git checkout <branch name>        #切换至<branch name>分支
git checkout -b <branch name>     #创建并切换至<branch name>分支
git branch                        #查看本地已有分支（* 表示当前分支）
git branch -all                   #查看所有已有分支（包含线上分支）（* 表示当前分支）
git merge <branch name>           #合并<branch name>到当前分支
git branch -d <branch name>       #删除分支
git branch -D <branch name>       #强制删除分支（丢弃未合并分支）
git merge --no-ff -m "descriptions" <branch name>     #合并后删除分支也在log中保留分支记录
git push origin test:master -f    #将test分支强制（-f）推送到主分支master
```

# 关联GitHub远程仓库（本地到远程）
```bash
git remote add origin <remote address>      #在本地工作区目录下按照 GitHub 提示进行关联
git remote rm origin           #解除错误关联
git push -u origin master      #第一次将本地仓库推送至远程仓库（每次在本地提交后进行操作）
git push origin master         #以后每次将本地仓库推送至远程仓库（每次在本地提交后进行操作）
```

# Bug分支管理（建立单独分支进行bug修复）
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

```bash
git stash			    #保存当前工作现场（在dev未完成开发，但master有bug需要修复）
git stash pop			#回到dev分支后恢复工作现场（list中的现场会同时被删除）
git stash list			#查看当前存储的工作现场
git stash apply stash@{#}	#回到指定工作现场（list中的现场不会被删除，需要用git stash drop）
git stash drop stash@{#}	#删除指定工作现场
git cherry-pick <id>		#在master修复好bug后，在dev复制一遍bug修复流程
```

# 协作与分支推送
```bash
User 1:
git remote [-v]						#查看远程库信息（-v 查看详细信息）
git push origin [master/dev/...]	#推送指定分支到远程

User 2:
git clone <remote address>			#克隆到本地（只能克隆master）
git checkout -b dev origin/dev		#本地新建分支并关联远程
git add/commit/push					#添加、提交、推送更新

User 1:
git add/commit/push					#推送时报错（与user 2推送的更新冲突）
git pull <remote> <branch>
git branch --set-upstream-to=origin/<branch> <branch>	#本地与远程关联
git pull						#拉取远程文件（并解决冲突）
git commit/push					#重新提交并推送
```

# 标签管理（常用于版本管理）：查看、创建、操作
```bash
git tag                     #查看标签
git show <tag name>         #查看指定标签
git tag <tag name>          #为上次commit位置打标签
git tag <tag name> <commit id>  #为指定commit位置打标签
git tag -a <tag name> -m "descriptions" <commit id>     #为指定commit打标并添加描述
git tag -d <tag name>               #删除本地标签
git push origin <tag name>          #推送指定标签到远程
git push origin --tags              #推送所有本地标签到远程
git push origin :refs/tags/<tag name>     #删除远程标签（先删除本地标签）
```

# 生成diff patch补丁文件
```bash
git <branch> log -n -p > diff.patch     #生成某分支过去n个commit的文件diff信息至单个diff文件
git diff <--cached> diff.patch    #针对当前缓存区的内容生成diff文件
```

# 打patch补丁
```bash
git apply --check diff.patch    #检查是否可以正常应用, 无回显证明无冲突
git apply --stat diff.patch     #查看应用diff文件后的文件变化
git apply diff.patch            #打patch, 仅仅改变文件信息, 无commit信息, 仍然需要add, commit
```

# 利用--format-patch生成patch, 带commit信息
```bash
git format-patch HEAD^ 　       #生成最近的1次commit的patch
git format-patch HEAD^^         #生成最近的2次commit的patch
git format-patch HEAD^^^        #生成最近的3次commit的patch
git format-patch <branch> -n 　 #生成分支<branch>最近的n次commit的patch
git format-patch <r1>..<r2>     #生成两个commit间的修改的patch（包含两个commit. <r1>和<r2>都是具体的commit号)
git format-patch -1 <r1>        #生成单个commit的patch
git format-patch <r1>           #生成某commit以来的修改patch（不包含该commit）
git format-patch --root <r1>　　#生成从根到r1提交的所有patch
```

# 利用am打patch
```bash
git apply --check 0001-update-bash.sh.patch #检查patch是否冲突可用
git apply --stat 0001-update-bash.sh.patch  #检查patch文件变更情况, 无回显证明无冲突
git am 0001-update-bash.sh.patch            #将该patch打上到当前分支, 带commit信息
git am ./*.patch                            #将当前路径下的所有patch按照先后顺序打上
git am --abort                              #终止整个打patch的过程, 类似rebase --abort
git am --resolved                           #解决冲突后, 可以执行该命令进行后续的patch, 类似rebase --continue
```

```
解决应用Patch时如果发生冲突，可以使用git am --abort取消操作。
如果仍需要应用这个Patch，可以使用如下解决方法：

1. 根据git am失败的信息，找到发生冲突的具体patch文件，然后用命令git apply --reject <patch_name>，强行打这个patch，发生冲突的部分会保存为.rej文件（例如发生冲突的文件是a.txt，那么运行完这个命令后，发生conflict的部分会保存为a.txt.rej），未发生冲突的部分会成功打上patch。
2. 根据.rej文件，通过编辑该patch文件的方式解决冲突。
3. 废弃上一条am命令已经应用了的patch：git am --abort
4. 重新应用patch：git am ~/patch-set/*.patch
```

# bundle打包
只打包track的文件.
```bash
git bundle create XXX.bundle HEAD master #打包master分支的所有数据
git clone XXX.bundle # 从XXX.bundle重建工程

# 可以打包指定的区间, 至于提交区间有多种表示方式
git bundle create awesome-cheatsheets.bundle HEAD~10
git bundle create awesome-cheatsheets.bundle HEAD~10..HEAD
git bundle create awesome-cheatsheets.bundle lhs_commit_md5..rhs_commit_md5
git bundle create awesome-cheatsheets.bundle origin/master..master
git bundle create awesome-cheatsheets.bundle master ^origin/master
```

# .gitignore配置文件
```bash
/<dir name>/      #忽略文件夹
*.zip             #忽略.zip文件
/<dir name>/<file name>     #忽略指定文件
git check-ignore -v <file>  #查看生效规则
```
