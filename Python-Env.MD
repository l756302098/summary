在Python工程中使用虚拟环境管理依赖是**最佳实践**，以下是完整的使用方法：

## 1. **创建虚拟环境**

### 方法一：使用 `venv`（Python 3.3+ 内置）
```bash
# 在项目根目录创建虚拟环境
python -m venv venv

# 指定Python版本（如果有多个版本）
python3.8 -m venv venv
python3.11 -m venv venv
```

### 方法二：使用 `virtualenv`（更灵活）
```bash
# 安装virtualenv
pip install virtualenv

# 创建虚拟环境
virtualenv venv
virtualenv -p python3.9 venv  # 指定Python版本
```

## 2. **激活虚拟环境**

### Windows (PowerShell)
```powershell
venv\Scripts\Activate.ps1
# 如果报错执行权限问题，先运行：
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Windows (CMD)
```cmd
venv\Scripts\activate.bat
```

### Linux/macOS
```bash
# bash/zsh
source venv/bin/activate

# fish
source venv/bin/activate.fish

# csh/tcsh
source venv/bin/activate.csh
```

激活后，命令行提示符会显示虚拟环境名称：`(venv) user@host:~$`

## 3. **管理依赖**

### 安装依赖到虚拟环境
```bash
# 激活虚拟环境后安装
pip install package_name
pip install package_name==1.2.3  # 指定版本

# 从requirements.txt安装
pip install -r requirements.txt

# 安装开发依赖
pip install pytest black flake8 --dev
```

### 生成依赖文件
```bash
# 生成 requirements.txt
pip freeze > requirements.txt

# 使用 pip-tools 生成更精确的依赖
pip install pip-tools
pip-compile requirements.in  # 从requirements.in生成requirements.txt

# 生成带hash值的requirements（用于生产环境）
pip freeze --all --exclude-editable > requirements.txt
```

## 4. **项目结构示例**
```
my_project/
├── venv/                 # 虚拟环境目录（建议.gitignore）
├── .gitignore           # 忽略虚拟环境
├── requirements.txt     # 生产依赖
├── requirements-dev.txt # 开发依赖
├── src/                 # 源代码
└── pyproject.toml      # 现代Python项目配置
```

## 5. **`.gitignore` 配置**
```
# 虚拟环境
venv/
env/
ENV/
.venv/

# Python缓存文件
__pycache__/
*.py[cod]
*$py.class

# 安装包
*.egg
*.egg-info/
dist/
build/
```

## 6. **使用 `pyproject.toml`（现代方式）**

创建 `pyproject.toml`：
```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-project"
version = "0.1.0"
dependencies = [
    "requests>=2.28.0",
    "numpy>=1.21.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=22.0.0",
    "flake8>=5.0.0",
]

[project.scripts]
my-cli = "my_project.main:cli"
```

安装依赖：
```bash
# 安装项目依赖
pip install -e .

# 安装开发依赖
pip install -e ".[dev]"
```

## 7. **退出虚拟环境**
```bash
deactivate
```

## 8. **使用 Makefile 或脚本自动化**

创建 `Makefile`：
```makefile
.PHONY: setup test clean

setup:
    python -m venv venv
    source venv/bin/activate && pip install -e ".[dev]"

test:
    source venv/bin/activate && pytest

clean:
    rm -rf venv
    find . -type d -name "__pycache__" -exec rm -rf {} +
```

或创建 `setup.sh`（Linux/macOS）：
```bash
#!/bin/bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

## 9. **VS Code 配置虚拟环境**

创建 `.vscode/settings.json`：
```json
{
    "python.defaultInterpreterPath": "${workspaceFolder}/venv/bin/python",
    "python.terminal.activateEnvironment": true,
    "python.terminal.activateEnvInCurrentTerminal": true
}
```

## **最佳实践建议**

1. **每个项目独立环境**：避免全局安装Python包
2. **记录精确版本**：使用 `pip freeze` 或 `pip-tools`
3. **分离开发依赖**：生产环境只需必要的包
4. **使用环境变量**：配合 `.env` 文件管理配置
5. **容器化部署**：生产环境使用Docker
   ```dockerfile
   FROM python:3.11-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   COPY . .
   ```

这样就能确保项目依赖的隔离性和可复现性。