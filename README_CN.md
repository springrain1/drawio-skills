# Draw.io Skill - Claude Code 图表绘制技能

[Documentation](https://bahayonghang.github.io/drawio-skills/) | [中文文档](https://bahayonghang.github.io/drawio-skills/zh/)

一个为 Claude Code 打造的技能，支持 AI 驱动的图表创建与编辑，并提供实时浏览器预览。

## 与上游项目的关系

本技能基于 [@DayuanJiang](https://github.com/DayuanJiang) 开发的 **[next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io)** 项目构建。

| 项目 | 作用 |
|------|------|
| [next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io) | 提供 draw.io 图表工具的 MCP Server |
| **本项目 (drawio-skills)** | Claude Code 技能，封装 MCP server 并提供工作流指导、XML 格式参考和图表模式 |

### 本技能的增强内容

- **工作流指导**：创建和编辑图表的完整流程说明
- **XML 格式参考**：样式属性、常用形状的详细文档
- **图表模式**：流程图和架构图的模板与最佳实践
- **自动 MCP 配置**：通过 `.mcp.json` 自动配置 MCP server
- **SVG 转换器**：Python 脚本 (`drawio_to_svg.py`)，将 `.drawio` 文件转换为可编辑的 SVG
- **示例图表**：提供现成的 `.drawio` 和 `.svg` 示例文件

## 安装方法

### 从 GitHub 安装

```bash
# 将本仓库克隆到 Claude Code 技能目录
git clone https://github.com/bahayonghang/drawio-skills.git ~/.claude/skills/drawio
```

安装完成后，技能将在 Claude Code 中自动可用。

## 功能特性

| 功能 | 说明 |
|------|------|
| **自然语言转图表** | 描述你的需求，获得专业图表 |
| **实时预览** | 在浏览器中即时查看变更 |
| **流程图** | 流程、决策树、工作流 |
| **架构图** | 系统架构、微服务、部署图 |
| **编辑现有图表** | 基于 ID 的精确修改操作 |
| **导出功能** | 保存为 `.drawio` 文件 |
| **SVG 转换** | 将 `.drawio` 转换为可编辑 SVG（可作为图片查看，也可在 Draw.io 中重新编辑） |

## 使用方法

安装技能后，直接向 Claude 描述你的需求即可：

```
"创建一个用户登录流程图"
"绘制一个三层架构图"
"生成一个电商系统的微服务架构图"
```

## MCP 工具

本技能使用 \`@next-ai-drawio/mcp-server\` 提供的以下 MCP 工具：

| 工具 | 用途 |
|------|------|
| \`start_session\` | 打开浏览器实时预览 |
| \`display_diagram\` | 从 XML 创建新图表 |
| \`get_diagram\` | 获取当前图表 XML |
| \`edit_diagram\` | 通过 cell ID 修改图表 |
| \`export_diagram\` | 保存为 .drawio 文件 |

## SVG 转换

MCP server 仅支持导出 `.drawio` 文件。如需获得**可编辑的 SVG**（既可作为图片查看，又可在 Draw.io 中重新编辑），可使用内置的 Python 转换器：

```bash
python drawio/scripts/drawio_to_svg.py input.drawio output.svg
python drawio/scripts/drawio_to_svg.py input.drawio  # 输出为 input.svg
```

### 工作原理

1. 解析 `.drawio` 文件中的 `mxGraphModel` XML（支持编码和明文两种格式）
2. 将形状（矩形、椭圆、菱形、三角形等）渲染为 SVG 元素
3. 渲染连接线/箭头，支持路径点和箭头标记
4. 使用 `<foreignObject>` 渲染文本标签，实现精准的 HTML 文本显示
5. 将原始 `mxGraphModel` 嵌入 SVG 的 `content` 属性中，确保 SVG 可在 Draw.io 中重新编辑

### 支持的形状

| 形状 | 样式键 | SVG 元素 |
|------|--------|----------|
| 矩形 | (默认) | `<rect>` |
| 圆角矩形 | `rounded=1` | `<rect rx="...">` |
| 椭圆/圆形 | `ellipse` | `<ellipse>` |
| 菱形 | `rhombus` | `<polygon>` |
| 三角形 | `triangle` | `<polygon>` |
| 连接线 | `edge=1` | `<polyline>` + 箭头标记 |

## 项目结构

```
drawio-skills/
├── drawio/
│   ├── .mcp.json                 # MCP server 配置
│   ├── SKILL.md                  # 主技能文件
│   ├── scripts/
│   │   └── drawio_to_svg.py      # .drawio → 可编辑 SVG 转换器
│   ├── examples/
│   │   ├── auth_flow.drawio      # 认证流程示例
│   │   ├── auth_flow.svg         # 认证流程 SVG 预览
│   │   ├── user_cognitive_flow.drawio  # 用户认知流程示例
│   │   ├── user_cognitive_flow.svg     # 用户认知流程 SVG 预览
│   │   └── test_diagram.drawio   # 简单测试图表
│   └── references/
│       ├── xml-format.md         # Draw.io XML 格式参考
│       └── diagram-patterns.md   # 流程图与架构图模式
├── README.md                     # 英文文档
└── README_CN.md                  # 中文文档
```

## 环境要求

- **Python 3.7+** — 用于运行 `drawio_to_svg.py` 转换器（仅使用标准库，无需额外依赖）
- **Node.js** — 用于 MCP server（`@next-ai-drawio/mcp-server`）

## 致谢

- **MCP Server**: [next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io) by [@DayuanJiang](https://github.com/DayuanJiang)
- **Draw.io**: [diagrams.net](https://www.diagrams.net/)

## 许可证

本技能按原样提供。底层 MCP server 采用 [Apache-2.0](https://github.com/DayuanJiang/next-ai-draw-io/blob/main/LICENSE) 许可证。
