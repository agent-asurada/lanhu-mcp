# 🎬 使用演示 / Demo

本文档展示精简后的 Lanhu MCP Server 在 Design-to-Code 场景下的用法。需求分析、团队留言板等演示已随对应工具移除。

## 场景三：UI 设计稿查看

### 用户输入
```
帮我看看首页的设计稿：
https://lanhuapp.com/web/#/item/project/stage?tid=xxx&pid=xxx
```

### AI 执行
```
老板，这个项目有 12 张设计图，设计师很用心！

您是要：
1. 全部下载（完整了解UI）
2. 先看关键页面（告诉我哪几张~）

等您指示！
```

### 用户选择
```
先看首页、登录页、个人中心这三张
```

### AI 返回
```
老板，设计稿已下载！

📊 共 3 张设计图：
1. 首页设计 (1920x1080)
2. 登录页 (750x1334)
3. 个人中心 (750x1334)

[图片1：首页设计]
[图片2：登录页]
[图片3：个人中心]

这些设计很不错！需要我分析设计规范或导出切图吗？
```

---

## 场景四：切图智能下载

### 用户输入
```
帮我下载"首页设计"的所有切图
```

### AI 执行流程

#### 1. 环境检测
```
正在检测项目类型...
✅ 检测到 package.json，这是一个 React 项目
✅ 发现 src/assets/images/ 目录
```

#### 2. 智能规划
```
二狗的下载计划：
📁 输出目录：src/assets/images/slices/homepage/
🏷️ 命名规则：基于图层路径的语义化命名
📦 共发现 23 个切图
```

#### 3. 批量下载
```
开始下载...
[1/23] ✅ status_bar_battery_icon_26x14.png
[2/23] ✅ navbar_logo_120x40.png
[3/23] ✅ btn_primary_background_200x50.png
...
[23/23] ✅ footer_wechat_qrcode_100x100.png

✅ 全部完成！23 个切图已保存到：
   src/assets/images/slices/homepage/

老板，切图都下载好了！文件名都是语义化的，直接用吧~
```

---


## 快速体验

想亲自体验？只需 3 步：

```bash
# 1. 克隆项目
git clone https://github.com/yourusername/lanhu-mcp-server.git

# 2. 快速启动
cd lanhu-mcp-server
./quickstart.sh  # Mac/Linux
# 或 quickstart.bat  # Windows

# 3. 配置 Cursor
# 在 MCP 配置中添加（建议使用英文参数）：
{
  "mcpServers": {
    "lanhu": {
      "url": "http://localhost:8000/mcp"
    }
  }
}
```

开始对话：
```
帮我看看这个设计稿：[粘贴蓝湖URL]
```

就是这么简单！🎉
