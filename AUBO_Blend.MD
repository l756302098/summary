# AUBO 机械臂joint 移动过程中 blend 的作用

> 主要从速度、末端法兰位姿两方面进行分析

---

## blend 对时间的影响

### blend 为 0.4 时的连续运动时间

```bash
[2026-01-13 10:43:56.582] [user] [debug] receive QueryMode.
[2026-01-13 10:43:56.584] [user] [debug] receive movex action.
[2026-01-13 10:43:56.584] [user] [debug] AUBO_tool_pose Joints: -0.421 -0.018 0.333 -3.130 -0.024 -1.487 
[2026-01-13 10:43:56.584] [user] [debug] actual_tcp_speed Joints: 0.000 0.000 -0.000 0.000 -0.000 0.000 
[2026-01-13 10:43:56.584] [user] [debug] Line number set to: 40
[2026-01-13 10:43:56.584] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 10:44:00.532] [user] [info] BlendMoveX: robot free done
[2026-01-13 10:44:00.532] [user] [info] AuboPlugin MoveX success
```

### blend 为 0.4 时的逐点运动时间

```bash
[2026-01-13 10:45:04.509] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 10:45:04.509] [user] [debug] receive QueryMode.
[2026-01-13 10:45:04.511] [user] [debug] receive movex action.
[2026-01-13 10:45:04.511] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 10:45:09.049] [user] [info] MoveJoints: robot free done
[2026-01-13 10:45:09.049] [user] [info] AuboPlugin MoveX success
```

| 运动方式             | waypoints | blend | 总耗时          |
| ---------------- | --------- | ----- | ------------ |
| 连续运动（BlendMoveX） | 5         | 0.4   | **≈ 3.95 s** |
| 逐点运动（MoveJoints） | 5         | 0.4   | **≈ 4.54 s** |

### 结论分析

1. 连续运动比逐点运动快约 0.6 秒  
    - 节省时间 ≈ 13%  

2. 即使在 blend 相同（0.4）、waypoints 相同（5） 的情况下  
    - 连续运动减少了 每个 waypoint 的停顿 / 减速 / 重新加速

3. 逐点运动中：  
    - 每个点都更接近 “stop-and-go” 行为
    - 即使设置了 blend，也难以达到真正的轨迹连续性
    - 即使设置了 blend，也会达到目标点

### blend 为 0.2时的连续运动时间

```bash
[2026-01-13 10:42:46.876] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 10:42:46.876] [user] [debug] receive QueryMode.
[2026-01-13 10:42:46.877] [user] [debug] receive movex action.
[2026-01-13 10:42:46.877] [user] [info] MoveXRequest waypoints size: 5

[2026-01-13 10:42:50.825] [user] [info] BlendMoveX: robot free done
[2026-01-13 10:42:50.825] [user] [info] AuboPlugin MoveX success
```

### blend 为 0.2时的逐点运动时间

```bash
[2026-01-13 10:41:31.508] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 10:41:31.508] [user] [debug] receive QueryMode.
[2026-01-13 10:41:31.510] [user] [debug] receive movex action.
[2026-01-13 10:41:31.510] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 10:41:36.050] [user] [info] MoveJoints: robot free done
[2026-01-13 10:41:36.050] [user] [info] AuboPlugin MoveX success
```

| 运动方式             | waypoints | blend | 总耗时          |
| ---------------- | --------- | ----- | ------------ |
| 连续运动（BlendMoveX） | 5         | 0.2   | **≈ 3.95 s** |
| 逐点运动（MoveJoints） | 5         | 0.2   | **≈ 4.54 s** |

### 结论分析

1. 在 blend = 0.2 时  
    - 连续运动依然比逐点运动 快约 0.6 秒
    - 时间优势与 blend = 0.4 的结果几乎一致

2. 在该轨迹和速度参数下，blend 0.2 → 0.4 对总时间几乎无影响，但是否使用连续轨迹影响显著。

