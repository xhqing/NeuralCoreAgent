# CHANGELOG

### 变更（本项目与子项目的 `AGENTS.md`、`.pi/` 改为完全独立，2026-11-08）

- **为什么改**：用户 2026-11-08 决定「Agent 项目与其子项目二者的 AGENTS.md（项目规则）和 .pi 目录之间应该完全无关」——各自只放本项目真实需要的规则与工具，这样才能同时满足两边的特异性需求。原做法（子项目挂 `AGENTS.md` 软链接 + `.pi/settings.json` 引用本项目四个资源目录）的毛病：跨仓库路径引用是隐式依赖，本项目改名 / 子项目单独 clone / 换机器都会静默失效（pi 不报错，只是少加载东西）；而「只允许追加、不许重述」的纪律本身就说明两层绑在一起有事。
- **改了什么**：本项目 `AGENTS.md` 的子项目节重写为「与子项目完全独立」——删掉「身份层软链接 + 能力层路径引用 + 只允许追加」三条，改为「各自只放本项目真实需要的内容」的口径（子项目怎么做 / 本项目怎么做 / 为什么 / 安全网不受影响）；子项目清单里的陈旧描述同步改正。
- **配套**：子项目仓库侧另行改造（实体 `AGENTS.md` + 删 `.pi/settings.json` 交叉引用），在各自的 PR 里完成。
- **影响**：安全网一件不少——全局 `~/.pi/agent/`（团队规则、`add` / `commit` / `bump` / `release` 技能、各类守卫扩展）在任何目录都会加载。

### 新增（CI 门禁：validate 检查 + main 分支 strict 保护）

- **为什么改**：用户 2026-11-03 立规「新建仓库一律必须 strict 模式的 PR CI」，并要求 28 个不合规的存量仓库统一执行最新标准。本项目此前在 PR 上没有门禁——改动合进 main 全靠人工把关，PR 可以拿旧 main 上通过的检查直接合并，回归防护存在缺口。本次补齐团队要求的「三件套」：PR 上真实跑的 CI 检查 + 挂成 required status check + `required_status_checks.strict = true`。
- **改了什么**（2026-11-03）：新增 `.github/workflows/validate.yml`（PR 与 main push 触发，检查名 `validate`）与 `.github/scripts/validate.py`（仓库卫生检查：必备文件存在、敏感文件未被跟踪、被跟踪文件不超过 10 MB、README 相对链接有效、Python / Shell 语法检查）；main 分支保护补挂 required check `validate` 并设 `strict=true`。
- **边界**：检查只做确定性、低误报的项目（required check 一旦误报就会卡住正常 PR，所以宁可少查）；不改动仓库任何原有内容。

## [未发布]

### 变更（项目规则文件本体统一改为 `AGENTS.md`，`CLAUDE.md` 弃用，2026-11-05）

- **为什么改**：用户 2026-11-05 立规「所有注册表项目与全局的规则文件本体统一用 `AGENTS.md`，`CLAUDE.md` 弃用」。pi 的上下文文件候选顺序是 `AGENTS.override.md` > `AGENTS.md` > `CLAUDE.md`（同一目录只加载第一个存在的），本体放在 `AGENTS.md` 后 `CLAUDE.md` 根本不参与加载——两份并存只会形成双份内容、随时漂移。
- **改了什么**：项目规则本体从 `.claude/CLAUDE.md` 迁到项目根 `AGENTS.md`（原 `AGENTS.md → .claude/CLAUDE.md` 软链接已删除，`.claude/` 下其它文件不动）；项目内引用「项目 `CLAUDE.md` / 全局 `CLAUDE.md`」的条文、skill、hook、脚本与 README 同步改为 `AGENTS.md`。
- **回归检查**：加载顺序不受影响（`AGENTS.md` 本就排在 `CLAUDE.md` 之前）；子项目根目录的 `AGENTS.md` 软链接仍指向本项目 `AGENTS.md`，目标不变、照常解析。

### 变更（子项目 `.pi` 改为真实目录 + 资源引用 + 子项目清单节改写，2026-10-30 用户修订）

