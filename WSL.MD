# 常用命令与工具速查手册

> 涵盖 WSL / Linux / Git / 构建与运行 / 网络代理 / 机器人调试 / 运维排错等常用操作

---

## 1. WSL（Windows Subsystem for Linux）

### 1.1 基本管理命令

```bash
wsl -l -o            # 显示可安装的发行版
wsl -l -v            # 显示已安装发行版及版本
wsl -d <发行版名>     # 启动指定发行版
wsl -t <发行版名>     # 关闭指定发行版
wsl --shutdown       # 关闭所有 WSL 实例
wsl --cd /           # 从 Windows 进入 Linux 根目录
```

示例：

```bash
wsl -d Ubuntu-20.04
```

---

## 2. Linux 常用系统命令

### 2.1 用户与权限

```bash
sudo -i              # 切换到 root
```

### 2.2 目录与权限

```bash
sudo mkdir -p /home/data/logs/
sudo chmod 777 /home/data/logs
```

---
