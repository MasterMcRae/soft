# 二手车停车场交易管理系统 V2

**2026版 · 企业级数据管理平台**

---

## 项目概述

二手车停车场交易管理系统是一个面向二手车交易市场的全流程数字化管理平台。系统以停车场车辆流转为核心，覆盖车辆进场识别、车商绑定、在售管理、交易闭环、统计分析等完整业务链路，并支持臻云摄像枪设备接入与分区映射管理。

核心业务流程：车辆进场（自动/手动）→ 待绑定管理 → 绑定车商 → 在售流转 → 售出 → 车牌变更记录 → 统计分析。

---

## 技术栈

| 层级 | 技术选型 | 说明 |
|------|----------|------|
| 后端框架 | **FastAPI 0.104** | 高性能异步 Web 框架，内置 OpenAPI 文档 |
| 数据库 | **SQLite** | 轻量级嵌入式数据库，零配置部署 |
| 前端 | **Vanilla JS (原生JavaScript)** | 无框架依赖，单文件 SPA 应用 |
| 身份认证 | **JWT (python-jose)** | 基于 JWT 的 Token 认证，支持角色权限控制 |
| 密码加密 | **SHA-256 + passlib** | 用户密码安全存储 |
| IoT 接入 | **MQTT (paho-mqtt)** | 臻云摄像枪设备消息队列接入 |
| API 交互 | **httpx** | 后端到臻云 API 的 HTTP 客户端 |

---

## 功能模块

### 1. 仪表盘 (Dashboard)
- 实时统计卡片：待绑定、在售、已售、外部车辆数量
- 今日操作日志统计
- 最近出入记录时间线
- 支持管理员全量视图 / 车商专属视图自动切换

### 2. 出入管理
- 手动模拟车辆进场登记（车牌号 + 分区 + 设备ID）
- 自动判定车辆状态：新车归入待绑定、在售车辆增加流转次数、外来车辆隔离、已售车辆仅记录
- 出入记录查询
- MQTT 回调接口（臻云摄像枪自动识别）

### 3. 待绑定管理
- 新进场车辆的集中管理视图
- 一键绑定至车商（指定车商 + 备注）
- 解绑操作（仅管理员，回到待绑定状态）
- 绑定后自动变更为「在售」状态

### 4. 在售车辆管理
- 全量车辆列表（支持按状态/分区/车商/车牌/品牌搜索）
- 车辆详情查看
- 售出操作：录入成交金额、买家姓名、买家电话
- 车牌变更记录：已售车辆更新新牌照号，原牌自动屏蔽
- 流转次数追踪

### 5. 交易闭环
- 售出车辆完整交易信息留存
- 成交金额自动汇总
- 车牌变更历史追踪
- 操作日志全链路可追溯

### 6. 车商管理
- 车商列表（含车辆数/已售数统计）
- 新增/编辑车商信息（名称、分区、电话）
- 启停用车商状态
- 新增车商时自动创建对应登录账号

### 7. 统计分析
- 销售概览：今日/本月/本年销量与金额
- 趋势图表：周/月/年维度销量与金额折线
- 车商对比：各车商在售数、已售数、成交额、流转量排名
- 已售明细：按品牌、日期范围筛选已售车辆

### 8. 报表
- 车辆汇总报表（状态/车商/关键词筛选）
- 车商月度汇总报表（在售数/已售数/流转量/绑定数）

### 9. 臻云配置
- AccessKey ID/Secret 配置
- MQTT 连接参数（主机、端口、用户名、密码）
- 连接启停控制
- 连接测试
- 图片下载URL生成

### 10. 设备分区映射
- 臻云摄像枪序列号 → 停车场分区映射
- 支持入口/出口方向标记
- 设备增删改查及启停管理
- 按序列号快速查询分区

### 11. 账号管理
- 管理员可创建/编辑/删除用户账号
- 三种角色：admin（管理员）、operator（岗亭操作员）、dealer（车商）
- 角色权限隔离：车商仅看自己数据，操作员可操作出入登记
- 默认账号：admin / 123456

---

## 快速启动

### 环境要求

- Python 3.10+
- Windows / Linux / macOS

### 1. 安装依赖

```bash
cd backend
pip install -r requirements.txt
```

### 2. 启动后端

```bash
cd backend
python main.py
```

服务启动后：
- API 地址：`http://localhost:8080`
- API 文档（Swagger）：`http://localhost:8080/docs`
- 前端页面：`http://localhost:8080`（自动挂载 frontend/index.html）

