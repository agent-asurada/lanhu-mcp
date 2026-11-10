<div align="center">

# 🎨 Lanhu MCP Server | 蓝湖MCP服务器

**面向 Design-to-Code：读取蓝湖设计稿、标注、样式与切图，辅助前端还原 UI**

**lanhumcp | 蓝湖mcp | lanhu-mcp | 蓝湖AI助手 | 蓝湖skills | Lanhu AI Integration**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io/)
[![FastMCP](https://img.shields.io/badge/FastMCP-Powered-orange.svg)](https://github.com/jlowin/fastmcp)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![GitHub Stars](https://img.shields.io/github/stars/dsphper/lanhu-mcp?style=social)](https://github.com/dsphper/lanhu-mcp/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/dsphper/lanhu-mcp)](https://github.com/dsphper/lanhu-mcp/issues)
[![GitHub Release](https://img.shields.io/github/v/release/dsphper/lanhu-mcp)](https://github.com/dsphper/lanhu-mcp/releases)
[![Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-2.0-4baaaa.svg)](CODE_OF_CONDUCT.md)

[English](README_EN.md) | 简体中文

[快速开始](#-快速开始) • [功能特性](#-核心特性) • [使用文档](#-使用指南) • [贡献指南](CONTRIBUTING.md)


</div>

---

## 🌟 项目亮点

这是 [dsphper/lanhu-mcp](https://github.com/dsphper/lanhu-mcp) 的精简 fork，只保留 **Design-to-Code** 场景需要的 MCP 工具：列出设计稿、读取图层/样式/标注、获取预览图，以及下载切图与素材。

已移除需求分析（Axure/PRD）、团队留言板、飞书通知和协作者追踪等与设计转代码无关的能力。

一个面向蓝湖设计交付的 [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) 服务器。由 MCP 提供来源数据和资源，大模型结合画面理解并适配目标工程。

设计工作流详见 [DESIGN_CONTEXT.md](DESIGN_CONTEXT.md)。本地 stdio 启动见 [LOCAL-STDIO.md](LOCAL-STDIO.md)。

🔥 **核心能力**：
- 🎨 **设计稿列表与预览**：获取项目内 UI 设计图，返回总览、局部裁图和节点索引
- 📐 **标注与样式**：按图层 ID 读取原始样式、祖先节点和关联素材 ID
- ✂️ **切图与素材**：获取切图下载信息，或导出校验后的原始资源包
- 🧩 **HTML/CSS 参考**：可生成派生布局代码，需结合原始标注和实际渲染核验

🎯 **适用场景**：在 Cursor、Claude Code、Windsurf 等支持 MCP 的 AI 开发工具中，把蓝湖设计稿还原为前端 UI 代码。

---
## 📑 目录

- [核心特性](#-核心特性)
- [快速开始](#-快速开始)
- [使用指南](#-使用指南)
- [可用工具列表](#-可用工具列表)
- [系统架构](#-系统架构)
- [项目结构](#-项目结构)
- [高级配置](#-高级配置)
- [性能指标](#-性能指标)
- [常见问题](#-常见问题)
- [安全说明](#-安全说明)
- [贡献指南](#-贡献指南)
- [许可证](#-许可证)
- [致谢](#-致谢)
- [联系方式](#-联系方式)
- [路线图](#-路线图)

---

## ✨ 核心特性

### 🎨 UI 设计支持
- **按画面和图层 ID 读取**：固定设计版本，返回设计总览、带编号的局部裁图、原始样式与素材 ID；不依赖统一的图层命名。
- **原始切图自动交付**：实际下载并校验图片尺寸/哈希，提供可通过 MCP 传输的资源包和客户端安装器。详见 [设计上下文与资源交付](DESIGN_CONTEXT.md)。
- **设计稿查看**：列出并展示项目中的 UI 设计图。
- **设计图分析**：返回设计图预览、源参数以及 Schema 派生的 **HTML+CSS 参考**；生成代码仍需结合原始标注和实际渲染核验。
- **切图提取**：识别和导出设计切图、图标资源。
- **邀请链接解析**：把蓝湖分享/邀请链接解析为带 pid 的项目 URL，便于后续读取设计稿。

---
## 🚀 快速开始

> ⚠️ **重要提示：必须使用支持视觉功能的AI模型！**
>
> 本项目需要AI模型具备**图像识别和分析能力**，推荐使用以下2026年主流视觉模型：
> - 🤖 **Claude** (Anthropic)
> - 🌟 **GPT** (OpenAI)
> - 💎 **Gemini** (Google)
> - 🚀 **Kimi** (月之暗面)
> - 🎯 **Qwen** (阿里巴巴)
> - 🧠 **DeepSeek** (深度求索)
>
> 不支持纯文本模型（如 GPT-3.5、Claude Instant 等）

---

> 💡 **小白用户？** 直接对 AI 说 "帮我克隆并安装 https://github.com/dsphper/lanhu-mcp 项目"，AI 会引导你完成所有步骤！

### 方式一：让 AI 帮你安装（推荐！！！）

直接在对 AI 说：
```
"帮我克隆并安装 https://github.com/dsphper/lanhu-mcp 项目"
```

AI 会自动完成：克隆项目 → 安装依赖 → 引导获取 Cookie → 配置并启动服务

📖 参考文档：[AI 安装指南](ai-install-guide.md) • [Cookie 获取教程](GET-COOKIE-TUTORIAL.md)

---

### 方式二：手动安装

**2.1 Docker 部署（推荐）**

优点：环境隔离、一键部署、易于管理

```bash
# 1. 克隆项目
git clone https://github.com/dsphper/lanhu-mcp.git
cd lanhu-mcp

# 2. 创建配置并填写 Cookie
cp .env.example .env
# 编辑 .env，将 LANHU_COOKIE 改为你自己的 Cookie

# 3. 构建并启动服务
docker compose up -d --build
```

Windows 用户可用 `copy .env.example .env` 创建配置文件。旧版 Docker Compose 可将 `docker compose` 替换为 `docker-compose`。

**已有单文件部署升级：** `lanhu_mcp_server.py` 已内嵌项目自带的 `lanhu_design` 实现。在依赖已安装的环境中，只替换这一个文件即可获得完整设计稿工具；不再需要额外复制 `lanhu_design/` 目录。正式安装仍推荐使用完整仓库、wheel 或 Docker，以便同步依赖。

📖 详细文档：[Docker 部署指南](DEPLOY.md)

**2.2 源码运行**

前置要求：Python 3.10+。macOS 自带的 Python 3.9 不满足要求，请先运行 `python3 --version` 确认版本。

```bash
# 1. 克隆项目
git clone https://github.com/dsphper/lanhu-mcp.git
cd lanhu-mcp

# 2. 一键安装（推荐，会引导你配置 Cookie）
bash easy-install.sh        # Linux/Mac
# 或
easy-install.bat           # Windows
```

> 💡 `easy-install.sh` 会自动安装依赖、引导获取 Cookie 并配置环境。国内用户默认优先使用阿里云 PyPI，并自动回退到清华和官方 PyPI；Chromium 使用 npmmirror 的 Chrome for Testing 专用镜像，兼容旧版路径并以 Playwright 官方 CDN 兜底；可通过 `PIP_INDEX_URL`、`PLAYWRIGHT_DOWNLOAD_HOST`、`PLAYWRIGHT_CHROMIUM_DOWNLOAD_HOST` 覆盖。

<details>
<summary>或者手动安装（不推荐）</summary>

Linux / macOS：

```bash
python3 -m venv venv
./venv/bin/python -m pip install -e .
./venv/bin/python -m playwright install chromium
cp .env.example .env
# 编辑 .env，将 LANHU_COOKIE 改为你自己的 Cookie
```

Windows：

```bat
python -m venv venv
venv\Scripts\python.exe -m pip install -e .
venv\Scripts\python.exe -m playwright install chromium
copy .env.example .env
```
</details>

### 配置（源码运行需要）

1. **设置蓝湖 Cookie**（必需）

```bash
export LANHU_COOKIE="your_lanhu_cookie_here"
```

> 💡 获取 Cookie：登录蓝湖网页版，打开浏览器开发者工具，从请求头中复制 Cookie

2. **其他环境变量**（可选）

```bash
# 服务器配置
export SERVER_HOST="0.0.0.0"       # 服务器监听地址
export SERVER_PORT=8000            # 服务器端口

# 数据存储
export DATA_DIR="./data"           # 数据存储目录

# 性能调优
export HTTP_TIMEOUT=30             # HTTP请求超时时间（秒）
```

> 📝 完整环境变量说明请参考 `config.example.env` 文件

### 运行服务

**源码运行：**
```bash
./venv/bin/lanhu-mcp --transport http       # Linux/macOS
venv\Scripts\lanhu-mcp.exe --transport http  # Windows
```

**按需启动（stdio，本地 MCP 客户端推荐）：**
```bash
./run-stdio.sh # Linux/Mac
./run-stdio.bat # Windows
```

`run-stdio.sh` 会自动进入项目目录、读取 `.env`，并以 stdio 方式启动 MCP 服务。适合 Cursor、Claude Code 等支持 `command` / `args` 配置的客户端按需拉起服务，无需手动常驻启动 HTTP 服务。

📖 不依赖 Docker 的完整安装与注册步骤：[纯本地按需调用指南](LOCAL-STDIO.md)

**Docker 运行：**
```bash
docker-compose up -d              # 启动
docker-compose logs -f            # 查看日志
docker-compose down              # 停止
```

服务器将在 `http://localhost:8000/mcp` 启动

### 连接到 AI 客户端

在支持 MCP 的 AI 客户端（如 Claude Code、Cursor、Windsurf）中配置：

**Claude Code 配置示例：**
```json
{
  "mcpServers": {
    "lanhu": {
      "type": "http",
      "url": "http://localhost:8000/mcp"
    }
  }
}
```

**Cursor / Windsurf 等其他客户端配置示例：**
```json
{
  "mcpServers": {
    "lanhu": {
      "url": "http://localhost:8000/mcp"
    }
  }
}
```

**按需启动配置示例（无需提前启动服务）：**

Linux/Mac
```json
{
  "mcpServers": {
    "lanhu": {
      "command": "/bin/bash",
      "args": [
        "<ABSOLUTE_PATH_TO_LANHU_MCP>/run-stdio.sh"
      ]
    }
  }
}
```

Windows
```json
{
  "mcpServers": {
    "lanhu": {
      "command": "<ABSOLUTE_PATH_TO_LANHU_MCP>/run-stdio.bat"
    }
  }
}
```

请将 `<ABSOLUTE_PATH_TO_LANHU_MCP>` 替换为本机 `lanhu-mcp` 项目的绝对路径；macOS/Linux 下可在项目目录执行 `pwd` 获取。

> 📌 stdio 环境变量说明：
> - `LANHU_DISABLE_SYSTEM_PROXY`: 设为 `1` 时，`run-stdio.sh` / `run-stdio.bat` 清除继承的代理并设置 `NO_PROXY=*`，避免 httpx 回落系统代理（适用于 IDE 注入 SOCKS 导致失败等场景）；详见 [LOCAL-STDIO.md](LOCAL-STDIO.md)

## 🎯 提升 UI 还原度

开启蓝湖的**设计稿转代码**功能可以显著提升 UI 还原度。如果遇到提示无法转换的问题，需要让 UI 设计师升级蓝湖插件版本后重新上传设计稿。

---

## ✨保持关注

给我们点个 Star，你将能第一时间从 GitHub 收到所有新版本的发布通知！
<img width="900" alt="Screenshot 2025-06-02 at 3 03 49 PM" src="https://github.com/user-attachments/assets/1c9a3661-80a4-4fba-a30f-f469898b0aec" />
## 📖 使用指南

### UI 设计稿查看

**推荐工作流（版本化、按 ID 读取）：**
```
1. lanhu_get_designs              # 获取设计图列表和 design_id
2. lanhu_get_design_overview      # 固定版本，返回总览图和节点索引
3. lanhu_inspect_design_region    # 按区域或 node_ids 查看样式与素材 ID
4. lanhu_export_design_assets     # 下载并校验原始素材
```

也可用 `lanhu_get_ai_analyze_design_result` 获取整图预览和派生的 HTML/CSS 参考（旧接口，建议优先使用上面的版本化工具）。

### 切图下载

```
1. lanhu_get_designs          # 获取设计图列表
2. lanhu_get_design_slices    # 获取指定设计图的切图信息和下载地址
```

若用户给的是邀请/分享链接，先调用 `lanhu_resolve_invite_link` 解析出带 pid 的项目 URL。

## 🛠️ 可用工具列表

| 工具名称 | 功能描述 | 使用场景 |
|---------|---------|---------|
| `lanhu_resolve_invite_link` | 解析邀请/分享链接为项目 URL | 用户提供分享链接时 |
| `lanhu_get_designs` | 获取 UI 设计图列表 | 查看设计稿前必调用 |
| `lanhu_get_design_overview` | 固定版本，返回设计图与节点索引 | 按稳定 ID 读取设计 |
| `lanhu_inspect_design_region` | 局部裁图、编号、样式与资源 ID | 结合视觉定位元素并读取样式 |
| `lanhu_export_design_assets` | 下载校验原始素材并提供资源包 | 安装到客户端工程 |
| `lanhu_get_ai_analyze_design_result` | 整图预览和 HTML/CSS 参考 | 快速查看设计并得到布局参考 |
| `lanhu_get_design_slices` | 获取切图信息和下载地址 | 下载图标、素材 |

另提供 MCP 资源：`lanhu://design/{snapshot_id}/preview/{preview_id}`（预览裁图）和 `lanhu://design/{snapshot_id}/bundle/{bundle_id}`（素材包）。

---
## 🏗️ 系统架构

```
AI 客户端 (Cursor / Claude Code / Windsurf / ...)
        │  MCP (HTTP 或 stdio)
        ▼
Lanhu MCP Server (FastMCP)
  - lanhu_get_designs / lanhu_get_design_overview / lanhu_inspect_design_region
  - lanhu_export_design_assets / lanhu_get_design_slices
  - lanhu_get_ai_analyze_design_result / lanhu_resolve_invite_link
  - lanhu_design 快照、预览图与素材包
        │  HTTP/JSON
        ▼
蓝湖平台 API（项目设计图、DDS schema、切图与素材）
```

工作流：解析 URL → 列出设计稿 → 固定版本快照 → 按区域/节点查看样式 → 导出并校验素材。

## 📁 项目结构

```
lanhu-mcp-server/
├── lanhu_mcp_server.py          # 主服务器文件
├── lanhu_design/                # 快照、区域、文本、素材变体与安装器
├── DESIGN_CONTEXT.md            # 设计上下文与资源交付说明
├── requirements.txt              # Python 依赖
├── Dockerfile                    # Docker 镜像
├── docker-compose.yml            # Docker Compose 配置
├── config.example.env            # 配置文件示例
├── data/                         # 数据存储目录（自动创建）
│   ├── lanhu_designs/            # 设计稿缓存
│   └── design_context/           # 版本化快照与素材包
└── logs/                         # 日志文件（自动创建）
```

## 🔧 高级配置

### 缓存目录

缓存目录由环境变量 `DATA_DIR` 控制。相对路径固定相对于 `.env` 所在目录；未使用 `.env` 时固定相对于源码目录，不会随 Cursor、Claude Code 等客户端的启动目录变化。

---
## 🤖 AI 助手集成

本项目专为 AI 助手设计，内置"二狗"（ErGou）助手人格：

- 🎯 **专业分析**：自动识别文档类型和最佳分析模式
- 📋 **TODO驱动**：基于任务清单的系统化工作流
- 🗣️ **中文交互**：专业的中文对话体验
- ✨ **自动化服务**：无需手动操作，AI 自动完成全流程
- 🔍 **细致严谨**：专注于准确性和质量，提供高质量技术分析
- 📝 **代码质量**：遵循严格的代码标准，避免AI生成代码的常见问题

## 📊 性能指标

- ⚡ 页面截图：~2秒/页（带缓存）
- 💾 资源下载：支持断点续传和增量更新
- 🔄 缓存命中：基于版本号的永久缓存
- 📦 批量处理：支持并发下载和分析

## 🐛 常见问题

<details>
<summary><b>Q: Cookie 过期怎么办？</b></summary>

A: 重新登录蓝湖网页版，获取新的 Cookie 并更新环境变量或配置文件。
</details>

<details>
<summary><b>Q: 截图失败或显示空白？</b></summary>

A: 确保系统已安装与当前 Playwright 版本匹配的 Chromium：
```bash
# Linux / macOS：国内镜像
PLAYWRIGHT_DOWNLOAD_HOST=https://cdn.npmmirror.com/binaries/playwright \
PLAYWRIGHT_CHROMIUM_DOWNLOAD_HOST=https://cdn.npmmirror.com/binaries/chrome-for-testing \
./venv/bin/python -m playwright install chromium

# Windows CMD：国内镜像
set PLAYWRIGHT_DOWNLOAD_HOST=https://cdn.npmmirror.com/binaries/playwright
set PLAYWRIGHT_CHROMIUM_DOWNLOAD_HOST=https://cdn.npmmirror.com/binaries/chrome-for-testing
venv\Scripts\python.exe -m playwright install chromium
```
Playwright 1.58+ 的 Chromium 使用 Chrome for Testing 路径，因此不能只设置旧的 `PLAYWRIGHT_DOWNLOAD_HOST`。一键安装脚本会自动配置双源，并兼容旧版 Playwright 和官方 CDN。
</details>

<details>
<summary><b>Q: 不用 Docker，本地 Python 按需启动（stdio）有哪些注意事项？</b></summary>

A:

1. 使用 Python 3.10–3.13（推荐 3.12），过新的版本可能缺少依赖的预编译包。
2. 运行 `bash easy-install.sh` 创建 `venv` 并安装依赖与 Chromium；MCP 客户端配置里的路径指向 `run-stdio.sh` / `run-stdio.bat` 的绝对路径，不要移动项目或删除 `venv`。
3. Intel Mac 上 `pip install` 若因 `cryptography` 需要编译 Rust 而失败，请强制使用预编译包：

```bash
./venv/bin/python -m pip install --only-binary cryptography -e .
```

4. 修改 MCP 配置或 `.env` 后，需要重启会话或在客户端里重连 MCP 才会生效。
5. 依赖含 `socksio`，httpx 可使用 SOCKS 代理；若 IDE 注入不可用 SOCKS 或需强制直连，可在 MCP `env` 中设置 `LANHU_DISABLE_SYSTEM_PROXY=1`（见 [LOCAL-STDIO.md](LOCAL-STDIO.md)）。

完整步骤见 [LOCAL-STDIO.md](LOCAL-STDIO.md)。
</details>

<details>
<summary><b>Q: 如何清理缓存？</b></summary>

A: 删除 `data/` 目录下的对应缓存文件即可。系统会自动重新下载。
</details>

## 🔒 安全说明

- ⚠️ **Cookie 安全**：请勿将含有 Cookie 的配置文件提交到公开仓库
- 🔐 **访问控制**：建议在内网环境部署或配置防火墙规则
- 📝 **数据隐私**：设计稿缓存存储在本地，请妥善保管

## 🤝 贡献指南

欢迎贡献代码！请遵循以下步骤：

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

### 开发指南

```bash
# 安装开发依赖
python -m pip install -e '.[dev]'

# 运行测试
python -m pytest tests/

# 代码格式化
black lanhu_mcp_server.py
```

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🙏 致谢

### 开源项目

- [FastMCP](https://github.com/jlowin/fastmcp) - 优秀的 MCP 服务器框架
- [Playwright](https://playwright.dev/) - 可靠的浏览器自动化工具
- [HTTPx](https://www.python-httpx.org/) - 现代化的异步 HTTP 客户端

### 服务平台

- [蓝湖](https://lanhuapp.com/) - 提供优质的设计协作平台

### 贡献者

感谢所有为这个项目做出贡献的开发者！

<!-- ALL-CONTRIBUTORS-LIST:START -->
<!-- 这里将自动生成贡献者列表 -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

### 特别感谢

- 所有提交 Issue 和 PR 的贡献者
- 所有在生产环境使用并提供反馈的团队
- 所有帮助改进文档的朋友

## 📮 联系方式

- 提交 Issue: [GitHub Issues](https://github.com/dsphper/lanhu-mcp/issues)
- 邮件: dsphper@gmail.com
<p align="center"><img src="images/wechat.jpg?v=20261103" alt="微信群二维码" width="400" /></p>

## 🗺️ 路线图

- [ ] 支持更多设计平台（Figma、Sketch）
- [ ] Web 管理界面
- [ ] 更多分析维度（前后端工时估算、技术栈推荐）
- [ ] 支持企业级权限管理
- [ ] API 文档自动生成
- [ ] 国际化支持

---

<p align="center">
  如果这个项目对你有帮助，请给它一个 ⭐️
</p>

<p align="center">
  Made with ❤️ by the Lanhu MCP Team
</p>

---
## Star History

[![Star History Chart](https://star-history.dera.page/svg?repos=dsphper/lanhu-mcp&type=date&legend=top-left)](https://star-history.dera.page/#dsphper/lanhu-mcp&type=date&legend=top-left)

---

## 🏷️ 标签 Tags

`lanhumcp` `蓝湖mcp` `lanhu-mcp` `蓝湖AI` `蓝湖skills` `lanhu-skills` `cursor-skills` `agent-skills` `lanhu-ai` `mcp-server` `cursor-plugin` `windsurf-integration` `claude-integration` `openclaw-integration` `clawbot-integration` `axure-automation` `design-to-code` `design-collaboration` `ai-development-tools` `model-context-protocol` `蓝湖插件` `蓝湖API` `OpenClaw` `ClawBot` `AI助手` `AI编程` `智能协作` `设计转代码` `设计协作` `前端开发工具`

---

## 🔍 常见搜索问题 FAQ Search

- **如何让 Cursor AI 读取蓝湖设计稿？** → 使用本精简版 Lanhu MCP Server
- **Windsurf 怎么连接蓝湖？** → 配置本 MCP 服务器
- **Claude Code 如何读取蓝湖设计稿并还原 UI？** → 通过本 MCP 的设计工具
- **OpenClaw 如何连接蓝湖？** → 直接配置 Lanhu MCP Server
- **ClawBot 怎么读取蓝湖设计稿？** → 本 MCP 服务器已原生支持
- **蓝湖有 API 吗？** → 本项目提供 MCP 协议接口
- **如何自动提取蓝湖切图？** → 使用本项目的切图工具
- **AI 如何自动生成测试用例？** → 使用测试分析模式
- **How to integrate Lanhu with Cursor?** → Install Lanhu MCP Server
- **Lanhu API for AI tools?** → Use this MCP server
- **OpenClaw Lanhu integration?** → Supported out of box
- **ClawBot design collaboration?** → Use Lanhu MCP Server
- **Design-to-code from Lanhu?** → Use this slimmed MCP server

## 🔍 SEO 关键词索引

**中文关键词**: 蓝湖mcp | lanhumcp | 蓝湖AI | 蓝湖skills | 蓝湖Skill | Cursor Skills 蓝湖 | Agent Skills 蓝湖 | 蓝湖插件 | 蓝湖API | 蓝湖Cursor | 蓝湖Windsurf | 蓝湖Claude | 蓝湖OpenClaw | 蓝湖ClawBot | OpenClaw | ClawBot | OpenClaw集成 | ClawBot集成 | AI助手 | 蓝湖设计稿 | 蓝湖切图 | 蓝湖设计稿 | 设计转代码 | MCP服务器 | 模型上下文协议

**English Keywords**: lanhu mcp | lanhu-mcp | lanhu ai | lanhu skills | cursor skills lanhu | agent skills lanhu | lanhu cursor | lanhu windsurf | lanhu claude | lanhu api | lanhu integration | lanhu openclaw | lanhu clawbot | openclaw mcp | clawbot mcp | mcp server | model context protocol | design-to-code | design collaboration | ai development tools

**适用人群**: 产品经理 | 前端开发 | 后端开发 | 测试工程师 | UI设计师 | 使用Cursor的开发者 | 使用Windsurf的开发者 | 使用Claude的开发者 | AI编程爱好者

---
## ⚠️ 免责声明

本项目（Lanhu MCP Server）是一个**第三方开源项目**，由社区开发者独立开发和维护，**并非蓝湖官方产品**。

**重要说明：**
- 本项目与蓝湖公司无任何官方关联或合作关系
- 本项目通过公开的网页接口与蓝湖平台交互，不涉及任何未授权访问
- 使用本项目需要您拥有合法的蓝湖账号和访问权限
- 请遵守蓝湖平台的服务条款和使用政策
- 本项目仅供学习和研究使用，使用者需自行承担使用风险
- 开发者不对因使用本项目导致的任何数据丢失、账号问题或其他损失承担责任

**数据和隐私：**
- 本项目在本地处理和缓存数据，不会向第三方服务器传输您的数据
- 您的蓝湖 Cookie 和项目数据仅存储在您的本地环境中
- 请妥善保管您的凭证信息，不要分享给他人

**开源协议：**
- 本项目采用 MIT 开源协议，按"原样"提供，不提供任何形式的保证
- 详见 [LICENSE](LICENSE) 文件

如有任何疑问或建议，欢迎通过 [GitHub Issues](https://github.com/dsphper/lanhu-mcp/issues) 与我们交流。

<!-- Last checked: 2026-09-11 02:59 -->
