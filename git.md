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
内容
```
*.pyc
```
注意如果gitignore晚于commit，已commit的不会被删除
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
### 删除
```
rm 文件名
git rm 文件名
```
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
# 远程
## 生成公私密钥对
```sh
ssh-keygen -t rsa -C "邮箱"
```
然后将~/.ssh/id_rsa.pub的内容粘贴到https://github.com/settings/ssh/new中  
当推送到github时，github会用公钥加密一段信息，本地用私钥解密，以验证身份
## 首次关联远程仓库
```
git remote add origin git@github.com:用户名/仓库名.git
```
## 推到远程
### 首次（远程为空）
```
git push -u origin master
```
### 后续
```
git push origin master
```
## 从远程拉回来
```
git pull
```
## 克隆到本地
```
git clone github项目主页
```
# 分支
## 分支策略
master发布新版本  
dev用来工作
## 创建分支
```
git branch 分支名
```
## 切换分支
```
git checkout 分支名
```
若当前分支有未add或commmit的修改将会报错，可以先将暂存区贮藏
## 创建并切换分支
```
git checkout -b 分支名
```
## 查看所有分支
```
git branch
```
当前分支有*号
## 合并分支到当前分支
```
git merge 分支名
```
若当前分支有未add或commmit的修改将会报错  
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
# 贮藏与恢复
## 将暂存区贮藏
```
git stash
```
## 查看已贮藏的暂存区
```
git stash list
```
## 恢复暂存区
