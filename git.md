# 配置
## 生成用户
```sh
git config --global user.name "用户名"
git config --global user.email "邮箱"
```
## 查看用户
```
git config user.name
git config user.email
```
# 本地
## 在项目根目录新建git仓库
```
git init
```
## 添加.gitignore
Windows下无法右键新建，需要
```
notepad .gitignore
```
内容参考https://github.com/github/gitignore  
注意如果.gitignore晚于commit，已commit的不会被删除
## 查看当前状态
```
git status
```
## 查看工作区与版本库（暂存区、仓库分支）不同
```
git diff 文件名
```
## 撤销工作区修改
```
git checkout -- 文件名
```
将工作区的文件恢复至最后一次add或commit时的状态，不可反悔
## 将修改加入暂存区
### 全部
```
git add .
```
### 单独
```
git add 文件名
```
### 查看阻止添加的.gitignore规则
```
git check-ignore -v 文件名
```
### 忽略.gitignore强行添加
```
git add -f 文件名
```
### 删除
```
rm 文件名
git rm 文件名
```
若一文件从未被add，则不会被git追踪
## 查看暂存区与仓库分支不同
```
git diff --cached
```
## 撤销暂存区修改
```
git reset HEAD 文件名
```
不改变工作区
## 提交修改
```
git commit
```
按vim方式输入commit备注
或
```sh
git commit -m "commit备注"
```
## 查看提交记录
```
git log
```
单行显示
```
git log --pretty=oneline
```
## 回退版本
当前版本 HEAD  
上一版本 HEAD^  
上n版本 HEAD~n  
```
git reset --hard 版本号前几位或相对HEAD版本
```
注意若回退前HEAD已推到远程，回退后无法推送
## 查看版本改动记录
```
git reflog
```
可以用来反悔回退
# 分支
## 分支策略
master发布新版本，需要远程同步  
dev用来工作，需要远程同步  
bug用来改错，不需要同步  
feature用来开发新功能，视是否合作决定同步
## 创建分支
```
git branch 分支名
```
## 切换分支
```
git checkout 分支名
```
若两分支不处于同一节点，且当前分支有未add或commmit的修改则会报错，可以add、commit，或将工作区贮藏
## 创建并切换分支
```
git checkout -b 分支名
```
此时工作区、暂存区都不变
## 查看所有分支
```
git branch
```
当前分支有*号
## 合并分支到当前分支
```
git merge 分支名
```
若有未add或commmit的修改将会报错  
若当前分支落后，则会采用Fast forward，若删除分支则不能看出合并过  
禁用Fast forward合并会产生新commit
```sh
git merge --no-ff -m "commit备注" 分支名
```
## 解决冲突
merge时产生冲突，冲突部分如下所示
```
<<<<<<< HEAD
当前文本
=======
冲突分支文本
>>>>>>> 分支名
```
手动解决冲突后，add、commit
## 查看分支图
```
git log --graph --pretty=oneline --abbrev-commit
```
## 删除分支
```
git branch -d 分支名
```
## 强行删除没有合并过的分支
```
git branch -D 分支名
```
# 贮藏与恢复
## 贮藏工作区
```
git stash
```
恢复至上次commit时的工作区  
若工作区无改动，则什么也不做  
暂存区将被丢弃
## 查看已贮藏的暂存区
```
git stash list
```
## 恢复工作区
必须将修改全部add，否则报错
### 恢复最近的
```
git stash apply
```
### 恢复指定的
```
git stash apply stash@{0}
```  
恢复后无冲突的改动直接进入暂存区  
若有冲突，则如下所示
```
<<<<<<< Updated upstream
当前工作区
=======
贮藏的工作区
>>>>>>> Stashed changes
```
## 删除贮藏的工作区
```
git stash drop
```
## 恢复并删除
```
git stash pop
```
# 远程
## 生成公私密钥对
```sh
ssh-keygen -t rsa -C "邮箱"
```
然后将~/.ssh/id_rsa.pub的内容粘贴到https://github.com/settings/ssh/new中  
当推送到github时，github会用公钥加密一段信息，本地用私钥解密，以验证身份
## 首次关联远程仓库
```
git remote add 远程库名 git@github.com:用户名/仓库名.git
```
## 删除与远程库的关联
```
git remote rm 远程库名
```
## 查看远程仓库
### 名称
```
git remote
```
### 详细信息
```
git remote -v
```
如果没有推送权限，就看不到(push)
## 推到远程
### 首次（远程为空）
```
git push -u 远程库名 分支名
```
### 后续
```
git push 远程库名 分支名
```
## 从远程拉回来
```
git pull
```
若提示There is no tracking information for the current branch，可以按照提示创建本地分支与远程分支的链接
```
git branch --set-upstream-to=远程库名/远程分支名 本地分支名
```
若冲突，需解决
## 变基
将远程拉回后产生冲突，解决冲突后本地分支和远程分支合并为一个新的commit  
如果想在本地保持线性提交历史，变基可以保留远程的提交，然后将本地提交嫁接在其后
```
git rebase
```
这会改变本地提交的内容，也可能产生冲突  
解决冲突后先add，再
```
git rebase --continue
```
若要放弃变基，则可
```
git rebase --abort
```
## 克隆到本地
```
git clone github项目主页
```
## 将远程分支拉到本地并切换
```
git checkout -b 分支名 远程库名/远程分支名
```
# 标签
## 打标签
先切换到要打标签的分支上
### 普通标签
```
git tag 标签名 [版本号前几位]
```
### 带说明的标签
```
git tag -a 标签名 -m "说明" [版本号前几位]
```
标签总跟commit挂钩，默认为当前commit  
如果某个打标的commit位于两个分支上，则这两个分支都能看见这个标签
## 查看标签
```
git tag
```
## 查看标签信息
```
git show 标签名
```
## 删除标签
```
git tag -d 标签名
```
## 将标签推到远程
### 单独
```
git push 远程库名 标签名
```
### 全部
```
git push 远程库名 --tag
```
## 远程删除标签
先在本地删除标签，然后
```
git push 远程库名 :refs/tags/标签名
```
# 别名
## 一般格式
```
git config [--global] alias.别名 原名
```
--global表示作用在当前用户，不加则表示作用在当前仓库  
用户配置文件~/.gitconfig，仓库配置文件./.git/config
## 常用别名
### 撤销暂存区修改
```sh
git config --global alias.unstage 'reset HEAD'
```
### 查看最后一次提交
```sh
git config --global alias.last 'log -1'
```
### 查看分支图
```sh
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
# 搭建git服务器
## 安装git
```
sudo apt install git
```
## 创建用户
```
sudo adduser git
```
## 收集公钥
将成员的id_rsa.pub粘贴到/home/git/.ssh/authorized_keys，一行一个
## 初始化git仓库
```
git init --bare 仓库名.git
sudo chown -R git:git 仓库名.git
```
## 禁用shell登录
将/etc/passwd中类似
```
git:x:1001:1001:,,,:/home/git:/bin/bash
```
的一行改为
```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```
## 克隆到本地
在成员的电脑上执行
```
git clone git@服务器:仓库位置
```
