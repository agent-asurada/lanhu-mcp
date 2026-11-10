# 🎨 Lanhu MCP Server | 蓝湖MCP服务器

**lanhumcp | lanhu-mcp | Lanhu AI Integration | MCP Server for Lanhu**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io/)
[![FastMCP](https://img.shields.io/badge/FastMCP-Powered-orange.svg)](https://github.com/jlowin/fastmcp)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![GitHub Stars](https://img.shields.io/github/stars/dsphper/lanhu-mcp?style=social)](https://github.com/dsphper/lanhu-mcp/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/dsphper/lanhu-mcp)](https://github.com/dsphper/lanhu-mcp/issues)
[![GitHub Release](https://img.shields.io/github/v/release/dsphper/lanhu-mcp)](https://github.com/dsphper/lanhu-mcp/releases)
[![Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-2.0-4baaaa.svg)](CODE_OF_CONDUCT.md)

A slimmed [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server (fork of [dsphper/lanhu-mcp](https://github.com/dsphper/lanhu-mcp)) for **design-to-code**: list Lanhu UI designs, read specs/layers/styles, fetch previews, and download slices/assets. Requirement/PRD analysis, the team message board, and Feishu notifications have been removed.

**v1.8.7 adds `socksio` for local stdio SOCKS proxies, optional `LANHU_DISABLE_SYSTEM_PROXY`, and a Docker-free stdio guide.** See [the release notes](RELEASE_NOTES_v1.8.7.md) and [LOCAL-STDIO.md](LOCAL-STDIO.md).

**MCP client integration:**

Client support depends on its MCP transport and image/resource capabilities. For visual design work, use a multimodal model and a client that can display MCP images and read resources.

**International Mainstream AI IDEs**:
- ✅ **Cursor** - Cursor AI directly reads Lanhu designs
- ✅ **Windsurf** - Windsurf Cascade AI directly reads Lanhu designs
- ✅ **Claude Desktop** - Claude AI desktop app directly accesses Lanhu
- ✅ **Continue** - VSCode/JetBrains AI coding assistant
- ✅ **Cline** - Powerful VSCode AI programming plugin
- ✅ **GitHub Copilot Workspace** - GitHub AI development environment

**Chinese AI IDEs & Coding Assistants**:
- ✅ **ByteDance Trae** - China's first AI-native IDE (Doubao-1.5-pro)
- ✅ **Alibaba Tongyi Lingma** - AI assistant based on Tongyi model
- ✅ **Tencent CodeBuddy** - Full-cycle AI integrated workbench
- ✅ **Baidu Wenxin Kuaima** - Baidu AI coding assistant
- ✅ **Kuaishou KwaiCoder** - Kuaishou AI programming tool
- ✅ **Zhipu CodeGeeX** - Tsinghua-based AI coding assistant
- ✅ **Huawei Cloud CodeArts Snap** - Huawei Cloud AI assistant
- ✅ **SenseTime SenseCode** - SenseTime AI programming tool

**Any MCP-compatible AI development tools**

English | [简体中文](README.md)

## ✨ Key Features

**Perfect for**: Frontend Developers | UI implementation from Lanhu designs | Cursor, Claude Code, Windsurf, and other MCP clients

### 🎨 UI Design Support
- **Design listing**: Get the project's UI design images and IDs
- **Versioned visual context**: Pin a design snapshot, then inspect regions using a clean crop, a numbered overlay, stable node IDs and source styles
- **Original asset delivery**: Select designer exports, download and verify files, then install a portable bundle in the frontend project
- **Source format selection**: Choose `original`, `prefer_svg` or `raster`
- **Typography evidence**: Read mixed-style text runs and font requirements
- **HTML/CSS reference**: Optional derived layout from the design schema; always verify against source annotations and the preview
- **Invite link resolution**: Turn a Lanhu share/invite URL into a project URL with `pid`

The model interprets component roles and chooses how to use assets. See [Visual design context and asset delivery](DESIGN_CONTEXT.md) for the full contract and limitations.

---
## 🚀 Quick Start

> **Visual design tasks need a vision-capable model.** The client must display MCP image content to the model. Text-only clients can list designs and metadata, but cannot interpret design screenshots.

---

### Prerequisites

- Python 3.10+
- FastMCP `>=3.0.2,<4` and Pillow `>=10.4.0` (installed by the commands below)
- Docker (optional, for containerized deployment)

### Installation

```bash
# Clone the repository
git clone https://github.com/dsphper/lanhu-mcp.git
cd lanhu-mcp

# Create an isolated environment, then install the package and Chromium
python3 -m venv venv
./venv/bin/python -m pip install -e .
./venv/bin/python -m playwright install chromium
cp .env.example .env

# Windows equivalents
python -m venv venv
venv\Scripts\python.exe -m pip install -e .
venv\Scripts\python.exe -m playwright install chromium
copy .env.example .env
```

Edit `.env` and replace `your_lanhu_cookie_here` with your Lanhu Cookie. macOS may still provide Python 3.9 as `/usr/bin/python3`; verify `python3 --version` before creating the environment. The bundled installer scripts prefer Aliyun PyPI with automatic Tsinghua/PyPI fallback and use npmmirror for Chromium; set `PIP_INDEX_URL` or `PLAYWRIGHT_DOWNLOAD_HOST` to override them.

With `uv`, create the environment first: `uv venv venv --python 3.13`, then run `uv pip install --python venv/bin/python -e .`.

For an existing source checkout, upgrade dependencies with `python -m pip install -U -r requirements.txt` and reinstall the package with `python -m pip install -e .`. Restart the server and reconnect the MCP client so it discovers the design-to-code tools. Docker users should rebuild the image; restarting an old image does not load the new package or dependencies.

### Single-file deployment upgrades

`lanhu_mcp_server.py` embeds the project's own `lanhu_design` implementation. In an environment where the declared third-party dependencies are installed, replacing this one file provides the full design-to-code tools; copying a separate `lanhu_design/` directory is no longer required. A full repository, wheel, or Docker installation remains recommended for dependency upgrades.

### Configuration

1. **Set Lanhu Cookie** (Required)

```bash
export LANHU_COOKIE="your_lanhu_cookie_here"
```

> 💡 Get Cookie: Log in to Lanhu web version, open browser developer tools, and copy Cookie from request headers

2. **Other Environment Variables** (Optional)

```bash
# Server Configuration
export SERVER_HOST="0.0.0.0"       # Server listen address
export SERVER_PORT=8000            # Server port

# Data Storage
export DATA_DIR="./data"           # Data storage directory

# Performance Tuning
export HTTP_TIMEOUT=30             # HTTP request timeout (seconds)
```

> 📝 For complete environment variable documentation, see `config.example.env`

### Running

**Method 1: Direct Run**

```bash
# Linux/macOS
./venv/bin/lanhu-mcp --transport http --host 127.0.0.1 --port 8000

# Windows
venv\Scripts\lanhu-mcp.exe --transport http --host 127.0.0.1 --port 8000

# For a client that starts its own stdio server
./run-stdio.sh
```

Server will start at `http://localhost:8000/mcp`.

**Method 2: Docker Deployment**

```bash
docker build -t lanhu-mcp-server .
docker run -p 8000:8000 \
  -e LANHU_COOKIE="your_cookie" \
  -v $(pwd)/data:/app/data \
  lanhu-mcp-server
```

Or use docker-compose:

```bash
# Edit environment variables in docker-compose.yml
docker-compose up -d
```

### Connect to AI Client

Configure in MCP-compatible AI clients (e.g., Claude Code, Cursor, Windsurf):

**Cursor Configuration Example:**
```json
{
  "mcpServers": {
    "lanhu": {
      "url": "http://localhost:8000/mcp?role=Backend&name=John"
    }
  }
}
```

> 📌 URL Parameters:
> - `role`: User role (Backend/Frontend/Tester/Product, etc.)
> - `name`: User name (for collaboration tracking and @mentions)

## 📖 Usage Guide

### UI Design Viewing

```
Please show me this design:
https://lanhuapp.com/web/#/item/project/stage?tid=xxx&pid=xxx
```

If the user sends an invite/share link, call `lanhu_resolve_invite_link` first.

### Visual Inspection and Asset Installation

```
Use this design version to implement the page. Inspect its regions, select the
source assets by node ID, prefer SVG where available, and install the asset
bundle in this project's public directory.
```

The coding agent can execute this sequence:

1. Find the design with `lanhu_get_designs`, then call `lanhu_get_design_overview` to obtain a stable `snapshot_id`.
2. Call `lanhu_inspect_design_region` with source coordinates or node IDs. Use the clean image, numbered image, text runs and asset IDs together.
3. Call `lanhu_export_design_assets` with the chosen `asset_ids` and `format_preference="prefer_svg"`. Check the export status, source resolution and fallback fields.
4. Read the returned `bundle_resource` through MCP and install it **on the coding agent's machine**:

```bash
python -m lanhu_design.install \
  --mcp-url 'http://localhost:8000/mcp' \
  --resource-uri 'lanhu://design/SNAPSHOT_ID/bundle/BUNDLE_ID' \
  --output '/absolute/path/to/frontend/public'
```

The installer verifies the manifest and hashes, then writes `install-receipt.json` with the asset/node IDs and actual local paths. The server's cache path is not a client path. `lanhu_get_design_slices` returns download metadata rather than installing files. See [DESIGN_CONTEXT.md](DESIGN_CONTEXT.md) for details.

`lanhu_get_ai_analyze_design_result` remains available for a full-design preview and derived HTML/CSS reference; prefer the versioned overview/inspect tools for implementation work.

## 🛠️ Available Tools

| Tool Name | Description | Use Case |
|-----------|-------------|----------|
| `lanhu_resolve_invite_link` | Parse invite/share link into a project URL | When the user provides a share link |
| `lanhu_get_designs` | Get UI design list | Must call before viewing designs |
| `lanhu_get_design_overview` | Pin a version and return a visual snapshot with stable node IDs | Start visual implementation from a specific design |
| `lanhu_inspect_design_region` | Inspect a region or nodes, clean/numbered crops, text runs and asset candidates | Connect visible elements to source specifications |
| `lanhu_export_design_assets` | Download and verify original/SVG/raster variants; return a portable bundle | Install selected assets on the coding agent's machine |
| `lanhu_get_ai_analyze_design_result` | Full-design preview and HTML/CSS reference | Quick review and derived layout |
| `lanhu_get_design_slices` | Get slice URLs and metadata | Download icons and assets |

MCP resources: `lanhu://design/{snapshot_id}/preview/{preview_id}` (preview crop) and `lanhu://design/{snapshot_id}/bundle/{bundle_id}` (asset zip).

---
## 📁 Project Structure

```
lanhu-mcp-server/
├── lanhu_mcp_server.py          # Main server file
├── lanhu_design/                # Snapshots, regions, text, source variants and bundle installer
├── DESIGN_CONTEXT.md            # Visual tool and asset delivery contract
├── requirements.txt              # Python dependencies
├── Dockerfile                    # Docker image
├── data/                         # Data storage directory
│   ├── lanhu_designs/            # Legacy design cache
│   └── design_context/           # Versioned visual snapshots and asset bundles
├── logs/                         # Log files
└── README.md                     # This document
```

## 🔧 Advanced Configuration

### Cache directory

The cache directory is controlled by `DATA_DIR`. A relative path is anchored to the `.env` directory, or to the source directory when no `.env` exists, so changing the MCP client's working directory does not create a second cache.

```bash
export DATA_DIR="/path/to/cache"
```

## 🤖 AI Assistant Integration

This project is designed for AI assistants with built-in "ErGou" assistant persona:

- 🎯 **Smart Analysis**: Read design snapshots, styles, and assets for UI implementation
- 📋 **TODO-Driven**: Systematic workflow based on task lists
- 🗣️ **Natural Interaction**: Friendly conversational experience
- ✨ **Tool-Assisted Execution**: The model can inspect, select and install assets; implementation choices and visual verification remain its responsibility

## 📊 Performance and Verification

- Screenshot latency depends on source size, browser startup, network and cache state; no universal timing or accuracy benchmark is claimed.
- Verified asset files can be reused; the installer skips identical local files and rejects conflicting content.
- Visual snapshots isolate versions and preserve image/node provenance. The new APIs do not change every legacy cache path.
- An anonymized exploration covered 57 Sketch-source designs and 12 representative workflow scenarios. Before the oversized-image fix, 11 scenarios completed; the previously failing large-image case now passes bounded-preview and native-region checks through an installed package. Real Figma and Photoshop imports are not yet covered by that sample set.
- Regression tests cover source identity, coordinates, versions, regional images, mixed text styles, variant selection and bundle installation. These are data/delivery checks, not a measured frontend reconstruction rate.

## 🐛 FAQ

<details>
<summary><b>Q: What if Cookie expires?</b></summary>

A: Re-login to Lanhu web version, get new Cookie and update environment variable or config file.
</details>

<details>
<summary><b>Q: Screenshot fails or shows blank?</b></summary>

A: Ensure Playwright browsers are installed:
```bash
./venv/bin/python -m playwright install chromium
```
</details>

<details>
<summary><b>Q: Feishu notification fails?</b></summary>

A: Check:
1. Webhook URL is correct
2. Feishu bot is added to the group
3. User ID mapping is correctly configured
</details>

<details>
<summary><b>Q: How to clear cache?</b></summary>

A: Delete corresponding cache files in `data/` directory. System will automatically re-download.
</details>

## 🔒 Security Notes

- ⚠️ **Cookie Security**: Do not commit config files containing cookies to public repositories
- 🔐 **Access Control**: Recommend deploying in intranet or configuring firewall rules
- 📝 **Data Privacy**: Message data is stored locally, please keep it safe

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork this repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

### Development Guide

```bash
# Install development dependencies
python -m pip install -e '.[dev]'

# Run tests
python -m pytest tests/

# Code formatting
black lanhu_mcp_server.py
```

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [FastMCP](https://github.com/jlowin/fastmcp) - Excellent MCP server framework
- [Playwright](https://playwright.dev/) - Reliable browser automation tool
- Lanhu Team - Providing quality design collaboration platform

## 📮 Contact

- Submit Issue: [GitHub Issues](https://github.com/dsphper/lanhu-mcp/issues)
- Email: dsphper@gmail.com

## 🗺️ Roadmap

- [ ] Validate more real Figma and Photoshop imports; current cross-project samples are Sketch-source designs
- [ ] Add implementation screenshot/DOM comparison and font availability checks
- [ ] Web management interface
- [ ] More analysis dimensions (Effort estimation, Tech stack recommendations)
- [ ] Enterprise-level permission management
- [ ] API documentation auto-generation
- [ ] Internationalization support

---

<p align="center">
  If this project helps you, please give it a ⭐️
</p>

<p align="center">
  Made with ❤️ by the Lanhu MCP Team
</p>

---

## ⚠️ Disclaimer

This project (Lanhu MCP Server) is a **third-party open source project**, independently developed and maintained by community developers, and **is NOT an official Lanhu product**.

**Important Notes:**
- This project has no official affiliation or partnership with Lanhu (蓝湖) company
- This project interacts with the Lanhu platform through public web interfaces, without any unauthorized access
- Using this project requires you to have a legitimate Lanhu account and access permissions
- Please comply with Lanhu platform's terms of service and usage policies
- This project is for learning and research purposes only, users assume all risks of use
- Developers are not responsible for any data loss, account issues, or other damages caused by using this project

**Data and Privacy:**
- This project reads Lanhu APIs and asset CDNs and caches data on the configured MCP server. A remote deployment stores that cache on the server, not on each client.
- Optional Feishu notifications send message content to the configured webhook. Export bundles and images are transferred to requesting MCP clients.
- Keep the configured Lanhu Cookie, project cache and exported bundles private; the service uses one configured account rather than per-caller Lanhu authorization.
- Please keep your credentials secure and do not share them with others

**Open Source License:**
- This project is licensed under the MIT License, provided "as is" without warranty of any kind
- See [LICENSE](LICENSE) file for details

If you have any questions or suggestions, please feel free to communicate with us through [GitHub Issues](https://github.com/dsphper/lanhu-mcp/issues).

---

## 🏷️ Tags

`lanhumcp` `lanhu-mcp` `lanhu-ai` `mcp-server` `cursor-plugin` `windsurf-integration` `claude-integration` `trae-integration` `tongyi-lingma` `codebuddy` `cline-plugin` `continue-plugin` `design-to-code` `design-collaboration` `ai-development-tools` `model-context-protocol` `lanhu-api` `lanhu-cursor` `lanhu-windsurf` `lanhu-claude` `ai-coding-assistant` `design-handoff` `bytedance-ai` `alibaba-ai` `tencent-ai` `baidu-ai`

---

## 🔍 Common Search Questions

- **How to connect Cursor AI with Lanhu?** → Use Lanhu MCP Server
- **Windsurf Lanhu integration?** → Configure this MCP server
- **Claude Code read Axure prototypes?** → Install Lanhu MCP
- **ByteDance Trae Lanhu connection?** → Use this MCP server
- **Alibaba Tongyi Lingma Lanhu integration?** → Configure Lanhu MCP
- **Tencent CodeBuddy support Lanhu?** → Connect via MCP protocol
- **Baidu Wenxin Kuaima integrate Lanhu?** → Use this project
- **Cline plugin access Lanhu?** → Configure MCP server
- **Lanhu API for AI tools?** → This project provides MCP interface
- **Automated slice extraction from Lanhu?** → Use slice tools in this project
- **AI automated test case generation?** → Use tester analysis mode
- **蓝湖 Cursor 集成？** → 安装 Lanhu MCP Server
- **如何让 AI 读取蓝湖需求？** → 使用本 MCP 服务器
- **字节 Trae 蓝湖连接？** → 配置本 MCP 服务器
- **通义灵码蓝湖集成？** → 使用 Lanhu MCP

---

<!-- Last checked: 2026-09-11 02:59 -->
