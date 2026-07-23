# 贡献指南

> 萤火虫大模型是社区共建的开源项目，欢迎所有形式的贡献！

## 🤝 贡献方式

### 1. 成为火种节点（最直接）
- 有一台带 NVIDIA GPU 的电脑/服务器
- 运行 `firefly-client`，贡献闲置算力
- 获得贡献分，影响最终模型权重分配

### 2. 代码贡献
```bash
# Fork 并克隆仓库
git clone https://github.com/XuZheFireFlylm/scheduler.git
# 创建特性分支
git checkout -b feat/your-feature
# 提交 PR
```

### 3. 文档贡献
直接在 GitHub 上提交 PR 修改 `docs/` 目录下的任何文档。

### 4. 训练数据贡献
> 格式：JSONL，每行一个 JSON 对象，包含 `text` 字段
```json
{"text": "用户: 你好\n助手: 你好！有什么我可以帮你的吗？"}
```
上传到 `docs/DATA_FORMAT.md` 中指定的 MinIO 路径。

---

## 📋 开发环境设置

```bash
# 调度中心
cd scheduler
cp .env.example .env  # 填写配置
docker compose -f docker/docker-compose.yml up -d
pip install -r requirements.txt
uvicorn app.main:app --reload

# 火种客户端
cd firefly-client
pip install -r requirements.txt
python -m firefly.cli setup
python -m firefly.cli run
```

## 🧪 测试

```bash
# 调度中心
pytest tests/ -v

# 火种客户端
pytest firefly/tests/ -v
```

## 🏷️ Issue 规范

提交 Issue 请包含：
- **环境信息**：GPU型号、操作系统
- **复现步骤**：完整的错误日志
- **期望行为** vs **实际行为**

## 📜 行为准则

- 尊重所有贡献者
- 专注于技术本身
- 不讨论政治敏感话题
- 违规内容将被立即删除
