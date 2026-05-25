# imotion-flow 项目规则文档

## Git 架构说明

本项目采用"追踪上游开源项目 + 维护自有定制"的工作流。

| Remote | URL | 用途 |
|--------|-----|------|
| `origin` | `https://github.com/artisian1028/imotion-flow.git` | 自有项目仓库，日常 push/pull |
| `upstream` | `https://github.com/freemocap/freemocap.git` | 上游开源项目，跟踪最新源码 |

## 分支策略

| 分支 | 用途 | 规则 |
|------|------|------|
| `main` | 跟踪上游开源项目 | **禁止直接修改**。只从 upstream/main 同步 |
| `custom` | 自有项目的定制开发 | 基于 main 分支，包含所有自定义修改 |

## AI 助手操作规范

### 日常开发

1. 所有代码修改都在 `custom` 分支上进行。操作前先确认当前分支：
   ```
   git branch --show-current
   ```
2. 如果当前在 `main` 分支，必须先切换到 `custom` 分支再修改代码：
   ```
   git checkout custom
   ```
3. `main` 分支仅用于跟踪上游，**禁止**直接在 main 上 commit。

### 同步上游更新

当需要拉取上游开源项目的最新代码时，按以下步骤操作：

```
# Step 1: 暂存当前工作区（如有未提交的修改）
git stash

# Step 2: 切换到 main 分支
git checkout main

# Step 3: 从 upstream 拉取最新代码
git fetch upstream
git merge upstream/main

# Step 4: 切回 custom 分支
git checkout custom

# Step 5: 合并 main 的更新到 custom
git merge main

# Step 6: 如有冲突，手动解决后 commit
# Step 7: 恢复之前暂存的修改（如有）
git stash pop
```

### 冲突处理

1. 合并时如遇到冲突，优先保留 **上游代码的逻辑结构**，在此基础上叠加 custom 的修改。
2. 解决完冲突后，运行项目测试确保功能正常。
3. 如果冲突涉及项目核心文件，向用户报告冲突详情，由用户决定取舍。

### 提交规范

1. `custom` 分支上的 commit 消息使用中文描述，清晰说明修改内容。
2. 不要在 commit 中包含敏感信息（密钥、密码等）。
3. 提交前检查不要包含自动生成的临时文件。

### 禁止操作

- **禁止** force push 到任何分支
- **禁止** 直接在 main 分支 commit
- **禁止** 修改 git config 的全局设置
- **禁止** 使用 `git push --force` 到 main/master
- **禁止** skip hooks（`--no-verify`, `--no-gpg-sign` 等）

## 项目结构

```
freemocap/          # 主 Python 包
freemocap-ui/       # 前端 UI
experimental/       # 实验性功能
bin/                # 脚本工具
```

## 常用命令速查

```bash
# 查看当前分支
git branch --show-current

# 查看所有 remote
git remote -v

# 从 upstream 拉取最新（仅 fetch，不合并）
git fetch upstream

# 查看 upstream 有哪些新提交
git log main..upstream/main --oneline

# 查看 custom 比 main 多了哪些提交
git log main..custom --oneline
```
