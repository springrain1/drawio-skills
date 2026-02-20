# Draw.io Skill for Claude Code

[Documentation](https://bahayonghang.github.io/drawio-skills/) | [中文文档](https://bahayonghang.github.io/drawio-skills/zh/)

A Claude Code skill that enables AI-powered diagram creation and editing with real-time browser preview.

## Relationship with Upstream Project

This skill is built on top of **[next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io)** by [@DayuanJiang](https://github.com/DayuanJiang).

| Project | Purpose |
|---------|---------|
| [next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io) | MCP Server that provides draw.io diagram tools |
| **This Project (drawio-skills)** | Claude Code skill that wraps the MCP server with workflow guidance, XML format references, and diagram patterns |

### What This Skill Adds

- **Workflow guidance** for creating and editing diagrams
- **XML format reference** with style properties and common shapes
- **Diagram patterns** for flowcharts and architecture diagrams
- **Automatic MCP configuration** via `.mcp.json`
- **SVG converter** — a Python script (`drawio_to_svg.py`) that converts `.drawio` files to editable SVG
- **Example diagrams** — ready-to-use `.drawio` and `.svg` samples

## Installation

### Install from GitHub

```bash
# Clone this repository to your Claude Code skills directory
git clone https://github.com/bahayonghang/drawio-skills.git ~/.claude/skills/drawio
```

The skill will be available automatically in Claude Code.

## Features

| Feature | Description |
|---------|-------------|
| **Natural Language → Diagram** | Describe what you need, get a professional diagram |
| **Real-time Preview** | See changes instantly in your browser |
| **Flowcharts** | Process flows, decision trees, workflows |
| **Architecture Diagrams** | System architecture, microservices, deployment |
| **Edit Existing Diagrams** | Modify diagrams using ID-based operations |
| **Export** | Save diagrams as `.drawio` files |
| **SVG Conversion** | Convert `.drawio` to editable SVG (viewable as image, re-editable in Draw.io) |

## Usage

Once the skill is installed, simply ask Claude to create a diagram:

```
"Create a flowchart for user login process"
"Draw a three-tier architecture diagram"
"Generate a microservices architecture for an e-commerce system"
```

## MCP Tools

This skill uses the following MCP tools from \`@next-ai-drawio/mcp-server\`:

| Tool | Purpose |
|------|---------|
| \`start_session\` | Open browser with real-time preview |
| \`display_diagram\` | Create new diagram from XML |
| \`get_diagram\` | Retrieve current diagram XML |
| \`edit_diagram\` | Modify diagram by cell ID |
| \`export_diagram\` | Save as .drawio file |

## SVG Conversion

The MCP server only exports `.drawio` files. To get an **editable SVG** (viewable as image AND re-editable in Draw.io), use the included Python converter:

```bash
python drawio/scripts/drawio_to_svg.py input.drawio output.svg
python drawio/scripts/drawio_to_svg.py input.drawio  # outputs to input.svg
```

### How It Works

1. Parses `mxGraphModel` XML from the `.drawio` file (supports both encoded and plain XML)
2. Renders shapes (rectangles, ellipses, diamonds, triangles, etc.) to SVG elements
3. Renders connections/edges with arrow markers and waypoint support
4. Renders text labels using `<foreignObject>` for accurate HTML text rendering
5. Embeds the original `mxGraphModel` in the SVG's `content` attribute — making the SVG re-editable in Draw.io

### Supported Shapes

| Shape | Style Key | SVG Element |
|-------|-----------|-------------|
| Rectangle | (default) | `<rect>` |
| Rounded Rectangle | `rounded=1` | `<rect rx="...">` |
| Ellipse / Circle | `ellipse` | `<ellipse>` |
| Diamond | `rhombus` | `<polygon>` |
| Triangle | `triangle` | `<polygon>` |
| Connections | `edge=1` | `<polyline>` with arrow markers |

## Project Structure

```
drawio-skills/
├── drawio/
│   ├── .mcp.json                 # MCP server configuration
│   ├── SKILL.md                  # Main skill file
│   ├── scripts/
│   │   └── drawio_to_svg.py      # .drawio → editable SVG converter
│   ├── examples/
│   │   ├── auth_flow.drawio      # Authentication flow example
│   │   ├── auth_flow.svg         # SVG preview of auth flow
│   │   ├── user_cognitive_flow.drawio  # User cognitive flow example
│   │   ├── user_cognitive_flow.svg     # SVG preview of cognitive flow
│   │   └── test_diagram.drawio   # Simple test diagram
│   └── references/
│       ├── xml-format.md         # Draw.io XML format reference
│       └── diagram-patterns.md   # Flowchart & architecture patterns
├── README.md                     # English documentation
└── README_CN.md                  # Chinese documentation
```

## Requirements

- **Python 3.7+** — for the `drawio_to_svg.py` converter (uses only the standard library, no extra dependencies)
- **Node.js** — for the MCP server (`@next-ai-drawio/mcp-server`)

## Credits

- **MCP Server**: [next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io) by [@DayuanJiang](https://github.com/DayuanJiang)
- **Draw.io**: [diagrams.net](https://www.diagrams.net/)

## License

This skill is provided as-is. The underlying MCP server is licensed under [Apache-2.0](https://github.com/DayuanJiang/next-ai-draw-io/blob/main/LICENSE).