- **为什么改**：用户 2026-10-30 修订「智能体专属文件」归属规则（取代 2026-10-17 的「`.pi` 也挂软链接」）——旧版把「共享」与「独占」压在同一个软链接上：子项目往 `.pi` 写任何东西都等于直接写进 Agent 项目，会污染该 Agent 名下全部兄弟子项目、并进 Agent 仓库的 git 历史。新版分层：身份层（`AGENTS.md`）继续软链接以保持一致（pi 的上下文文件没有 include 机制，超集只能靠复制、复制必然漂移）；能力层（`.pi/`）在子项目里建真实目录，`settings.json` 用路径引用 Agent 项目的资源目录，本地专属内容放自己的目录。
- **改了什么**：①`.claude/CLAUDE.md`「子项目」节改写为 2026-10-30 版——标题更新为「2026-10-30 用户修订：身份层软链接 + 能力层目录引用」；正文拆成「实体只在本项目」+「AgentCortex 按两层处理（`AGENTS.md` 软链接 / 真实 `.pi/` 目录 + `settings.json` 引用）」+「子项目 `.pi` 只允许追加、不得重述 / 覆盖基座规则」（子项目专属规则用 `.pi/APPEND_SYSTEM.md` 追加，不用 `.pi/SYSTEM.md`）；规则出处路径由旧的 `~/.claude/docs/agents-registry.md` 改为权威源 `~/.pi/agent/docs/agents-registry.md`。②子项目 AgentCortex 按新规把 `.pi` 软链接换成真实目录 + `.pi/settings.json`（详情记该仓 CHANGELOG）。
- **说明**：本项目 `.pi` 目前只有 `.gitkeep`、尚无 `extensions` / `skills` / `prompts` / `themes` 目录，因此子项目 `settings.json` 里的这四条引用当下为空转；以后本项目 `.pi` 新增技能 / 扩展时会被子项目自动带过去，无需再同步。

## [0.1.2] - 2026-10-17

### 变更（Agent 进驻子项目：AGENTS.md / .pi 软链接网络，2026-10-17 用户修订）

- **为什么改**：用户 2026-10-17 修订「智能体专属文件」归属规则（取代 2026-10-12「子项目一律不得存在」）——智能体专属文件的实体只在 Agent 项目，子项目根目录挂两条相对路径软链接指回负责 Agent（`AGENTS.md → ../<Agent项目>/AGENTS.md`、`.pi → ../<Agent项目>/.pi`），在子项目目录开会话即继承负责 Agent 的身份 / 规则 / 技能；链接随仓库提交；后台会话仍只以 Agent 项目根为工作目录。
- **改了什么**：①新建项目根软链接 `AGENTS.md` → `.claude/CLAUDE.md`（作为 pi 侧会话入口与子项目链接的目标）；②新建项目级 `.pi/`（含 `.gitkeep`，作为子项目 `.pi` 链接的落点）；③`.claude/CLAUDE.md`「子项目」节措辞更新为 2026-10-17 版；④子项目 AgentCortex 按新规新增指回本项目的 `AGENTS.md` / `.pi` 两条软链接（详情记该仓 CHANGELOG）。

## [0.1.1] - 2026-10-12

### 变更（子项目新规改写 + AgentCortex 智能体专属文件清理，2026-10-12 用户修订）

- **为什么改**：用户 2026-10-12 修订全局规则，取代原「子项目 `.claude/` 超集」规则——`CLAUDE.md` / `AGENTS.md` / `.claude/` / 项目级 `.pi/` 等「会话启动时按工作目录加载、表征智能体身份」的智能体专属文件只有 Agent 项目允许拥有，非 Agent 项目（含子项目）一律不得存在；后台开启 pi 会话（intercom / `agent_call` / `agent_wake`）只能以 Agent 项目根目录为工作目录。
- **改了什么**：①`.claude/CLAUDE.md`「子项目 `.claude/` 自动同步（2026-08-10 立）」节整节改写为「子项目（2026-10-12 用户修订）」——列明子项目清单（AgentCortex）、子项目不携带智能体专属文件、跨目录按路径操作、后台会话工作目录限制、映射维护要求；②子项目 AgentCortex 的存量智能体专属文件按新规清理删除（`CLAUDE.md` + `AGENTS.md` 软链；其指南中与 README 重复的部分不再保留，独有内容「评测文档入口」迁入该仓 README 的 Project Files 表，随附的本项目规则全文属本项目、副本删除）——AgentCortex 侧详情记该仓 CHANGELOG。

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
