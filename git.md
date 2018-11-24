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
## 将修改加入暂存区
### 全部
```
git add .
```
### 单独
```
git add 文件名
```
## 查看暂存区与仓库分支不同
```
git diff --cached
```
## 本地提交修改
```
git commit
```
按vim方式输入commit名字
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
```git
git reflog
```
可以用来反悔回退
## 首次关联远程仓库
```
git remote add origin git@github.com:用户名/仓库名.git
```
## 推到远程
```
git push –u origin master
```
## 从远程拉回来
```
git pull
```
## 克隆到本地
```
git clone github项目主页
```
