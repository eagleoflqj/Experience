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
--version|显示版本
* 登录shell：`$0`（以及`ps -f`的CMD列）以`-`开头或，`login`、`ssh`、tty1~6
* 非登录shell：shell中启动shell、GUI启动终端
* `/etc/passwd`每行最后一项指定登录shell
* 登录bash执行`~/.bash_profile`（CentOS）或`~/.profile`（Ubuntu），一般会在上述文件加入对`~/.bashrc`的执行
* 登录bash退出执行`~/.bash_logout`
* 非登录bash执行`~/.bashrc`
* 关闭终端时，前台、后台、暂停进程收到SIGHUP信号结束
# 内置命令
## bg 后台运行作业
```sh
bg [%作业号1 [... %作业号n]]
```
## cd 切换工作目录
## disown 忽略sighup
```sh
disown -h [%作业号或进程号]
```
* 关闭终端或exit当前shell时，后台R、S进程被接管，其他进程结束
## echo 输出并换行
```sh
echo [选项] 内容
echo "abc${变量}def"
```
选项|意义
-|-
-e|将无-e时的输出进行转义
-n|不自动换行
### 转义
* 无引号：2n、2n+1个\\输出为n个\\
* 单引号：n个\\输出为n个\\
* 双引号：2n-1、2n个\\输出为n个\\
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
* T进程结束
* R、S进程（显然为后台进程）被接管，遇到IO时若终端已关闭（TTY为?）则结束，若未关闭则I阻塞，O输出到终端
## export 设置环境变量
```sh
export 变量=值
```
* 环境变量可供子进程访问，但子进程对其的修改不影响父进程
* export后第二次可以直接赋值
## fg 前台运行作业
```sh
fg [%作业号]
```
## jobs 查看全部作业
```sh
jobs [参数]
```
参数|意义
-|-
-l|显示进程号
* +：fg、bg、disown默认的作业
* -：下一个默认的作业
## logout 登出当前账户
```sh
logout [返回值]
```
* 需要作用在登录shell，等同于exit
## popd 弹出栈中目录
```sh
popd
```
## pushd 压栈当前目录并跳转
```sh
pushd 新目录
```
## pwd 输出当前目录
```sh
pwd
```
## set 设置bash选项
```sh
set 选项
```
选项|意义
-|-
-f|关闭glob扩展
+h|关闭当前PATH下的命令位置hash缓存，每次顺序搜索PATH
-o noglob|关闭glob扩展
## source 在当前shell执行脚本
```sh
source 脚本
```
## type 判断命令类型
```sh
type [选项] 命令
```
选项|意义
-|-
-a|输出命令的所有定义，否则只输出使用中的定义
-t|只输出类型，alias、builtin、file、function、keyword
## ulimit shell资源限制
```sh
ulimit [选项 [数值或unlimited]]
```
选项|意义
-|-
-a|显示全部资源限定
-c|core文件块数
-d|数据段KB数
-e|nice优先级
-f|创建的文件块数
-i|阻塞信号数
-l|进程锁在内存KB数
-m|使用内存KB数
-n|同时打开文件数
-p|管道512B数
-q|POSIX消息队列字节数
-r|实时nice优先级
-s|栈KB数
-t|CPU秒数
-u|用户进程数
-v|虚拟内存KB数
-x|文件锁数
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
# Globbing
* 先解释，再执行；命令接受展开后的参数
* 若文件名匹配不存在，则不进行扩展
## \`命令\` 或 $(命令) 命令的标准输出
## ~ 家目录
## ~+ 当前目录
## ~用户 用户家目录
## !! 上一条命令
```sh
sudo !!
PYTHONIOENCODING=utf-8 !!
```
## $变量 或 ${变量} 变量值
### $@ 当前函数实参数组
### $# 当前函数实参数
### $$ 当前shell的PID
### $? 上一条命令的返回值
## $((整数运算)) 运算结果
## ${!前缀*} 或 ${!前缀@} 变量名
## * 0或多个字符
* 不匹配隐藏文件
* 不匹配路径分隔符
## ** 0或多层子目录
* 需开启globstar
## {字符串1,...,字符串n} 每个字符串出现一次
* 字符串可为空
* 大括号可以嵌套
* 与其他模式组合时，优先展开大括号
## {起点..终点[..步长]} 区间内字符串出现一次
* 支持逆序
* 无法迭代则原样输出
* 起点包含前导0，则所有字符串被扩展为等长，长度由最长字符串决定
## [字符1...字符n] 集合中单字符
* `-`只能放在开头或结尾
## [起点1-终点1...] 连续集合中单字符
## [!...] 或 [^...] 集合外单字符
## [![:字符类:]] 类外单字符
## [[:字符类:]] 类中单字符
字符类|意义
-|-
alnum|字母和数字
alpha|字母
blank|空格和tab
cntrl|ASCII 0-31不可打印字符
digit|数字
graph|字母、数字和标点
lower|小写字母
print|ASCII 32-127可打印字符
punct|标点
space|空格、tab、LF、VT、FF、CR
upper|大写字母
xdigit|A-F、a-f、数字
## ? 单个非空字符
* 不匹配路径分隔符
# 符号
## && 前一条命令成功则执行后一条
## -- 分隔选项和参数
```sh
kill -9 -- -进程组号
```
## || 前一条命令失败则执行后一条
## \ 多行命令
## ; 同行多命令
## <<EOF 多行输入，直到遇到EOF
```sh
cat <<EOF >文件
内容
EOF
```
# 环境变量
## BASH_VERSION bash版本
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
Alt+D|剪切光标右侧的单词
Alt+F|移动光标到右侧（含）单词尾字母后
Alt+R|取消对来自history的命令的修改
Ctrl+C|发送SIGINT
Ctrl+D|发送EOF
Ctrl+K|剪切光标及右侧
Ctrl+L|清屏
Ctrl+R|搜索历史命令
Ctrl+U|剪切光标左侧
Ctrl+W|剪切光标左侧的单词
Ctrl+Y|粘贴
Ctrl+Z|暂停
Ctrl+/|撤销
