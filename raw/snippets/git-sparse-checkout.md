---
title: Git 稀疏检出最佳实践
type: snippet
created: 2026-04-23
tags: [git, devops, best-practices]
use_case: 只检出仓库中的部分目录，节省空间和时间
---

# Git 稀疏检出最佳实践

## 场景
- 只需要仓库中的文档目录
- 大仓库，只想检出特定子项目
- 节省磁盘空间和克隆时间

## 完整流程

```bash
# 1. 克隆时不检出，不下载内容，只取最新提交
git clone --no-checkout --filter=blob:none --depth 1 --sparse <repo-url>

# 2. 进入仓库
cd <repo-name>

# 3. 初始化稀疏检出（cone 模式性能最优）
git sparse-checkout init --cone

# 4. 设置要检出的目录
git sparse-checkout set docs/ md/

# 5. 查看配置
git sparse-checkout list

# 6. 执行检出
git checkout
```

## 参数说明

| 参数 | 作用 |
|------|------|
| `--no-checkout` | 克隆后不立即检出，等 sparse 配置好再检出 |
| `--filter=blob:none` | 不下载文件内容，检出时按需拉取 |
| `--depth 1` | 浅克隆，只取最新提交 |
| `--sparse` | 启用稀疏检出能力 |
| `--cone` | 使用 cone 模式（Git 2.37+，性能最优） |

## 常用命令

```bash
# 添加更多目录
git sparse-checkout add tests/

# 查看当前配置
git sparse-checkout list

# 禁用稀疏检出（恢复完整仓库）
git sparse-checkout disable

# 重新启用
git sparse-checkout init --cone
git sparse-checkout set <dirs>
```

## 实际案例

检出 docker 仓库的 md/ 目录：

```bash
git clone --no-checkout --filter=blob:none --depth 1 --sparse github.com_cucker_docker
cd github.com_cucker_docker
git sparse-checkout init --cone
git sparse-checkout set md/
git sparse-checkout list  # 确认配置
git checkout
```

## 注意事项

- `--cone` 模式只支持目录级别的选择，不支持文件级别的 glob
- 如需文件级别的精细控制，去掉 `--cone`，手动编辑 `.git/info/sparse-checkout`
- `--filter=blob:none` 需要服务器支持（GitHub、GitLab 都支持）