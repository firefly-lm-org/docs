# Firefly LLM — Roadmap

**萤火虫分布式训练平台路线图**

> 最后更新：2026-07-25
> 定位：分布式志愿算力驱动的开源持续微调与对齐平台

---

## 整体架构

```
贡献者节点 ──(HTTPS)──> 调度中心 (firefly-lm-org/scheduler)
                              │
                         MinIO 存储
                         (任务包/权重)
                              │
                    ┌─────────┴──────────┐
                    │   权重聚合 Worker   │
                    │   (FedAvg, ≥3份)   │
                    └─────────┬──────────┘
                              │
                         聚合模型权重
                              │
                         HuggingFace / ModelScope
```

---

## 阶段 0：项目初始化 ✅

- [x] GitHub org `firefly-lm-org` 创建
- [x] 4 仓库就位（scheduler / firefly-client / docs / infra）
- [x] 定位文案确定
- [x] PRIVACY.md / CLA.md / LICENSE
- [ ] 域名注册（firefly-lm.com / .org）
- [ ] TRADEMARK 查询
- [ ] 官网搭建（Cloudflare Pages）

---

## v0.1 微光版 ✅（调度闭环）

**目标：** 验证分布式调度全链路可行性

### 核心功能
- [x] FastAPI 调度中心（PostgreSQL + Redis + MinIO Mock）
- [x] 用户注册/登录（JWT）/ 节点注册
- [x] 任务生命周期（5态：pending→running→submitting→completed/failed）
- [x] 分布式锁防抢（Redis SETNX）
- [x] 贡献值结算（不可篡改日志）
- [x] 后台协程（超时回收/离线检测/信誉恢复）
- [x] 权重聚合 Worker（FedAvg，≥3份触发）
- [x] firefly-client CLI（7命令 + 42/42测试全绿）

### 退出标准
- [x] 本地 E2E 3节点全链路跑通
- [x] 聚合 E2E 验证通过
- [x] 隐私政策 / CLA / LICENSE
- [ ] 官网单页
- [ ] 5人种子测试群

---

## v0.5 训练版（真实 QLoRA，1.5B）

**目标：** 在真实训练任务中验证平台

### 技术目标
- [ ] 底座锁定：**Qwen3-1.5B 4bit**
- [ ] Unsloth QLoRA rank=32 集成到客户端
- [ ] 任务包标准化：`config.yaml`（epoch/lr/rank） + `data.jsonl`
- [ ] 客户端真实训练（替换模拟 sleep+随机权重）
- [ ] 分域标签（general / law / code / medical / safety）
- [ ] L2/L3 校验实装

### 运营目标
- [ ] 节点规模：300~1000 节点
- [ ] V2EX / HN / Reddit / 知乎首发
- [ ] 云厂商资源赞助洽谈

### 合规前置
- [ ] MODEL_LICENSE.md（底座许可）
- [ ] GOVERNANCE.md
- [ ] 7条 ADR 架构决策记录

---

## v1.0 成炬版（7B 可用）

**目标：** 产出可投入使用的 7B 领域模型

- [ ] 底座升级 Qwen3-7B 4bit
- [ ] 分域聚类 + Critic 校验
- [ ] DoRA / rsLoRA
- [ ] 动态 LoRA 路由推理
- [ ] 周期融合 + HF / ModelScope 发布
- [ ] scheduler 水平扩展（Redis Cluster / Kafka）
- [ ] 客户端代码签名
- [ ] 管理面板前端
- [ ] 商业主体注册 / 基金会挂靠
- [ ] 节点 3000+
- [ ] 收入覆盖 10-30 万/年运维成本

---

## v2.0 遍野版（MoE 探索）

- [ ] MoE 底座接入（Llama4-MoE / Qwen-MoE）
- [ ] Expert 路由聚合策略
- [ ] 跨域知识融合
- [ ] 10000+ 节点规模支持

---

## v3.0 长明版

- [ ] 全模态支持（Vision / Audio / Text）
- [ ] 去中心化治理（DAO）
- [ ] 商业化收入（企业 API / 定制微调）

---

## 技术决策（ADR）

| # | 决策 | 状态 |
|---|------|------|
| ADR-001 | 放弃从零训练7B，基于开源底座微调 | ✅ |
| ADR-002 | 使用 LoRA/QLoRA 而非全参数微调 | ✅ |
| ADR-003 | MinIO 本地 Mock 方案（无 Docker 场景） | ✅ |
| ADR-004 | 权重聚合使用 FedAvg | ✅ |
| ADR-005 | 自我造血：赞助/捐赠，不碰红十字 | ✅ |
| ADR-006 | MoE 推迟到 v3.0 | ✅ |
| ADR-007 | 收入预期：v1.0 覆盖 10-30 万/年 | ✅ |
