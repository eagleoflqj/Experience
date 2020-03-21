# Bourne-Again SHell
```sh
bash [参数]
```
参数|意义
-|-
无|启动非登录bash
文件|在新bash中执行脚本
-c 命令字符串 [$0 ... $n]|在新bash中执行命令
-l或--login|启动登录bash
* 登录shell：`$0`（以及`ps -f`的CMD列）以`-`开头或，`login`、`ssh`、tty1~6
* 非登录shell：shell中启动shell、GUI启动终端
* `/etc/passwd`每行最后一项指定登录shell
* 登录bash执行`~/.bash_profile`（CentOS）或`~/.profile`（Ubuntu），一般会在上述文件加入对`~/.bashrc`的执行
* 登录bash退出执行`~/.bash_logout`
* 非登录bash执行`~/.bashrc`
# 内置命令
## cd 切换工作目录
## echo 输出并换行
```sh
echo 文字
echo $变量
echo ${变量}
echo "abc${变量}def"
```
### 转义
* 无引号：2n、2n+1个\\输出为n个\\
* 单引号：n个\\输出为n个\\
* 双引号：2n-1、2n个\\输出为n个\\
* -e：将无-e时的输出进行转义
## exec 替换当前shell
```sh
exec [选项] 命令 [参数1 ...]
```
选项|意义
-|-
-a 参数0|传递给命令的参数0，否则为命令
-c|在空环境执行命令
-l|替换为登录shell
## exit 退出当前shell
```sh
exit
```
## export 设置环境变量
```sh
export 变量=值
```
* 环境变量可供子进程访问，但子进程对其的修改不影响父进程
* export后第二次可以直接赋值
## logout 登出当前账户
```sh
logout [返回值]
```
* 需要作用在登录shell，等同于exit
## set 设置bash选项
```sh
set 选项
```
选项|意义
-|-
+h|关闭当前PATH下的命令位置hash缓存，每次顺序搜索PATH
## source 在当前shell执行脚本
```sh
source 脚本
```
## umask 设置新文件默认权限
```sh
umask [参数]
```
参数|意义
-|-
无|查看当前掩码
-S|查看当前权限
0abc|设置新目录权限777-abc，新文件权限666-abc
# 重定向
* 0标准输入
* 1标准输出 2标准错误 默认1  
* \>覆盖 \>\>追加
## 只显示错误
```sh
命令 >/dev/null
```
## 全不显示
```sh
命令 1>/dev/null 2>/dev/null 
命令 1>/dev/null 2>&1
```
# 符号
## ~ 家目录
```sh
cd ~
cd ~用户名
```
## \`命令\` 命令的标准输出
```sh
file `which ls`
```
## !! 上一条命令
```sh
sudo !!
PYTHONIOENCODING=utf-8 !!
```
## $@ 当前函数实参数组
## $# 当前函数实参数
## $? 上一条命令的返回值
```sh
echo $?
```
## -- 分隔选项和参数
```sh
kill -9 -- -进程组号
```
## <<EOF 多行输入，直到遇到EOF
```sh
cat <<EOF >文件
内容
EOF
```
# 环境变量
## HOME 用户家目录
## PATH 可执行文件搜索路径
* 顺序搜索，`:`分隔
## PS1 命令提示符
## PWD 工作目录
## SHELL 当前shell
## SHLVL 当前shell层数
# 本地化配置
## LANG 语言
* zh_CN.UTF-8
* 覆盖其他默认`LC_*`
## LC_TIME 日期时间
## LC_ALL 所有配置
* 若非空则覆盖其他默认/非默认`LC_*`
* C或POSIX表示7位ASCII
# 代理配置
* 在DNS之前决定是否使用代理
## http_proxy https_proxy HTTP[S]代理
* 值为`[协议]IP:端口`
## no_proxy 不使用代理的网站
* `,`分隔
* 包括高级域名
# 快捷键
快捷键|意义
-|-
Alt+B|移动光标到左侧单词首字母
Alt+F|移动光标到右侧（含）单词尾字母后
Alt+R|取消对来自history的命令的修改
Ctrl+K|剪切光标及右侧
Ctrl+R|搜索历史命令
Ctrl+U|剪切光标左侧
Ctrl+W|剪切光标左侧的单词
Ctrl+Y|粘贴
Ctrl+/|撤销
