# Claude 使用与环境配置整理

本文对原始文档内容进行结构化整理，突出 **Claude 使用技巧**，并对环境配置部分进行归类说明，便于长期查阅与实践。

---

## 一、基础环境配置

### 1. WSL 网络与代理设置

#### 1.1 网络模式

* **WSL Settings → Network → Mirrored 模式**
* 适合与宿主机代理/VPN 配合使用，减少网络异常

#### 1.2 临时代理设置（当前会话有效）

```bash
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
export all_proxy="socks5://127.0.0.1:7890"
```

#### 1.3 永久代理设置（写入 ~/.bashrc）

```bash
echo 'export http_proxy=http://127.0.0.1:7890' >> ~/.bashrc
echo 'export https_proxy=http://127.0.0.1:7890' >> ~/.bashrc
echo 'export all_proxy=socks5://127.0.0.1:7890' >> ~/.bashrc
source ~/.bashrc
```

---

### 2. VPN 相关

* 登录地址：

  ```
  https://sbnil.mxwlonline.top/auth/login
  http://www3.mxwljsq.com/auth/login
  ```

* IP 风险检测：

  ```
  ip111.cn
  ```

---

## 二、Claude Code 安装与资料

* 官网：

  * [https://claude.com/product/claude-code](https://claude.com/product/claude-code)
* 官方文档：

  * [https://code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview)

---

## 三、Claude 使用技巧（重点）

> 本节为核心内容，适合反复阅读与实践。

---

### 1. 使用 `CLAUDE.md` 管理规则（强烈推荐）

**目的：让 Claude 持续对齐你的偏好与项目规范**

* 使用 `/init` 初始化项目时，会自动生成 `CLAUDE.md`
* Claude 会在每次执行任务时读取该文件

**推荐用途：**

* 项目背景说明
* 编码风格 / 语言偏好
* 常见错误总结（“错题本”）
* 不希望 Claude 重复犯的错误

> 本质上：给 AI 一本长期有效的「项目说明书 + 复盘笔记」

---

### 2. 使用 Git 管理与 Claude 的协作过程

**为什么要配合 Git？**

* 清楚看到 Claude 改了哪些文件
* 便于 review / 回滚 / 对比方案
* 防止 AI“好心办坏事”

**推荐习惯：**

* 每次让 Claude 改动前先 `git status`
* 一次只让 Claude 做一类改动
* 改完立即 `git diff` 查看

---

### 3. 使用 `@路径` 精确指定文件

* 在对话中使用：

  ```
  @src/main.cpp
  @config/config.yaml
  ```

**好处：**

* Claude 不需要猜文件位置
* 避免“改错文件 / 重复生成代码”
* 大型项目中效率提升明显

---

### 4. Shift + Tab：计划模式（Plan Mode）

**这是 Claude Code 中最容易被忽略、但极其重要的能力**

* 连续按 **两次 Shift + Tab**
* 进入 **Plan Mode（计划模式）**

**适合场景：**

* 架构设计
* 重构方案
* 多文件联动修改
* 复杂需求拆解

**正确用法：**

1. 先不写代码
2. 与 Claude 像“产品经理 + 架构师”一样反复对齐方案
3. 明确：目标、边界、风险、修改点
4. 确认方案无误后，再进入执行

> 计划清晰程度 ≈ 最终代码质量

---

### 5. 权限管理（安全与稳定性）

**不要做的事：**

* ❌ 使用 `--dangerously-skip-permissions`

这相当于让 AI 在你的机器上“裸奔”。

**推荐做法：**

* 使用 `/permissions`
* 将确认安全的命令加入白名单，例如：

  * `bun run test`
  * `bun run build`

**收益：**

* 降低误删 / 误执行风险
* 提高对 Claude 操作的可控性

---

### 6. 同时运行多个 Claude 实例

**适用场景：**

* 一个实例写代码
* 一个实例查文档 / 做方案
* 一个实例做测试或重构建议

**好处：**

* 减少等待
* 并行思考
* 提高整体效率

---

### 7. 模型选择建议

* **Opus**

  * 复杂架构设计
  * 深度重构
  * 难问题分析

* **Sonnet**

  * 日常编码
  * 小功能修改
  * 快速验证想法

---

### 8. 核心原则：没有验证，就没有质量

* AI 输出 ≠ 正确结果
* 必须：

  * 运行测试
  * 编译验证
  * 逻辑 review

> 把 Claude 当成**高级实习生 / 合作工程师**，而不是“真理机器”

---

## 四、推荐使用心法（总结）

* 用 `CLAUDE.md` 管长期记忆
* 用 Plan Mode 管复杂问题
* 用 Git 管风险
* 用权限系统管安全
* 用验证机制管质量

**Claude 的上限，取决于你是否把它当成一个“系统”来使用。**
