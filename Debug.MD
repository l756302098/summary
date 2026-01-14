# 常用命令与工具速查手册

---

## 运行环境变量（LD_LIBRARY_PATH）

### 1 通用配置方式

```bash
ROOT_DIR=/home/vecow/workspace/hybrid-prototype
export LD_LIBRARY_PATH=$ROOT_DIR/build/bin:$ROOT_DIR/build/install/lib:$LD_LIBRARY_PATH
```

### 2 OpenMind 依赖

```bash
export LD_LIBRARY_PATH=/opt/openmind/lib:$LD_LIBRARY_PATH
```

---

## 构建 / 运行 / 测试脚本

### 1 行为树 / 机器人测试

```bash
./simple_locomotion_bt_trigger -f config/tests/test_aubo_WH.yaml -p post_wp_1_1
./simple_locomotion_bt_trigger -p home photo_wp_up post_pick_wp_up
./manip_bt_trigger -o QR0010
./simple_bt_trigger sps_WH   # 全流程
```

### 2 夹爪 / 输送带控制

```bash
./simple_bt_trigger gripper_open
./simple_bt_trigger enable_jodell_gripper

# 输送带
./simple_bt_trigger belt_forward
./simple_bt_trigger belt_reverse
./simple_bt_trigger belt_off
```

---

## 打包 / 解压 / 传输

### 1 打包与解压

```bash
tar -czvf hybrid-prototype.tar.gz hybrid-prototype/build/bin hybrid-prototype/build/install
tar -zxf hybrid-prototype.tar.gz
```

### 2 SCP 文件传输

```bash
scp hybrid-prototype.tar.gz vecow@10.111.147.68:/home/vecow/workspace
```

---

## 运维 / 排错 / 进程管理

### 1 杀进程（机器人系统）

```bash
killall -9 ros2_ebox_bridge
killall -9 OmBtMagrServer
killall -9 OpenmindVelocityCommand
killall -9 OpenmindControllerServer
killall -9 OpenmindPlannerServer
killall -9 OpenmindCostmapServer
killall -9 slam_main
killall -9 LocomotionServer
```

### 2 按名称杀进程

```bash
kill_by_name.sh grasp
kill_by_name.sh vision
```

---

## 其他零散但常用

```bash
./fake_vel_cmd_pub /cmd_vel -0.2   # 发送移动指令
sudo gparted                     # 分区工具
```

---
