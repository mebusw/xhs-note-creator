# 小红书笔记创作技能 (XHS Note Creator)

> OpenClaw Agent Skill for creating Xiaohongshu (小红书) notes with AI-generated content and beautifully styled image cards.

## 🎯 核心功能

### 1. 内容创作
- 小红书风格的标题（≤20字，吸引眼球）
- 格式化正文（段落清晰，点缀 Emoji）
- 自动生成 SEO 友好的 Tags

### 2. 图片生成（亮点）
- **8 套主题皮肤**：
  - 默认简约灰
  - Playful Geometric（孟菲斯风格）
  - Neo-Brutalism（新粗野主义）
  - Botanical（植物园自然）
  - Professional（专业商务）
  - Retro（复古怀旧）
  - Terminal（终端命令行）
  - Sketch（手绘素描）

- **4 种分页模式**：
  - `separator`：按 `---` 分隔手动分页
  - `auto-fit`：固定尺寸自动缩放
  - `auto-split`：根据高度自动拆分
  - `dynamic`：动态调整图片高度

- 小红书推荐 3:4 比例（1080×1440px）

### 3. 发布功能（可选）
- 支持自动发布到小红书
- 支持定时发布
- 支持私密笔记

## 📦 安装

```bash
# 通过 ClawHub 安装（推荐）
clawhub install xhs-note-creator

# 或手动安装
# 1. 下载 xhs-note-creator.skill 文件
# 2. 放入 ~/.openclaw/skills/ 目录
# 3. 重启 OpenClaw Gateway
```

### 依赖安装

**Python：**
```bash
pip install -r requirements.txt
playwright install chromium
```

**Node.js：**
```bash
npm install
npx playwright install chromium
```

## 🚀 使用方法

### 触发方式
当用户需要创建小红书笔记时，Agent 会自动触发此技能。例如：
- "帮我写一篇关于效率神器的小红书笔记"
- "把这个资料转成小红书风格的内容"
- "生成小红书图片卡片"

### 工作流程

1. **撰写内容** → Agent 根据需求创作标题和正文
2. **生成 Markdown** → 创建带 YAML 头部的 Markdown 文件
3. **渲染图片** → 使用脚本生成封面和正文卡片
4. **发布（可选）** → 自动发布到小红书

## ⚠️ 未解决的重大问题：自动登录验证

### 问题描述
当前技能的**发布功能**存在关键限制：

**Cookie 需要手动获取，且会过期**

目前的流程：
1. 用户需要在浏览器中手动登录小红书
2. 打开开发者工具（F12）→ Network 标签
3. 复制请求头中的 Cookie 字符串
4. 设置环境变量 `XHS_COOKIE` 或存入 memory

**问题**：
- ❌ Cookie 有效期有限（通常几天到几周）
- ❌ 过期后需要重复手动操作
- ❌ 无法实现真正的"自动化"发布
- ❌ 用户体验差

### 急需社区帮助

我们需要解决以下技术难题：

#### 方案 1：自动登录 + 验证码处理
- 使用 Playwright 模拟浏览器登录
- 处理短信验证码/滑块验证码
- 自动保存和刷新 Cookie

#### 方案 2：二维码登录
- 生成小红书登录二维码
- 用户扫码授权
- 获取并保存长期有效的 Token

#### 方案 3：官方 API
- 小红书是否提供官方开发者 API？
- 需要申请开发者账号和权限

#### 方案 4：逆向工程
- 分析小红书 Web/App 的登录流程
- 找到 Token 刷新机制
- 实现自动续期

### 贡献方式

如果你有解决方案或想法，欢迎：
1. 提交 Issue 讨论技术方案
2. 提交 Pull Request 实现功能
3. 在 OpenClaw Discord 社区讨论

## 🛠 技术栈

- **Markdown 渲染**：Playwright + HTML/CSS
- **样式系统**：CSS 主题 + 动态注入
- **小红书 API**：xhs Python 库（基于逆向）
- **图像处理**：Pillow

## 📁 文件结构

```
xhs-note-creator/
├── SKILL.md              # 技能定义文件
├── README.md             # 本文件
├── requirements.txt      # Python 依赖
├── package.json          # Node.js 依赖
├── assets/               # 模板和样式
│   ├── cover.html        # 封面模板
│   ├── card.html         # 正文卡片模板
│   ├── styles.css        # 基础样式
│   └── themes/           # 主题样式
├── scripts/              # 可执行脚本
│   ├── render_xhs.py     # Python 渲染脚本
│   ├── render_xhs.js     # Node.js 渲染脚本
│   └── publish_xhs.py    # 发布脚本
└── demos/                # 示例输出
```

## 📄 许可证

MIT License © 2026

## 🙏 致谢

- [OpenClaw](https://openclaw.ai) - 个人 AI 助手平台
- [Playwright](https://playwright.dev/) - 浏览器自动化
- [xhs](https://github.com/ReaJason/xhs) - 小红书 API 客户端

---

**状态**：✅ 内容创作和图片生成功能已完成
**状态**：🚧 自动登录验证待解决（需要社区帮助）
