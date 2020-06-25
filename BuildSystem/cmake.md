# 命令
## 生成构建系统
```sh
cmake [选项] 源目录
cmake [选项] 已存在的构建目录
cmake [选项] -S 源目录 -B 构建目录
```
## 构建项目
```sh
cmake --build 构建目录 [选项] [-- 构建工具选项]
```
## 运行命令行工具
```sh
cmake -E 命令 [选项]
```
# CMakeLists.txt
* 命令大小写不敏感
```cmake
cmake_minimum_required(VERSION 3.10)
project(项目 VERSION 版本)
add_executable(项目 源文件)
```
