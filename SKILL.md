---
name: xhs-note-creator
description: 小红书笔记素材创作技能。当用户需要创建小红书笔记素材时使用这个技能。技能包含：根据用户的需求和提供的资料，撰写小红书笔记内容（标题+正文），生成图片卡片（封面+正文卡片）。
---

# 小红书笔记和图片卡片创作

这个技能用于创建专业的小红书笔记素材，包括内容撰写和图片卡片生成。

> ⚠️ **重要说明**：小红书平台禁止 AI 工具自动登录或发布笔记。本技能只生成可直接在小红书手动发布的素材（标题、正文、图片卡片），由用户自行在小红书 App 或网页端完成发布。

## 使用场景

- 用户需要创建小红书笔记时
- 用户提供资料需要转化为小红书风格内容时
- 用户需要生成精美的图片卡片时

## 工作流程

### 第一步：撰写小红书笔记内容

根据用户需求和提供的资料，创作符合小红书风格的内容：

#### 标题要求
- 不超过 20 字
- 吸引眼球，制造好奇心
- 可使用数字、疑问句、感叹号增强吸引力
- 示例：「5个让效率翻倍的神器推荐！」「震惊！原来这样做才对」

#### 正文要求
- 使用良好的排版，段落清晰
- 点缀少量 Emoji 增加可读性（每段 1-2 个即可）
- 使用简短的句子和段落
- 结尾给出 SEO 友好的 Tags 标签（5-10 个相关标签）

### 第二步：生成 Markdown 文档

**注意：这里生成的 Markdown 文档是用于渲染卡片的，必须专门生成，禁止直接使用上一步的笔记正文内容。**

Markdown 文件，文件应包含：

1. YAML 头部元数据（封面信息）：
```yaml
---
emoji: "🚀"           # 封面装饰 Emoji
title: "大标题"        # 封面大标题（不超过15字）
subtitle: "副标题文案"  # 封面副标题（不超过15字）
---
```

2. 用于渲染卡片的 Markdown 文本内容：
   - 当待渲染内容必须严格切分为独立的数张图片时，可使用 `---` 分割线主动将正文分隔为多个卡片段落（每个段落文本控制在 200 字左右），输出图片时使用参数`-m separator`
   - 当待渲染内容无需严格分割，生成正常 Markdown 文本即可，跟下方分页模式参数规则按需选择

完整 Markdown 文档内容示例：

```markdown
---
emoji: "💡"
title: "5个效率神器让工作效率翻倍"
subtitle: "对着抄作业就好了，一起变高效"
---

# 📝 神器一：Notion

> 全能型笔记工具，支持数据库、看板、日历等多种视图...

## 特色功能

- 特色一
- 特色二

# ⚡ 神器二：Raycast

\`\`\`
可使用代码块来增加渲染后图片的视觉丰富度
\`\`\`

## 推荐原因

- 原因一
- 原因二
- ……


# 🌈 神器三：Arc

全新理念的浏览器，侧边栏标签管理...

...

```

### 第三步：渲染图片卡片

将 Markdown 文档渲染为图片卡片。使用以下脚本渲染：

```bash
python scripts/render_xhs.py <markdown_file> [options]
```

- 默认输出目录为当前工作目录
- 生成的图片包括：封面（cover.png）和正文卡片（card_1.png, card_2.png, ...）

#### 渲染参数（Python）

| 参数 | 简写 | 说明 | 默认值 |
|---|---|---|---|
| `--output-dir` | `-o` | 输出目录 | 当前工作目录 |
| `--theme` | `-t` | 排版主题 | `default` |
| `--mode` | `-m` | 分页模式 | `separator` |
| `--width` | `-w` | 图片宽度 | `1080` |
| `--height` |  | 图片高度（`dynamic` 下为最小高度） | `1440` |
| `--max-height` |  | `dynamic` 最大高度 | `4320` |
| `--dpr` |  | 设备像素比（清晰度） | `2` |

#### 排版主题（`--theme`）

