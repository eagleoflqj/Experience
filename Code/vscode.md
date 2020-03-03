# 全局
## 快捷键
功能|命令
-|-
启动调试|F5
非调试启动|Ctrl+F5
命令|Ctrl+Shift+P
代码格式化|Alt+Shift+F
转到定义|F12
颜色主题|Ctrl+K Ctrl+T
## 扩展
* Code Runner
* Power Mode
## 配置
```json
{
    "editor.fontFamily": "consolas",
    "workbench.colorTheme": "Abyss",
    "powermode.enabled": true,
    "powermode.presets": "flames",
    "code-runner.saveFileBeforeRun": true,
}
```
# 汇编
## 扩展
x86 and x86_64 Assembly
# C/C++
## 扩展
C/C++  
## 快捷键
功能|命令
-|-
运行|Ctrl+Alt+N
# markdown
## 扩展
Markdown PDF
## 配置
Markdown-pdf: Executable Path设置为Chrome.exe的位置
```json
{
    "markdown.preview.fontFamily": "consolas",
}
```
## 快捷键
功能|命令
-|-
预览|Ctrl+K V
## 导出PDF
右键或命令 Markdown PDF: Export (pdf)
# python
## 扩展
Python
## 配置
```json
{
    "python.linting.enabled": true,
    "code-runner.executorMap": {
        "python": "python3 -u",
    },
}
```