### 3. 直接打开前端（开发调试）

也可以直接用浏览器打开 `frontend/index.html`，前端会通过 JavaScript 连接 `http://localhost:8080` 的后端 API。

### 4. 默认登录账号

| 用户名 | 密码 | 角色 | 说明 |
|--------|------|------|------|
| admin | 123456 | 超级管理员 | 全部功能权限 |
| operator | 123456 | 岗亭操作员 | 出入登记操作 |
| dealer1 | 123456 | 鑫达二手车 | A区车商视图 |
| dealer2 | 123456 | 永信车行 | A区车商视图 |
| dealer3 | 123456 | 诚信二手车 | B区车商视图 |

---

## 目录结构

```
V2/
├── README.md                          # 本文件 - 项目说明文档
├── backend/                           # 后端应用
│   ├── main.py                        # FastAPI 主入口，路由注册，启动配置
│   ├── database.py                    # 数据库初始化，建表，种子数据
│   ├── models.py                      # Pydantic 数据模型定义
│   ├── auth.py                        # JWT 认证：Token生成/验证/用户认证
│   ├── vzicloud.py                    # 臻云 MQTT 客户端与API交互
│   ├── requirements.txt               # Python 依赖清单
│   ├── used_car_system.db             # SQLite 数据库文件（运行时数据）
│   └── routers/                       # API 路由模块
│       ├── __init__.py                # 包初始化
│       ├── auth_router.py             # 认证路由（登录/用户管理）
│       ├── dashboard.py               # 仪表盘统计路由
│       ├── vehicles.py               # 车辆管理路由（绑定/售出/车牌变更）
│       ├── entry.py                   # 出入管理路由（模拟进出/MQTT回调）
│       ├── dealers.py                # 车商管理路由
│       ├── stats.py                  # 统计分析路由
│       ├── reports.py                # 报表路由
│       ├── logs.py                   # 操作日志路由
│       ├── vzicloud_config.py        # 臻云配置路由
│       └── device_zones.py           # 设备分区映射路由
└── frontend/                          # 前端应用
    └── index.html                     # 单文件 SPA（HTML + CSS + JS）
```

---

## 数据库设计

系统使用 SQLite，数据库文件位于 `backend/used_car_system.db`。启动时自动初始化表结构及种子数据。

### 数据表

| 表名 | 说明 | 核心字段 |
|------|------|----------|
| `vehicles` | 车辆主表 | plate, status, dealer_id, brand, color, entry_time, sold_amount, flow_count |
| `dealers` | 车商表 | id, name, zone, phone, status |
| `entry_records` | 出入记录 | plate, vehicle_id, direction, zone, device_id, time |
| `operation_logs` | 操作日志 | action, detail, operator, time |
| `users` | 用户账号 | id, name, role, password_hash, dealer_id, zone |
| `vzicloud_config` | 臻云配置 | access_key_id, mqtt_host, enabled |
| `device_zones` | 设备分区映射 | device_serialno, zone, direction, status |
| `recognition_logs` | 识别日志 | plate, raw_data, device_id, time |

### 车辆状态流转

```
new（新车进场） → pending（待绑定） → forsale（在售） → sold（已售）
                                             ↑
                                        external（外来车辆，隔离）
```

---

## API 路由总览

| 路由前缀 | 标签 | 说明 |
|----------|------|------|
| `/api/auth` | 认证 | 登录、获取用户信息、账号 CRUD |
| `/api/dashboard` | 仪表盘 | 统计卡片、最近出入记录 |
| `/api/vehicles` | 车辆管理 | 列表查询、绑定、解绑、售出、车牌变更 |
| `/api/entry` | 出入管理 | 模拟进出登记、记录查询、MQTT 回调 |
| `/api/dealers` | 车商管理 | 车商 CRUD、启停用 |
| `/api/stats` | 统计分析 | 销售概览、趋势图表、车商对比、已售明细 |
| `/api/reports` | 报表 | 车辆汇总、车商月度报表 |
| `/api/logs` | 操作日志 | 操作日志查询 |
| `/api/vzicloud` | 臻云配置 | 配置读写、连接测试、图片URL |
| `/api/device-zones` | 设备分区 | 设备-分区映射 CRUD、按序列号查询 |

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| V2 | 2026-05 | 完善开发文档，优化目录结构，新增 V2 打包版本 |
| V1 | 2026-04 | 初始版本，完整功能实现 |