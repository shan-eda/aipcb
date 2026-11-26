# INSTALL_FOR_AI — AI Agent 自动配置执行手册

本文件专门面向 **Codex / Claude Code / Antigravity / Cursor** 等 AI Agent。当用户说“去 GitHub 找 `shan-eda/aipcb` 帮我配好环境/skill”时，请严格按以下确定性步骤执行，力求**一次跑通、零卡死、结果清晰可验证**。

---

## Step 0：识别目标范围与当前运行环境

1. **判断当前 AI 客户端的 Skill 落盘根目录**（优先复用已有目录，勿破坏现有配置）：
   - Codex：通常为 `~/.codex/skills/` 或 `~/.agents/skills/`
   - Claude Code：通常为 `~/.claude/skills/`
   - Antigravity / Gemini：通常为 `~/.gemini/config/skills/`
   - 如同时存在多个且不确定，安装到当前正在读取的那个 Skill 根目录，并在总结里写明绝对路径。
2. **判断用户想装哪几个组件**：
   - 用户说“全配 / 配齐 3 个 / 帮我配一下” → 目标为 `[1] easyeda-agent` + `[2] Konnect-EDA` + `[3] pcb-prompt-helper`。
   - 用户只提了立创 EDA → 目标为 `[1] easyeda-agent` + `[3] pcb-prompt-helper`。
   - 用户只提了 KiCad → 目标为 `[2] Konnect-EDA` + `[3] pcb-prompt-helper`。
3. **快速检测本机桌面 EDA 软件**（非阻塞，不要因为没装软件就报错中止）：
   - 检测是否存在嘉立创 EDA 专业版（Windows 常见于 `EasyEDA Pro` / `lceda-pro.exe`）。
   - 检测是否存在 KiCad（Windows 常见于 `C:\Program Files\KiCad\*\bin\kicad-cli.exe` 或 `D:\KiCad\*\bin\kicad-cli.exe`；macOS/Linux 检查 `kicad-cli`）。

---

## Step 1：安装内置 Skill #3 — `pcb-prompt-helper`（必装）

1. 读取本仓库文件：`skills/pcb-prompt-helper/SKILL.md`。
2. 在本机 Skill 根目录下创建目录 `pcb-prompt-helper/`，并将 `SKILL.md` 完整写入为：
   `<SKILLS_ROOT>/pcb-prompt-helper/SKILL.md`
3. 验证文件非空且包含 YAML Frontmatter（`name: pcb-prompt-helper`）。

---

## Step 2：安装 Skill #1 — 嘉立创 EDA 专业版（`zhoushoujianwork/easyeda-agent`）

> 若用户明确只装 KiCad，跳过本步。

1. 拉取或查看上游仓库 `https://github.com/zhoushoujianwork/easyeda-agent` 的最新 `README` 与安装方式。
2. 按上游最新指引完成：
   - 本地 Skill 安装与依赖检查；
   - 定位或构建出本机 `easyeda-agent-connector.eext` 插件文件，记录其**完整绝对路径**。
3. **不要假装已经帮用户在 GUI 里点了导入**：在最终汇报中必须把 `easyeda-agent-connector.eext` 的绝对路径单独放在代码块里，并附上 4 步极简导入指引：
   1. 打开 **嘉立创 EDA 专业版**（v3.2+ / v4.1+，注意不是标准版）；
   2. 顶部菜单 **`高级` → `扩展管理器`** → 左下角蓝色 **`导入`**；
   3. 粘贴路径并选中 **`easyeda-agent-connector.eext`**；
   4. 切到 **`已安装`** → **`EDA Agent Connector`** → **`配置`** → 勾选 **`允许外部交互`**，然后回到对话发送 **`尝试进行连接`**。
4. 如果用户本机当前已运行嘉立创 EDA 且已导入插件，直接执行健康检查验证连通。

---

## Step 3：安装 Skill #2 — KiCad（`Orchis-ai-LLC/Konnect-EDA`）

> 若用户明确只装嘉立创 EDA，跳过本步。

1. 拉取或查看上游仓库 `https://github.com/Orchis-ai-LLC/Konnect-EDA` 的最新 `README` 与安装方式。
2. 按上游最新指引完成：
   - 安装 `Konnect-EDA` 对应的本地 KiCad Skills（原理图、PCB、器件库、审查、制造等）；
   - 配置 `konnect` MCP 服务及本机 KiCad 路径（如有 `kicad-cli`，一并写入/验证）。
3. **连通测试规则**：
   - 若本机已安装 KiCad → 执行一次连通测试（验证 MCP / KiCad 环境就绪）；
   - 若本机尚未安装 KiCad → 保留已安装好的 Skill 与配置，状态标记为「Skill 已安装，待安装 KiCad 后连接」，**不要反复重试导致失败退出**。

---

## Step 4：输出统一验收报告（必须清晰给到用户）

全部步骤完成后，用以下简洁结构向用户汇报：

1. **三件套安装状态总表**：
   - `pcb-prompt-helper`（四要素提示词 Skill）：状态 + 本机安装路径
   - `easyeda-agent`（嘉立创 EDA Skill）：状态 + `.eext` 绝对路径（如适用）
   - `Konnect-EDA`（KiCad Skill & MCP）：状态 + KiCad 连通情况（如适用）
2. **用户唯一需要手动做的步骤**（若有）：例如立创 `.eext` 导入步骤，或提示重启客户端以刷新 MCP。
3. **立刻可用的第一步示范**：
   > 现在你可以直接对我说：  
   > `用 pcb-prompt-helper 帮我生成四要素画板提示词：我想做一块【写下你想做的板子】`
