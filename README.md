# Raxul A

Minecraft 基岩版模型与贴图工具。Windows 单文件程序，无运行时依赖。

本仓库只发布可执行文件，不包含源代码。

---

## 功能

**建模**

- 显式盒：每个盒子的位置、尺寸、旋转、pivot 都由 JSON 描述。
- 旋转：绕 pivot 按 Z → Y → X 施加，与 Bedrock cube rotation 一致。
- 粗体素：椭球 / 胶囊 / 长方体按固定格子转成盒子，只沿单轴合并。
- 一个部件可以包含任意多个盒子。

**贴图**

- 贴图尺寸与区域都由 spec 划分，工具不自动排 UV。
- 六个面可以分别指定区域（例如正面画脸、其余面用头发色）。
- 绘制：整区填色、整个部件填色、单像素落笔。
- 细节：1px 倒角与两档抖动，可对选中区域叠加。

**输出**

- Bedrock 几何 `format_version 1.12.0`。
- PNG 贴图。
- 体检报告：部件数、盒子数、面数、贴图区域数、包围盒。

**预览**

- 等轴平涂，按贴图取样，最近邻。
- 多视角拼图：`--views 0,90,180`。

---

## 使用

```
Raxul-A build <spec.json> [-o DIR] [--size N] [--views 0,90,180]
                          [--keep-texture] [--no-preview]
Raxul-A paint <spec.json> [-o DIR] [--region NAME=#RRGGBB] [--part NAME=#RRGGBB]
                          [--px X,Y=#RRGGBB] [--bevel] [--grain] [--size N]
Raxul-A info
```

`build` 生成几何、贴图、预览、报告；`--keep-texture` 保留已经画好的贴图。

`paint` 在已有贴图上继续画；输出目录没有贴图时，先按 spec 铺一层底。

---

## spec 格式

```json
{
  "name": "demo",
  "identifier": "geometry.raxul_a.demo",
  "texture": [16, 16],
  "regions": {
    "skin":  [0, 0, 8, 8],
    "cloth": [8, 0, 8, 8]
  },
  "palette": {
    "skin":  "#e8c8b0",
    "cloth": "#4a6fa5"
  },
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
| `texture` | 贴图宽高，像素 |
| `regions` | 贴图上的矩形，`[x, y, w, h]` |
| `palette` | `build` 时把区域铺成这些颜色，可省略 |
| `parts[].pivot` | 骨骼 pivot，也是部件内盒子的默认旋转中心 |
| `boxes[].center` / `size` | 盒子中心与尺寸，y 向上，1 单位 = 1 像素 |
| `boxes[].rot` / `pivot` | 可选，绕 pivot 旋转，单位是度 |
| `boxes[].uv` | 区域名，或 `{ "south": "face", "all": "hair" }` 按面指定 |
| `boxes[].voxel` | 用粗体素代替盒子，见下 |

粗体素：

```json
{ "voxel": { "shape": "ellipsoid", "center": [0, 39, -3],
             "radius": [5.7, 4.7, 3.4], "cell": 2.0, "uv": "hair" } }
```

`shape` 支持 `ellipsoid`、`capsule`（`a` / `b` / `radius`）、`box`（`min` / `max`）。

---

## 注意事项

- 仅支持 Windows 10 及以上，64 位。程序自带运行库，不需要额外安装。
- 只接受 JSON spec。不做自动形状分解，不自动排 UV，不做面剔除。
- 输出几何的坐标与尺寸以像素为单位，贴图区域需要自己规划，超出区域不会自动修正。
- 预览是平涂等轴图，固定三档面亮度，无光照、AO、描边、抗锯齿；它只用来看形，不代表游戏内最终效果。
- 贴图细节只有 1px 倒角和两档抖动，不区分材质。
- 不包含动画、骨骼语义、着色器、粒子、JSON UI、结构生成、音频等功能。
- 不提供源代码，不提供插件或脚本接口。

---

## 许可

本程序仅授权使用。

- 允许：个人、学习、评估用途的免费使用。
- 不允许：任何商业用途。
- 不允许：反向工程、反编译、反汇编、解密或试图还原内部实现。
- 不允许：修改、破解、绕过或关闭程序中的任何限制。
- 不允许：再分发原版或修改版（分享仓库链接可以）。

完整条款见 [LICENSE](LICENSE)，以英文原文为准。
