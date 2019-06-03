# 检查
## CPU支持
```sh
egrep -c '(vmx|svm)' /proc/cpuinfo # 需要大于0
```
## 硬件加速
```sh
apt install cpu-checker
kvm-ok
```
