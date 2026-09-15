# 小红书笔记和图片卡片创作技能 (XHS Note Creator)


## 🎯 核心功能

### 1. 内容创作
- 小红书风格的标题（≤20字，吸引眼球）
- 格式化正文（段落清晰，点缀 Emoji）
- 自动生成 SEO 友好的 Tags

### 2. 图片卡片生成（亮点）
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

> ⚠️ **重要**：小红书平台禁止 AI 工具自动登录或发布笔记。本技能只生成可直接发布的素材（标题、正文、图片卡片），由用户自行在小红书 App 或网页端手动发布。

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
4. **交付素材** → 用户自行在小红书平台手动发布

## 🛠 技术栈

- **Markdown 渲染**：Playwright + HTML/CSS
- **样式系统**：CSS 主题 + 动态注入
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
│   ├── render_xhs_v2.py  # Python 渲染脚本 v2（更多样式）
│   ├── render_xhs_v2.js  # Node.js 渲染脚本 v2（更多样式）
│   ├── comment_manager.py # 评论辅助脚本
│   └── publish_xhs.py    # 发布脚本（仅供研究，请勿使用，平台禁止 AI 自动发布）
└── demos/                # 示例输出
```

## 📄 许可证

MIT License © 2026

## 🙏 致谢

- [OpenClaw](https://openclaw.ai) - 个人 AI 助手平台
- [Playwright](https://playwright.dev/) - 浏览器自动化

---

**状态**：✅ 内容创作和图片生成功能已完成
**状态**：🚫 不再支持自动发布（小红书平台禁止 AI 工具自动登录和发布）
