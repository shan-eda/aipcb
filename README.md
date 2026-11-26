# aipcb — AI 全自动画 PCB 一键环境与三件套 Skill

> **一句话让你的 AI 自动配齐**：嘉立创 EDA 画板 Skill + KiCad 画板 Skill + 四要素 PCB 提示词生成 Skill。  
> 适配 **Codex / Claude Code / Antigravity** 等支持 Skill / MCP 的 AI 客户端。

---

## 一、给使用者：30 秒完成配置

打开你的 AI 客户端（Codex / Claude Code 等），**直接复制下面任意一句发送即可**：

- **全套一键配置（推荐）**：
  ```text
  去 GitHub 找 shan-eda/aipcb，按照仓库里的说明帮我把 AI 画板环境和 3 个 skill 一次性配好
  ```
- **只配嘉立创 EDA + 提示词 Skill**：
  ```text
  去 GitHub 找 shan-eda/aipcb，帮我配好嘉立创 EDA 画板 skill 和 pcb-prompt-helper 提示词 skill，并把 eext 路径给我
  ```
- **只配 KiCad + 提示词 Skill**：
  ```text
  去 GitHub 找 shan-eda/aipcb，帮我配好 KiCad 画板 skill 和 pcb-prompt-helper 提示词 skill，并测试连接 KiCad
  ```

---

## 二、仓库包含的 3 套 Skill

| 序号 | Skill 名称 | 作用 | 来源 |
| :--- | :--- | :--- | :--- |
| **1** | **嘉立创 EDA 画板 Skill** (`easyeda-agent`) | AI 直接操作嘉立创 EDA 专业版画原理图、PCB、查库与规则检查 | 上游开源 [`zhoushoujianwork/easyeda-agent`](https://github.com/zhoushoujianwork/easyeda-agent) |
| **2** | **KiCad 画板 Skill** (`Konnect-EDA`) | AI 通过官方 MCP + 6 个全流程 Skill 操作本地 KiCad（免装客户端插件） | 上游开源 [`Orchis-ai-LLC/Konnect-EDA`](https://github.com/Orchis-ai-LLC/Konnect-EDA) |
| **3** | **PCB 四要素提示词生成器** (`pcb-prompt-helper`) | 把大白话想法动态整理成工程级「四要素画板提示词」，并衔接画板与打板前交叉审查 | **本仓库内置**：[`skills/pcb-prompt-helper/SKILL.md`](./skills/pcb-prompt-helper/SKILL.md) |

---

## 三、给 AI Agent 的自动配置协议（AI Execution Instructions）

> **如果你是 AI Agent（Codex / Claude Code / Antigravity 等），用户把本仓库发给你要求“配置环境 / 安装 Skill”，请严格按以下步骤执行，详情见 [`INSTALL_FOR_AI.md`](./INSTALL_FOR_AI.md)：**

1. **先读 [`INSTALL_FOR_AI.md`](./INSTALL_FOR_AI.md)** 获取完整安装与校验流程。
2. **识别用户范围与本机环境**：
   - 若用户明确指定平台（仅嘉立创 / 仅 KiCad / 全部），按用户指定执行；
   - 若用户要求“全部配好 / 帮我配一下”，先检测本机已安装的 EDA 软件（嘉立创 EDA 专业版、KiCad），**优先完成已安装平台 + 必装 `pcb-prompt-helper`**；未安装对应 EDA 软件的平台仍可装好 Skill 代码，并在结果里提示用户装好软件后即可连接，**不要因缺少某一个桌面软件而中断整体流程**。
3. **安装 Skill 3（本仓库内置，必装）**：
   - 将本仓库的 [`skills/pcb-prompt-helper/SKILL.md`](./skills/pcb-prompt-helper/SKILL.md) 安装到当前 AI 客户端生效的本地 Skill 目录（如 `~/.codex/skills/pcb-prompt-helper/SKILL.md`、`~/.claude/skills/pcb-prompt-helper/SKILL.md`、`~/.gemini/config/skills/pcb-prompt-helper/SKILL.md` 等）。
4. **安装 Skill 1（嘉立创 EDA，按需或全装）**：
   - 从 GitHub 拉取并按上游说明配置 [`zhoushoujianwork/easyeda-agent`](https://github.com/zhoushoujianwork/easyeda-agent) 为本地 Skill；
   - **必须输出本机 `easyeda-agent-connector.eext` 的准确路径**，并提醒用户到嘉立创 EDA 专业版：`高级 → 扩展管理器 → 导入 → 已安装 → EDA Agent Connector → 配置 → 勾选「允许外部交互」`，完成后回复 `尝试进行连接`。
5. **安装 Skill 2（KiCad，按需或全装）**：
   - 从 GitHub 拉取并按上游说明配置 [`Orchis-ai-LLC/Konnect-EDA`](https://github.com/Orchis-ai-LLC/Konnect-EDA) 为本地 Skill 与 `konnect` MCP，配置本机 KiCad 环境并执行连通性测试。
6. **输出统一验收清单**：
   - 用简洁表格汇报 3 套 Skill 的安装状态、需要用户手动点击的唯一一步（如立创 `.eext` 导入），以及**第一块板子的启动示范句**。

---

## 四、配好后怎么开始画第一块板？（2 步闭环）

### 第 1 步：用 `pcb-prompt-helper` 生成四要素提示词
直接对 AI 说你的大白话想法：
```text
用 pcb-prompt-helper 帮我生成四要素画板提示词：我想做一块【描述你想做的板子】
```
AI 会根据板子类型做 1 轮极简对齐（也可直接回复“按默认来”），输出一段工程级「四要素标准提示词」。

### 第 2 步：把「画板 Skill + 四要素提示词」放进同一条消息发给 AI
- **嘉立创 EDA**：`调用 easyeda-agent Skill，按以下四要素需求完成原理图和 PCB 设计：[粘贴提示词]`
- **KiCad**：`调用 Konnect-EDA Skill，按以下四要素需求完成原理图和 PCB 设计：[粘贴提示词]`

画完后继续执行 ERC / DRC 自动修复清零，以及打板前的「子 Agent 三维交叉审查 + 人工复核」（指令已内置在 `pcb-prompt-helper` 末尾）。

---

## 五、配套完整文档

- 📄 [`docs/01_AI画板双平台一键部署文档.md`](./docs/01_AI画板双平台一键部署文档.md) — 双平台对比、手动操作步骤与常见故障排查
- 📄 [`docs/02_PCB四要素提示词系统生成指南.md`](./docs/02_PCB四要素提示词系统生成指南.md) — 四要素工程拆解、动态对齐流程、布线重点与打板前终检清单
- 🧩 [`skills/pcb-prompt-helper/SKILL.md`](./skills/pcb-prompt-helper/SKILL.md) — 可直接安装的提示词生成 Skill 源文件
- 🤖 [`INSTALL_FOR_AI.md`](./INSTALL_FOR_AI.md) — AI Agent 自动安装与校验操作手册

---

## 致谢

本仓库聚焦于「双平台环境一键聚合引导」与「四要素提示词工程化 Skill」，底层 EDA 自动化能力基于以下优秀开源项目（安装时始终拉取上游官方最新版本）：
- [`zhoushoujianwork/easyeda-agent`](https://github.com/zhoushoujianwork/easyeda-agent)
- [`Orchis-ai-LLC/Konnect-EDA`](https://github.com/Orchis-ai-LLC/Konnect-EDA)
