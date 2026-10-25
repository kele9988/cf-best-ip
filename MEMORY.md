# MEMORY.md — 百川的长期记忆

> 任何 AI 读取本文件后，即视为已知信息，无需再次询问。

## 身份与偏好

- 称呼固定为「百川」
- 语言：简体中文
- 沟通风格：直接、结构化、**结论先行**；善用表格与清晰行动步骤；不接受含糊敷衍
- 涉及 UI 操作时习惯直接上传截图而非文字描述
- 要求跨会话持久记忆，能按关键词自动调取历史上下文
- 手机 2026-10-25 换为**华为 Mate 90（纯血鸿蒙 HarmonyOS NEXT，不兼容安卓 APK）**；原小米已弃用
- 运维类问题期望「先给结论，再给可照做的步骤」
- 文档类交付倾向中文输出、结构化、可复制命令

## 家庭网络（核心长期主题，详见 projects/家庭网络运维.md）

- 主力设备：飞牛 fnNAS（fnnas，硬件 oesplus / fnOS / aarch64），IP `192.168.2.5` / `192.168.2.253`
- 原 N1 盒子（192.168.2.22）已于 2026-10 完全弃用，全部迁移至飞牛
- 旁路由 `192.168.2.2` = 飞牛 Docker 内 OpenWrt 24.10.8 容器（OpenClash + AdGuard Home）
- 域名：`kele9988.top` / `kele9988.dpdns.org`（Cloudflare 管理）
- Tailscale tailnet：`kele9988@`
- 远程控制台 cf_ctrl 部署于飞牛 `:8777`（MCP 端点 `/mcp`，Bearer token 在飞牛 `/root/cf_ctrl/token.txt`）
- Tailscale tailnet `kele9988@`：飞牛 fnnas = `100.81.83.32`（常驻在线）
- 公网隧道 `ssh.kele9988.top`（Cloudflare，健康）→ 内网 cf_ctrl；外网也可直连 `https://ssh.kele9988.top/mcp`

## AI 个人大脑（三层，2026-10-25 建成）

| 层 | 载体 | 职责 |
|---|---|---|
| A | 本仓库 `kele9988/ai-brain` | 权威档案、决策、规范；**异地冷备份** |
| B | Engram Cloud @ 飞牛 `192.168.2.5:18081` | 语义记忆，autosync 30s 双向同步 |
| C | WorkBuddy 资料库（8 份 `ai-brain-*` 文档） | 手机端无连接器也能读的镜像 |

- **Engram 公网端点**（手机 5G / 云电脑沙箱通用）：`https://fnnas.tailc9fc92.ts.net:8443/mcp`（Tailscale Funnel → Bearer 代理 `:18083` → MCP 桥 `:18082`）
- 调用脚本 `scripts/engram_http.py`（只需 python3 + 公网 + `ENGRAM_TOKEN`，无需 CLI/局域网）
- ⚠️ **WorkBuddy 手机 App 无自定义 MCP 连接器**（仅桌面端有）→ 手机上要么「连接电脑」模式，要么用上面的脚本/HTTP
- 🔑 `engram search` 默认 AND 匹配，整句自然语言查空 → 用**短关键词**或 `match=any`

## 影视磁力助手 movieget（详见 projects/影视磁力助手movieget.md）

- 飞牛 fnOS 上的影视磁力搜索/推送 Web 应用，`:8779`，纯 Python3 标准库，源码在 `/vol1/@appcenter/movieget/server/`
- 数据源 btbtla + SeedHub（后者需 FlareSolverr）；推送引擎 qBittorrent（dlcenter 实例）或迅雷远程下载
- 铁律：改前端免重启刷新即生效、改 app.py 需 `systemctl restart movieget`；改代码必须浏览器自验证后再部署

## 飞牛同步面板 fnsync（详见 projects/飞牛同步面板fnsync.md）

- 飞牛 fnOS 上的 rclone 可视化同步面板，`:8778`（X-Token 鉴权），把本地目录 copy/move 到移动云盘（AList WebDAV）
- 特色：直播模式（rc bwlimit 实时降速）；纯标准库实现
- 铁律：限速绝不 kill rclone（无断点续传）；重启面板前先 /api/job/stop（main stop 不杀 rclone 子进程）；AList 大文件必须 `--timeout 30m`

## 铁律

1. 个人目录（Desktop/Downloads/Documents 等）的删除类操作必须先扫描报告、经确认后分批执行
2. 自建 MCP 的鉴权 token 只能写字面量进 headers（WorkBuddy 不展开自建 server 的 `${VAR}`）
3. cf_ctrl `/api/exec` 的 cmd 有约 57KB 长度上限，大文件用分块 base64 追加（8000 字符/块）
4. 判断「某设备/服务装没装」前必须交叉验证：PATH 二进制 / systemd+docker / `ip addr` 接口 / `/vol1/@appcenter` 目录——单点证据（尤其 fnOS 应用不在 PATH）不可采信
5. 本机多网卡：有线 `192.168.2.88`（网关主路由 2.1，有效出口）；无线 `192.168.2.66` 长期未连 WiFi，**禁止用于网络诊断**
6. 涉及飞牛的「有没有装什么 / 状态如何」，先读资料库《飞牛家庭网络运维手册》再实测；两者冲突以实测为准并回写手册
7. Engram 记忆写入后需确认 autosync 进程在跑（PC 端靠 WorkBuddy 的 engram MCP 连接器，重启后才生效），否则记忆只留在本机
