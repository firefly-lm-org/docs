# v0.2 真实 QLoRA 训练里程碑

**日期**: 2026-07-26
**状态**: ✅ 训练成功（60步 / 51秒 / loss 0.8754）

## 成果

### 训练结果
- **Loss 曲线**: 1.17 → 1.00 → 0.90 → 0.78 → 0.71 → 0.68（逐步收敛）
- **最终 train_loss**: 0.8754
- **训练耗时**: 51.4 秒（60步，RTX 4090）
- **GPU 显存峰值**: 1.07 GB（4-bit NF4 量化，LoRA r=8）
- **可训练参数**: 1,089,536 / 1,544,803,840 (0.0705%)

### 产物
| 文件 | 大小 | 说明 |
|------|------|------|
| adapter_model.safetensors | 4.2 MB | LoRA 权重（Qwen2.5-1.5B-Instruct） |
| firefly_trainer_meta.json | 278 B | 训练元数据 |
| adapter_config.json | 689 B | LoRA 配置（r=8, alpha=16） |
| trainer_state.json | - | SFTTrainer 完整状态 |

GitHub: `firefly-lm-org/firefly-client/benchmarks/rtx-4090-qwen2.5-1.5b/`

## 环境

| 包 | 版本 |
|----|------|
| torch | 2.4.0+cu121 |
| transformers | 4.44.0 |
| bitsandbytes | 0.50.0 |
| accelerate | 1.14.0 |
| peft | 0.12.0 |
| trl | 0.8.0 |

模型：Qwen2.5-1.5B-Instruct（本地 ModelScope 缓存）
路径：`/root/models/models/Qwen--Qwen2.5-1.5B-Instruct/snapshots/master`

## 踩坑全记录

### Bug 1: dispatch_model 与 bitsandbytes 4-bit 冲突
**症状**: `ValueError: .to is not supported for 4-bit or 8-bit bitsandbytes models`
**根因**: `transformers 4.44.0` 的 `from_pretrained()` 在任何 `device_map` 下都会调用 `dispatch_model()`，`dispatch_model()` 调用 `model.to(device)` 与已量化模型冲突
**解法**: Patch `accelerate/big_modeling.py`：
```python
# 在 dispatch_model 函数的 "model.to(device)" 前加：
if getattr(model, "is_quantized", False):
    return model
```
已固化到 `scripts/accelerate_patch.py`

### Bug 2: SFTTrainer packing ValueError
**症状**: `ValueError: too many dimensions 'str'`
**根因**: trl 0.8.0 的 `SFTTrainer` 当 `packing=False` 时需指定 `dataset_text_field` 或 `formatting_func`
**解法**: 加 `formatting_func=lambda x: x["text"]`，去掉显式 `DataCollatorForLanguageModeling`

### Bug 3: final_train_loss 读取错误
**症状**: 读 `log_hist[-1].get("loss")` 返回 -1.0
**根因**: `log_history` 最后一条是 run summary（含 `train_loss` 字段），不是 step log
**解法**: 读 `trainer.state.train_loss`，或遍历找最后一个有 `step` 字段的 entry

### Bug 4: training_args.bin 误入 adapter 目录
**症状**: `trainer.save_model()` 保存了 `TrainingArguments`
**解法**: 训练完成后手动 `os.remove(adapter_dir + "/training_args.bin")`

## 下一步

1. **FedAvg 聚合验证**：第二台机器跑训练 → 产 2 份 adapter → 聚合 → 推理测试
2. **阿里云 OSS 集成**：替换 MinIO mock，真实上传 adapter
3. **v0.2 里程碑文档**：补全 milestone_v0.2.md