### blend 为 0.1时的连续运动时间

```bash
[2026-01-13 10:37:55.702] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 10:37:55.702] [user] [debug] receive QueryMode.
[2026-01-13 10:37:55.704] [user] [debug] receive movex action.
[2026-01-13 10:37:55.705] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 10:37:59.851] [user] [info] BlendMoveX: robot free done
[2026-01-13 10:37:59.851] [user] [info] AuboPlugin MoveX success
```

### blend 为 0.1时的逐点运动时间

```bash
[2026-01-13 10:39:41.906] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 10:39:41.906] [user] [debug] receive QueryMode.
[2026-01-13 10:39:41.908] [user] [debug] receive movex action.
[2026-01-13 10:39:41.908] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 10:39:46.450] [user] [info] MoveJoints: robot free done
[2026-01-13 10:39:46.450] [user] [info] AuboPlugin MoveX success
```

| 运动方式             | waypoints | blend | 总耗时          |
| ---------------- | --------- | ----- | ------------ |
| 连续运动（BlendMoveX） | 5         | 0.1   | **≈ 4.15 s** |
| 逐点运动（MoveJoints） | 5         | 0.1   | **≈ 4.54 s** |

### 结论分析

1. 连续运动仍然更快  
    - 时间优势 ≈ 0.39 秒
    - 相比 blend = 0.2 / 0.4（≈ 0.6 s），优势略有缩小

2. blend 变小，连续性的收益下降
    - blend = 0.1 时，轨迹在 waypoint 处的减速更明显
    - 但仍未退化为完全逐点 stop-and-go
3. 逐点运动时间几乎不受 blend 影响
    - 0.1 / 0.2 / 0.4 都稳定在 ≈ 4.54 s
    - 说明逐点执行的主要耗时来自每段独立规划与加减速

### blend 为 0时的连续运动时间

```bash
[2026-01-13 11:14:21.255] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 11:14:21.255] [user] [debug] receive QueryMode.
[2026-01-13 11:14:21.256] [user] [debug] receive movex action.
[2026-01-13 11:14:21.256] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 11:14:25.698] [user] [info] BlendMoveX: robot free done
[2026-01-13 11:14:25.698] [user] [info] AuboPlugin MoveX success
```

### blend 为 0时的逐点运动时间

```bash
[2026-01-13 11:12:59.557] [user] [debug] receive ExecuteQueryMode.
[2026-01-13 11:12:59.557] [user] [debug] receive QueryMode.
[2026-01-13 11:12:59.559] [user] [debug] receive movex action.
[2026-01-13 11:12:59.559] [user] [info] MoveXRequest waypoints size: 5
[2026-01-13 11:13:04.100] [user] [info] MoveJoints: robot free done
[2026-01-13 11:13:04.100] [user] [info] AuboPlugin MoveX success
```

| 运动方式             | waypoints | blend | 总耗时          |
| ---------------- | --------- | ----- | ------------ |
| 连续运动（BlendMoveX） | 5         | 0     | **≈ 4.44 s** |
| 逐点运动（MoveJoints） | 5         | 0     | **≈ 4.54 s** |

### 结论分析

1. 当 blend = 0 时，两者时间几乎一致  
    - 连续运动仅快 ≈ 0.10 秒
    - 已基本退化为逐点 stop-and-go

2. 与前面结果形成清晰对比  

| blend | 连续运动         | 逐点运动         | 时间优势         |
| ----- | ------------ | ------------ | ------------ |
| 0.4   | ≈ 3.95 s     | ≈ 4.54 s     | ≈ 0.59 s     |
| 0.2   | ≈ 3.95 s     | ≈ 4.54 s     | ≈ 0.59 s     |
| 0.1   | ≈ 4.15 s     | ≈ 4.54 s     | ≈ 0.39 s     |
| **0** | **≈ 4.44 s** | **≈ 4.54 s** | **≈ 0.10 s** |

---

## 2. 从法兰位姿的角度分析
