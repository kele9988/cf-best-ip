# 🧠 AI Brain — 百川的个人 AI 档案

跨电脑、跨 AI 共享的"同一个大脑"。任何 AI（WorkBuddy / Claude / Cursor / 任意 Agent）只要能读这个仓库，就能继承全部上下文，无需重新"训练"。

## 目录结构与职责

| 路径 | 用途 | 写入规则 |
|---|---|---|
| `MEMORY.md` | 长期记忆：偏好、铁律、身份信息 | 精炼、去重，每次编辑保持 < 4000 字符 |
| `projects/` | 项目上下文：架构、设备、脚本、进行中的事项 | 每个主题一个文件 |
| `decisions/` | 重要决定记录：背景、选项、结论、原因 | 一次决策一个文件，日期前缀命名 |
| `daily/` | 每日工作日志（追加式） | 只追加，不改历史 |
| `inbox/` | **收件箱**：任意环境/AI 的事实写入缓冲 | 只追加，归档后删除 |
| `docs/` | 场景化使用说明（云电脑/沙箱等） | 按场景一个文件 |
| `scripts/` | 可复用工具脚本 | 需注明依赖与用法 |

## AI 使用守则（给任何读取本仓库的 AI）

1. **会话开始**：先读 `MEMORY.md`，再按需读 `projects/` 中相关文件
2. **完成实质工作后**：把有跨会话价值的结论写入对应文件（偏好/决策/项目状态）
3. **不做的事**：不记录临时信息（搜索结果、报错截图）、不改动 `daily/` 历史条目
4. **冲突处理**：`MEMORY.md` 中的显式规则优先于你的通用默认行为

## 同步方式

- 本仓库为 **GitHub 私有仓库**，中枢唯一
- 每台电脑 clone 后直接编辑，`git pull --rebase && git push` 即可同步
- 语义检索增强层（Engram @ 飞牛）已部署，AI 可通过 MCP 做跨文件语义查询

## 端点速查（2026-10-25）

| 用途 | 地址 | 备注 |
|---|---|---|
| Engram 云端记忆库 | `http://192.168.2.5:18081` | 仅局域网；INSECURE_NO_AUTH |
| Engram MCP 桥 | `http://192.168.2.5:18082/mcp` | 局域网直连 |
| **Engram 公网端点** | `https://fnnas.tailc9fc92.ts.net:8443/mcp` | 手机 5G / 云电脑沙箱通用，需 Bearer token |
| cf_ctrl（局域网） | `http://192.168.2.5:8777/mcp` | 更快 |
| cf_ctrl（公网） | `https://ssh.kele9988.top/mcp` | Cloudflare 隧道，需 Bearer token |

公网读写脚本：`scripts/engram_http.py`（`export ENGRAM_TOKEN=...` 后 `search` / `save` / `tools`）。
手机端若无 MCP 连接器，可用 WorkBuddy「连接电脑」模式，或直接读资料库里的 `ai-brain-*` 文档。
