# 萤火虫大模型 — 贡献指南

感谢你愿意参与 Firefly LM 的建设！本项目采用开源社区协作模式，所有贡献者需遵守以下规范。

---

## 贡献方式

### 🐛 报告问题
- 前往 [GitHub Issues](https://github.com/firefly-lm-org/firefly-scheduler/issues) 报告 Bug
- 安全漏洞请发邮件至 **admin@firefly-lm.com**，勿在公开渠道披露

### 💡 提出功能建议
- 前往 [GitHub Discussions](https://github.com/firefly-lm-org/scheduler/discussions) 发起讨论

### 🔧 提交代码（Pull Request）

#### 第一步：签署 DCO

所有提交到本组织的代码必须附有 **Signed-off-by** 声明，表示你确认代码由你原创或你有权提交。

在你的 commit message 末尾添加：

```
Signed-off-by: 你的名字 <your@email.com>
```

可以用 `git commit -s` 自动附加。

#### 第二步：提交 PR

1. Fork 对应仓库
2. 创建特性分支 `git checkout -b feat/your-feature`
3. 完成开发，确保测试通过
4. 提交时使用 `git commit -s -m "feat: description"`
5. Push 并提交 Pull Request

#### PR 审查标准

- ✅ 代码风格与项目一致
- ✅ 有新增测试或更新了现有测试
- ✅ 不引入新的 lint 错误
- ✅ 提交信息符合 [Conventional Commits](https://www.conventionalcommits.org/)

---

## 开发环境

```bash
# 克隆仓库
git clone https://github.com/firefly-lm-org/scheduler.git
cd scheduler

# 安装依赖
pip install -r requirements.txt

# 复制环境变量
cp .env.example .env  # 编辑填入实际值

# 启动服务
python -m app.main
```

```bash
# 克隆客户端仓库
git clone https://github.com/firefly-lm-org/firefly-client.git
cd firefly-client
pip install -r requirements.txt
firefly-cli --help
```

---

## 行为准则

本项目遵守 [Contributor Covenant](https://www.contributor-covenant.org/) 行为准则。

---

## 贡献者名录

见 [docs/CONTRIBUTORS.md](./CONTRIBUTORS.md)（启动中）

---

*萤火虫大模型 — 聚微光，筑大同*
