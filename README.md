# Raxul A

Minecraft 基岩版模型与贴图工具，供 AI Agent 与脚本调用。

单文件可执行程序，无运行时依赖。可执行文件见 [Releases](https://github.com/wachg-studio/Raxul-A/releases)。

---

## MCP

Raxul-A 内置 stdio MCP 服务端，可直接接入 Agent 客户端。

```json
{
  "mcpServers": {
    "raxul-a": {
      "command": "C:\\Tools\\Raxul-A\\Raxul-A.exe",
      "args": ["mcp"]
    }
  }
}
```

| 工具 | 说明 |
| --- | --- |
| `build` | 从 spec 生成几何、贴图、预览与构建报告 |
| `paint` | 填区域、填部件、点像素，可叠加倒角与抖动 |
| `info` | 返回能力说明 |

参数：`spec`（必填）为 spec 文件路径；`out` 为输出目录；`size`、`views`、`preview` 控制预览。

---

## 功能

- **建模**：显式盒，位置、尺寸、旋转、pivot 由 JSON 描述；单个部件支持多个盒子。
- **体素**：椭球、胶囊、长方体按固定格生成盒子。
- **贴图**：按区域绘制，六个面可分别指定区域；支持整区填色与单像素编辑。
- **细节**：1px 倒角与抖动，可叠加到指定区域。
- **预览**：等轴平涂，多视角拼图。
- **输出**：Bedrock 几何（`format_version 1.12.0`）、PNG 贴图、构建报告。

---

## 命令行

```bash
Raxul-A build model.json
Raxul-A paint model.json --part head=#ff7a59
Raxul-A mcp
```

```
Raxul-A build <spec.json> [-o DIR] [--size N] [--views 0,90,180]
                          [--keep-texture] [--no-preview]

Raxul-A paint <spec.json> [-o DIR] [--region NAME=#RRGGBB] [--part NAME=#RRGGBB]
                          [--px X,Y=#RRGGBB] [--bevel] [--grain] [--size N]

Raxul-A mcp
Raxul-A info
```

产物写入 `out/<name>/`。

---

## Spec

```json
{
  "name": "demo",
  "texture": [16, 16],
  "regions": { "skin": [0, 0, 8, 8], "cloth": [8, 0, 8, 8] },
  "palette": { "skin": "#e8c8b0", "cloth": "#4a6fa5" },
  "parts": [
    {
      "name": "body",
      "pivot": [0, 12, 0],
      "boxes": [
        { "center": [0, 12, 0], "size": [8, 12, 4], "uv": "cloth" }
      ]
    }
  ]
}
```

| 字段 | 说明 |
| --- | --- |
| `texture` | 贴图宽高（像素） |
| `regions` | 贴图矩形，`[x, y, w, h]` |
| `palette` | 可选，`build` 时区域的基础色 |
| `parts[].pivot` | 骨骼 pivot，盒子的默认旋转中心 |
| `boxes[].center` / `size` | 盒子中心与尺寸，y 向上，1 单位 = 1 像素 |
| `boxes[].rot` / `pivot` | 可选，绕 pivot 旋转，单位：度 |
| `boxes[].uv` | 区域名，或按面指定，如 `{ "south": "face", "all": "hair" }` |

体素用 `{"voxel": {...}}` 替代 `center/size`，`shape` 支持 `ellipsoid`、`capsule`、`box`。

---

## 环境

Windows 10 及以上，64 位。无需安装运行库。

---

## 共创

- Claude
- ZCode

---

## 许可

个人、学习与评估用途免费使用；禁止商业使用、反向工程、修改与再分发。

完整条款见 [LICENSE](LICENSE)。
