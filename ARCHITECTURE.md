# Firefly LM · 系统架构

## 组件概览

```
                        ┌─────────────────────────────────────────────┐
                        │           Firefly Scheduler (API)            │
                        │  FastAPI · PostgreSQL · Redis · MinIO       │
                        │  http://scheduler.firefly-ai.org:8000       │
                        └──────────────────┬──────────────────────────┘
                                           │
                     ┌─────────────────────┼─────────────────────┐
                     │  心跳/领取/上报     │                     │
                     ▼                     ▼                     ▼
             ┌──────────────┐      ┌──────────────┐      ┌──────────────┐
             │  火种节点 A  │      │  火种节点 B  │      │  火种节点 N  │
             │  RTX 3060    │      │  RTX 4090    │      │  A100 40GB   │
             │  firefly-cli │      │  firefly-cli │      │  firefly-cli │
             └──────┬───────┘      └──────┬───────┘      └──────┬───────┘
                    │  下载训练数据 & LoRA 权重上报             │
                    ▼                                             ▼
         ┌──────────────────────────────────────────────────────────────────┐
         │                         MinIO (S3 兼容)                            │
         │  ┌─────────────────┐    ┌──────────────────┐   ┌────────────────┐ │
         │  │ firefly-models │    │ firefly-weights │   │ firefly-data   │ │
         │  │  (基础模型)     │    │  (LoRA权重)      │   │  (训练数据)    │ │
         │  └─────────────────┘    └──────────────────┘   └────────────────┘ │
         └──────────────────────────────────────────────────────────────────┘
```

## 核心数据流

### 1. 节点注册
```
火种节点 → POST /nodes/register → 调度中心 → PostgreSQL
                                         ↓
                              返回 node_id + node_key（凭证）
```

### 2. 任务领取（竞争模式）
```
多个火种节点 → POST /tasks/claim → 调度中心
                               ↓
             调度中心按 priority + 时间排序，选择最早等待的任务
                               ↓
             返回 TaskPackage(base_model, config, data_s3_prefix)
             调度中心记录 TaskSubmission（assigned_count++）
```

### 3. 训练执行（节点本地）
```
TaskPackage → 下载训练数据(MinIO) → QLoRA训练(base_model + LoRA)
                                  ↓
                              上传 LoRA 权重 → MinIO
```

### 4. 结果上报
```
POST /submissions/report
  → 计算贡献分（GPU系数 × steps × batch_size × epochs）
  → 更新 Node.total_compute_score
  → 更新 Task.completed_count
  → 节点状态恢复 ONLINE
```

### 5. 权重聚合（后台任务）
```
触发条件: 某任务 Task.completed_count ≥ AGGREGATION_MIN_TASKS
执行器: scheduler/worker/aggregator.py
聚合策略: Simple Average → DARE → MoE（渐进）
输出: 合并后权重 → firefly-weights/{task_id}/merged/
```

## 仓库职责

| 仓库 | 职责 |
|------|------|
| `scheduler` | API 服务、数据库、心跳管理、任务分发 |
| `firefly-client` | 节点端：硬件检测、训练执行、结果上报 |
| `infra` | GitHub Actions、Helm/K8s、Docker Compose |
| `docs` | 技术文档、路线图、贡献指南 |

## 技术栈

| 层级 | 选型 | 原因 |
|------|------|------|
| API 框架 | FastAPI | 异步、高性能、自动文档 |
| 数据库 | PostgreSQL | 强一致、JSON 支持、成熟 |
| 缓存/队列 | Redis | 心跳存储、任务队列 |
| 对象存储 | MinIO | S3 兼容、本地开发友好 |
| 量化训练 | QLoRA (bitsandbytes) | 4-bit 量化，6GB 显存可运行 |
| LoRA | peft (HuggingFace) | 主流、轻量 |
| 容器编排 | Kubernetes | 生产级，可扩展 |
