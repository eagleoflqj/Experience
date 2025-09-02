设置 -> 系统 -> 可选功能 -> 查看功能 -> 查看可用功能 -> OpenSSH 服务器

服务 -> OpenSSH SSH Server -> 启动类型：自动，启动

存放公钥于以下地址并执行
```pwsh
icacls "C:\ProgramData\ssh\administrators_authorized_keys" /inheritance:r
icacls "C:\ProgramData\ssh\administrators_authorized_keys" /grant "Administrators:F"
icacls "C:\ProgramData\ssh\administrators_authorized_keys" /grant "SYSTEM:F"
Restart-Service sshd
```
