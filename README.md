# Firefly LM · 文档中心

> 萤火虫大模型 · 完整文档索引

## 📁 文档结构

```
docs/
├── README.md                  ← 本文件
├── ROADMAP.md                ← 修正版技术路线图
├── ARCHITECTURE.md           ← 系统架构说明
├── CONTRIBUTION.md           ← 贡献者指南
├── TASK_SPEC.md              ← v0.1 任务拆解
└── COMPLIANCE.md             ← 合规路线图
```

## 🌟 项目定位

**萤火虫大模型（Firefly LM）** 是一个基于全球分布式志愿算力的开源中文持续微调平台：

- 不从零训练模型，基于 Qwen3 / Llama3.1 等开源强底座
- 用 LoRA / QLoRA 做轻量级持续微调（4-bit 量化）
- 全球志愿者贡献闲置 GPU 算力
- 开源权重、社区共有

## 🔗 核心链接

| 仓库 | 说明 |
|------|------|
| [XuZheFireFlylm/scheduler](https://github.com/XuZheFireFlylm/scheduler) | 调度中心 API |
| [XuZheFireFlylm/firefly-client](https://github.com/XuZheFireFlylm/firefly-client) | 火种客户端 |
| [XuZheFireFlylm/infra](https://github.com/XuZheFireFlylm/infra) | CI/CD 与部署 |
