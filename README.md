# Firefly Docs

> Firefly-LM 官方文档仓库

## 目录

- [v0.1 调度系统技术设计](./docs/v0.1-scheduler-design.md)

## 调度系统 v0.1 设计摘要

### 系统架构

`
[Client/Node] --HTTPS--> [API Gateway / FastAPI] ---> [PostgreSQL]
                     |                              [Redis]
                     +---> [MinIO (S3)]
`

### 数据库模型

| 表 | 说明 |
|---|---|
| users | 用户 / JWT / 积分余额 |
| 
odes | 节点注册信息 / 等级 / 信誉 |
| 	asks | 任务状态机 |
| contribution_logs | 积分流水（不可篡改） |

### 核心 API

| 方法 | 路由 | 说明 |
|---|---|---|
| POST | /api/v1/auth/register | 注册 |
| POST | /api/v1/auth/login | 登录拿 JWT |
| POST | /api/v1/node/register | 注册节点（自动评级） |
| POST | /api/v1/node/heartbeat | 心跳（30s 一次） |
| GET | /api/v1/task/available | 可领取任务列表 |
| POST | /api/v1/task/claim | 乐观锁领取任务 |
| POST | /api/v1/task/progress | 进度更新（延长 TTL） |
| POST | /api/v1/task/submit | 提交结果 + 结算积分 |
| POST | /api/v1/admin/tasks | 手动创建任务 |
| GET | /api/v1/admin/stats | 全局统计 |

### 任务状态机

`
pending → running → completed
              ↓
            failed → (≤3 retry) → pending
`

### 核心能力

- JWT + bcrypt 认证
- 节点自动分级（L1-L3）
- Redis SETNX 分布式任务锁
- PostgreSQL FOR UPDATE 行锁防竞态
- 后台清理：超时回收 / 离线检测 / 信誉恢复
- 积分原子结算（SELECT FOR UPDATE）

## 相关仓库

- [scheduler](https://github.com/firefly-lm-org/scheduler) — 调度中心 API
- [firefly-client](https://github.com/firefly-lm-org/firefly-client) — 节点客户端
- [infra](https://github.com/firefly-lm-org/infra) — 基础设施