- `default`：默认简约浅灰渐变背景（`#f3f3f3 -> #f9f9f9`）
- `playful-geometric`：活泼几何（Memphis）
- `neo-brutalism`：新粗野主义
- `botanical`：植物园自然
- `professional`：专业商务
- `retro`：复古怀旧
- `terminal`：终端命令行
- `sketch`：手绘素描

#### 分页模式（`--mode`）

- `separator`：按 `---` 分隔符分页（适合内容已手动控量）
- `auto-fit`：固定尺寸下自动缩放文字，避免溢出/留白（适合封面+单张图片但尺寸固定的情况）
- `auto-split`：按渲染后高度自动切分分页（适合切分不影响阅读的长文内容）
- `dynamic`：根据内容动态调整图片高度（注意：图片最高 4320，字数超过 550 的不建使用此模式）

#### 常用示例

```bash
# 1) 默认主题 + 手动分隔分页
python scripts/render_xhs.py content.md -m separator

# 2) 固定 1080x1440，自动缩放文字，尽量填满画面
python scripts/render_xhs.py content.md -m auto-fit

# 3) 自动切分分页（推荐：内容长短不稳定）
python scripts/render_xhs.py content.md -m auto-split

# 4) 动态高度（允许不同高度卡片）
python scripts/render_xhs.py content.md -m dynamic --max-height 4320

# 5) 切换主题
python scripts/render_xhs.py content.md -t playful-geometric -m auto-split
```

#### Node.js 渲染（可选）

```bash
node scripts/render_xhs.js content.md -t default -m separator
```

Node.js 参数与 Python 基本一致：`--output-dir/-o`、`--theme/-t`、`--mode/-m`、`--width/-w`、`--height`、`--max-height`、`--dpr`。

### 第四步：交付素材，手动发布

技能完成的内容包括：
- 一份 Markdown 笔记文档（包含 YAML 头部的封面信息 + 正文内容）
- 一张封面图：`cover.png`
- 多张正文卡片图：`card_1.png`、`card_2.png`、...

**用户手动发布流程**（在小红书 App 或网页端）：
1. 在浏览器打开 https://www.xiaohongshu.com 并登录
2. 点击"发布笔记" → 选择"上传图片"
3. 选择本技能生成的 `cover.png` 和所有 `card_*.png`
4. 复制笔记文档中的标题到标题字段
5. 复制笔记文档中的正文到正文字段
6. 添加 5-10 个相关 Tags
7. 提交发布

> ⚠️ **不要**使用任何脚本或工具自动登录、提交或发布到小红书。平台明确禁止 AI 工具进行此类自动化操作。

## 图片规格说明

### 封面卡片
- 尺寸比例：3:4（小红书推荐比例）
- 基准尺寸：1080×1440px
- 包含：Emoji 装饰、大标题、副标题
- 样式：渐变背景 + 圆角内容区

### 正文卡片
- 尺寸比例：3:4
- 基准尺寸：1080×1440px
- 支持：标题、段落、列表、引用、代码块、图片
- 样式：白色卡片 + 渐变背景边框

## 技能资源

### 脚本文件
- `scripts/render_xhs.py` - Python 渲染脚本
- `scripts/render_xhs.js` - Node.js 渲染脚本
- `scripts/render_xhs_v2.py` - Python 渲染脚本（实验性质，支持更多样式）
- `scripts/render_xhs_v2.js` - Node.js 渲染脚本（实验性质，支持更多样式）
- `scripts/comment_manager.py` - 评论管理辅助脚本

### 资源文件
- `assets/cover.html` - 封面 HTML 模板
- `assets/card.html` - 正文卡片 HTML 模板
- `assets/styles.css` - 共用样式表

## 注意事项

1. Markdown 文件应保存在工作目录，渲染后的图片也保存在工作目录
2. 技能目录 (`xhs-note-creator/`) 仅存放脚本和模板，不存放用户数据
3. 图片尺寸会根据内容自动调整，但保持 3:4 比例
4. 生成的图片素材由用户自行在小红书平台手动发布，不要使用任何脚本尝试自动登录或发布
5. 渲染依赖 Playwright 浏览器，需要运行 `playwright install Chrome`
