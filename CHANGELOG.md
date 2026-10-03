# CHANGELOG

## [Unreleased]

### 变更（CLAUDE.md 删去「由 Claude Code 自动加载」说明句）

- **为什么改**：用户 2026-09-12 要求 CLAUDE.md 不再强调本文由 Claude Code 加载，团队全部项目的 CLAUDE.md 统一清理此类语句。
- **改了什么**（2026-09-12）：`.claude/CLAUDE.md` 开头角色定位行删去句尾「本文件由 Claude Code 在每次会话开头自动加载。」，角色描述本身保留。

### 变更（全局规则路径与通用能力句式更新：rules 废弃 + find-skill 删除联动）

- **为什么改**：①全局通用规则已全部迁入 `~/.claude/CLAUDE.md`、`~/.claude/rules/` 目录废弃，本项目两处指向旧目录的引用失效；②全局 find-skill skill 已删（实际使用中从未用到），通用能力句式不再提及。均系 2026-09-12 用户指出后的联动清理。
- **改了什么**：`.claude/CLAUDE.md`：①「遵守通用工作规则（见全局 `~/.claude/rules/`）」与「通用工作纪律（三个规则文件名）见全局 `~/.claude/rules/`」两处改指 `~/.claude/CLAUDE.md`；②「（anysearch 实时搜索、find-skill 找 skill 等）」→「（anysearch 实时搜索等）」。子项目 AgentCortex 的根 CLAUDE.md 同款句式已按超集规则同步（不另记其 CHANGELOG）。

### 变更（项目迁移收尾：CLAUDE.md 子项目清单路径更新）

- **为什么改**：项目现址在 `~/Developer/`（`~/Documents/Projects/` 旧址已弃用，2026-09-08 迁移收尾时发现子项目清单仍指旧路径），避免后续会话被引导到不存在的位置。
- **改了什么**：`.claude/CLAUDE.md` 子项目清单中 AgentCortex 路径更新为 `~/Developer/AgentCortex`；子项目 AgentCortex 仓库内 `.claude/CLAUDE.md` 的同款行已同步更新（超集关系保持一致）。

### 变更（措辞统一 fleet → team / 舰队 → 团队：README 中英双语跟随全局统一）

- **为什么改**：用户 2026-08-16 已把 xhqing 主页 README 的自称从「舰队 / fleet」改为「团队 / team」，但本仓 README 中英两版仍是 fleet 旧措辞——外部读者沿「主页 → 各 agent 仓库」浏览会看到两种自称并存；2026-08-21 用户裁定全量存量一次清零、统一为团队 / team。
- **改了什么**：`README.md` 6 处 fleet → team（Ada 引言、owns 句、Position in the fleet 节标题及表内 2 处、末段）；`README_cn.md` 对应处舰队 / fleet → 团队（「在舰队中的位置」→「在团队中的位置」等）。仅改措辞，职责、结构、徽章均不变。

## [0.1.0] - 2026-08-11

### 新增

- 项目初始化：按 fleet 脚手架约定（2026-07-13 立）新建 AI 算法工程师 Agent——目录名 `NeuralCoreAgent`、拟人名 **Ada**（致敬 Ada Lovelace，历史上第一位程序员、算法思想先驱，与「AI 算法工程师」角色契合；查注册表无重名）。
- 角色定位：负责所有 AI 算法与推理机制的设计、开发与评测；目前唯一在手项目为 **AgentCortex**（深度推理引擎规则集——Infinite-Reasoning / Rapid-Reasoning / Incisive-Reasoning 三套引擎，分别面向 DeepSeek V4 Pro / Flash、GLM 5.1），由其设计、迭代与评测。
- 项目标配文件齐全：角色化 `.claude/CLAUDE.md`、中英双语 `README.md` / `README_cn.md`（含 logo、License / Last Commit / Type 三枚徽章，不含 Stars 数量徽章）、`assets/logo.svg`（Ada 神经核心主题，紫罗兰渐变，与 Anvil 的蓝青、Prometheus 的橙红区分）、`LICENSE.md`（MIT，版权人归一为 All Contributors）、`.gitignore`、`VERSION`（0.1.0）、`CHANGELOG.md`、`.claude/settings.json` 与 `.claude/settings.local.example.json`（本机配置模板）、`.claude/settings.local.json`（本机，已 gitignore）。
- 不复制通用能力：`.claude/` 不放置 anysearch / find-skill 等通用副本（「通用能力开源单一出口」规则，2026-08-09 立），通用能力一律从全局 `~/.claude/` 或 CapabilityManagerAgent 的 `claude/` 开源镜像获取。
- 新增「子项目 `.claude/` 自动同步」规则（`.claude/CLAUDE.md` 新一节）：本项目 `.claude/` 为权威源，各子项目 `.claude/` 为其超集——本项目 `.claude/` 下除 `CLAUDE.md` 外的内容变更（新增 / 修改 / 删除）后自动同步到所有子项目，子项目独有内容保留不动，同步后 diff 核对；`CLAUDE.md` 内容同样超集（实现方式不限、效果等价即可）。原因：确保用户只操作子项目时，其 `.claude/` 也包含本项目的完整内容，体现项目归 Ada 负责。当前子项目清单：**AgentCortex**。
- 建立超集关系：已把本项目 `.claude/` 全部文件同步至 AgentCortex 的 `.claude/`（`settings.json` / `settings.local.example.json` / `settings.local.json` 逐字节一致；`CLAUDE.md` 全文并入 AgentCortex 项目指南、标题降级为 `###` 并带指代说明「本项目指 NeuralCoreAgent」）；同步时按规则给 AgentCortex 的 `.gitignore` 补上 `settings.local.json` 等忽略规则。
- 全局注册表更新：`~/.claude/CLAUDE.md`「智能体命名注册表」新增 Ada 行、「Agent 项目与子项目的 `.claude/` 超集关系」映射表新增 NeuralCoreAgent → AgentCortex 行、「销售流水线顺序」段补 Ada 独立于流水线。
