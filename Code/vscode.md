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
    "code-runner.saveFileBeforeRun": true,
    "code-runner.runInTerminal": true,
    "editor.fontFamily": "consolas",
    "powermode.enabled": true,
    "powermode.presets": "flames",
    "workbench.colorTheme": "Abyss",
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
# Java
## 扩展
Java Extension Pack
# Markdown
## 扩展
Markdown PDF
## 配置
Markdown-pdf: Executable Path设置为Chrome.exe的位置
```json
{
    "markdown.preview.fontFamily": "consolas",
    "markdown-pdf.displayHeaderFooter": false
}
```
## 换页
```html
<div style="page-break-after: always;"></div>

```
## 快捷键
功能|命令
-|-
预览|Ctrl+K V
## 导出PDF
右键或命令 Markdown PDF: Export (pdf)
# Python
## 扩展
Python
## 配置
```json
{
    "python.linting.enabled": true,
    "python.formatting.provider": "autopep8",
    "code-runner.executorMap": {
        "python": "python3 -u",
    },
}
```
