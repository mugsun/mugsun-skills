# mugsun-skills

中文 | [English](#english)

mugsun 平台的官方 AI 技能包，采用 Agent Skills 格式（每个技能一个目录，内含 `SKILL.md`）。技能内容以 [mugsun-boot](https://github.com/mugsun/mugsun-boot)、[mugsun-core](https://github.com/mugsun/mugsun-core)、[mugsun-pc](https://github.com/mugsun/mugsun-pc) 等仓库的真实约定为准；各仓根目录的 `AGENTS.md` 是更细的权威说明，二者冲突时以仓内 `AGENTS.md` 为准。

技能正文仅有中文。

## 技能列表

| 技能 | 用途 |
| --- | --- |
| [mugsun-design](mugsun-design/SKILL.md) | 设计新业务模块：分层、实体、租户、数据权限、Flyway 迁移、前端页面 |
| [mugsun-doc](mugsun-doc/SKILL.md) | 同步更新文档站 docs.mugsun.com，中英文同步更新 |
| [mugsun-audit](mugsun-audit/SKILL.md) | 代码审查：阻塞 / 建议 / 可选三级，带文件:行号和改法 |
| [mugsun-commit](mugsun-commit/SKILL.md) | 提交与推送：Conventional Commits、CHANGELOG、迁移与分支规则 |

## 安装

把需要的技能目录复制到你的 AI 工具读取技能的位置，例如项目内的 `.claude/skills/` 或 `.cursor/skills/`：

```bash
git clone https://github.com/mugsun/mugsun-skills.git
cp -r mugsun-skills/mugsun-audit your-project/.claude/skills/
```

## 许可证

[MIT](LICENSE)

---

<a id="english"></a>

## English

Official AI skills for the mugsun platform, in the Agent Skills format (one directory per skill, each with a `SKILL.md`). The skills follow the actual conventions of repositories such as [mugsun-boot](https://github.com/mugsun/mugsun-boot), [mugsun-core](https://github.com/mugsun/mugsun-core) and [mugsun-pc](https://github.com/mugsun/mugsun-pc). Each repository's root `AGENTS.md` is the more detailed source of truth; if they disagree, the repository's `AGENTS.md` wins.

The skill bodies are in Chinese only.

### Skills

| Skill | Purpose |
| --- | --- |
| [mugsun-design](mugsun-design/SKILL.md) | Design a new business module: layers, entities, tenancy, data scope, Flyway migrations, front-end pages |
| [mugsun-doc](mugsun-doc/SKILL.md) | Update docs.mugsun.com, keeping Chinese and English in sync |
| [mugsun-audit](mugsun-audit/SKILL.md) | Code review graded blocker / suggestion / optional, each with file:line and a fix |
| [mugsun-commit](mugsun-commit/SKILL.md) | Commit and push: Conventional Commits, CHANGELOG, migration and branch rules |

### Install

Copy the skill directories you need into the folder your AI tool reads skills from, such as `.claude/skills/` or `.cursor/skills/` in your project:

```bash
git clone https://github.com/mugsun/mugsun-skills.git
cp -r mugsun-skills/mugsun-audit your-project/.claude/skills/
```

### License

[MIT](LICENSE)
