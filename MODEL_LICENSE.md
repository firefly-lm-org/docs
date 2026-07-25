# 萤火虫大模型 — 模型许可证

**版本**: v0.1  
**生效日期**: 2026-07-24  
**许可证类型**: [OpenRAIL-M v1](https://www.licenses.ai/blog/2023/6/25/open-rail-a-new-license-for-responsible-ai鸳鸯) + Apache 2.0 双轨

---

## 概述

Firefly LM 项目采用双许可证架构：

| 组件 | 许可证 | 说明 |
|------|--------|------|
| **调度中心代码** (scheduler) | Apache 2.0 | 可商用、可修改 |
| **客户端代码** (firefly-client) | Apache 2.0 | 可商用、可修改 |
| **聚合 LoRA 权重** | OpenRAIL-M | 详见限制用途 |
| **底座模型权重** | 各自底座许可证 | Qwen3 / Llama3 等各自的许可证 |

---

## OpenRAIL-M 限制用途（适用于 Firefly 聚合 LoRA 权重）

以下用途**明确禁止**：

- ❌ **军事或战争用途**：任何用于武器或战争相关的应用
- ❌ **非法活动**：用于促进犯罪或违法的任何用途
- ❌ **未经同意的 surveillance**：未经本人同意的监控或追踪个人
- ❌ **自动决策歧视**：在法律或金融等高风险场景中用于自动化决策（无人类监督）
- ❌ **生成伤害性内容**：用于制造虚假信息、仇恨言论、骚扰内容
- ❌ **商业禁止行为**：在违反适用法律或法规的背景下使用

### 允许的用途

- ✅ 研究和学术用途
- ✅ 商业产品和服务的开发（需满足上述限制）
- ✅ 安全对齐研究
- ✅ 贡献到开源社区

---

## 底座模型各自许可证

Firefly LM 的聚合基于以下开源底座，各底座模型持有其各自的权利：

- **Qwen3 系列**: [Alibaba License](https://github.com/QwenLM/Qwen3/blob/main/LICENSE)  
- **Llama 3.x 系列**: [Llama 3.1 Community License Agreement](https://github.com/meta-llama/llama3/blob/main/LICENSE)

使用 Firefly 聚合 LoRA 权重时，请同时遵守对应底座模型许可证。

---

## 免责声明

Firefly LM 聚合 LoRA 权重由社区志愿者贡献，模型可能存在偏差、错误或不适内容。使用者需自行承担风险。

---

## 贡献者承诺

所有向 Firefly LM 提交训练数据或模型权重的贡献者，需确认：

1. 贡献内容不侵犯第三方知识产权
2. 贡献内容不包含个人隐私数据（未经本人同意）
3. 贡献内容不违反OpenRAIL-M 禁止用途条款

---

*萤火虫大模型 — 聚微光，筑大同*
