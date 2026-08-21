# Skills Manifest

`skills-manifest` 是 Active AI Lab 技能工作区的 Repo manifest，使用一个 `default.xml` 集中声明技能及相关工具仓库，让开发者可以用统一命令初始化并同步整套工作区。

## 功能概览

- 统一管理 29 个项目，包括技能仓库、安装工具和 manifest 自身。
- 默认从 `ssh://git@github.com/active-ailab/` 拉取仓库。
- 默认使用 `main` 分支、4 路并发同步和深度为 1 的浅克隆。
- 可为单个项目覆盖默认分支；当前 `wsl-serial-monitor` 使用 `master`。
- 各项目同步到与仓库职责对应的独立目录，便于统一更新和本地使用。

## 前置条件

- 已安装 `repo` 命令。
- 已配置可访问 `github.com/active-ailab` 的 SSH 凭据；manifest 的远端地址使用 SSH。
- 本地 Git 环境可正常工作。

## 初始化与同步

在准备作为技能工作区的目录中执行：

```bash
repo init -u https://github.com/active-ailab/skills-manifest.git -b main
repo sync
```

`repo init` 会从本仓库读取 manifest，`repo sync` 会按照 `default.xml` 中的项目、路径和分支配置拉取全部仓库。

## 清单结构

`default.xml` 的核心配置如下：

- `remote`：名为 `origin`，指向 Active AI Lab GitHub 组织的 SSH 地址。
- `default`：默认远端为 `origin`，默认分支为 `main`，同步并发数为 4，克隆深度为 1。
- `project`：为每个仓库声明 `name` 和本地 `path`；如有需要可单独声明 `revision`。

当前清单覆盖研发规范、知识库、Jira/Gerrit 工作流、飞书 MCP 授权、构建与发布、设备操作、运动数据处理等技能和工具仓库。

## 更新清单

新增或调整受管仓库时，请修改 `default.xml` 中对应的 `<project>` 条目，并确认仓库名称、本地路径和分支与实际仓库一致。未显式设置 `revision` 的项目会继承默认的 `main` 分支。

## 注意事项

- 默认使用浅克隆；需要完整历史的场景应结合 Repo/Git 工作流另行处理。
- 除 `wsl-serial-monitor` 外，当前项目均继承默认的 `main` 分支。
- 本仓库只维护同步清单，不负责把同步后的 Skill 安装到 Agent 或 Codex 的用户目录；安装职责由清单中的 `active_skills_install` 仓库承担。
