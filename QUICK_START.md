# 快速启动指南

## 环境要求

- **Python**：3.10 及以上版本
- **操作系统**：Windows / Linux / macOS
- **浏览器**：Chrome / Edge / Firefox 等现代浏览器

## 安装与启动

### 第一步：解压/复制项目

将 `V2` 目录放到任意位置，例如 `E:\软件开发\二手车交易系统\V2`。

### 第二步：安装 Python 依赖

打开终端（PowerShell / CMD / Bash），进入 backend 目录：

```powershell
cd E:\软件开发\二手车交易系统\V2\backend
pip install -r requirements.txt
```

### 第三步：启动后端服务

```powershell
python main.py
```

启动成功后会看到类似输出：

```
[Server] 数据库初始化完成
[Server] API 文档: http://localhost:8080/docs
```

### 第四步：访问系统

| 入口 | 地址 |
|------|------|
| 管理系统前端 | http://localhost:8080 |
| API 交互文档 (Swagger) | http://localhost:8080/docs |

> **提示**：如果后端未启动，也可以直接用浏览器打开 `frontend/index.html` 查看前端界面（部分功能需要后端支持）。

## 默认账号

| 用户名 | 密码 | 角色 | 数据范围 |
|--------|------|------|----------|
| `admin` | `123456` | 超级管理员 | 全部数据 |
| `operator` | `123456` | 岗亭操作员 | 出入登记操作 |
| `dealer1` | `123456` | 鑫达二手车 | A区自有车辆 |
| `dealer2` | `123456` | 永信车行 | A区自有车辆 |
| `dealer3` | `123456` | 诚信二手车 | B区自有车辆 |
| `dealer4` | `123456` | 宏达汽贸 | B区（已停用） |
| `dealer5` | `123456` | 宝驰名车 | A区自有车辆 |

## 常见问题

### 端口被占用

如果 8080 端口已被占用，修改 `backend/main.py` 最后一行的端口号：

```python
uvicorn.run(app, host="0.0.0.0", port=9090, log_level="info")
```

### 数据库重置

删除 `backend/used_car_system.db` 文件，重新启动后端即可自动重建数据库并初始化种子数据。

### 依赖安装失败

确保 Python 版本 ≥ 3.10，并使用国内镜像加速：

```powershell
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```