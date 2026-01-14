# 常用命令与工具速查手册

---

## 静态检查与工具

### 1 cppcheck

```bash
cppcheck --enable=all --inconclusive --std=c++11 ./ 2> cppcheck_report.txt
```

### 2 删除 Windows Zone 标记文件

```bash
find . -type f -name "*Zone.Identifier" -delete
```

---

## 其他零散但常用

```bash
./fake_vel_cmd_pub /cmd_vel -0.2   # 发送移动指令
sudo gparted                     # 分区工具
```

---
