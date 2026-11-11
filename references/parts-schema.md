# Parts Schema

Use one JSON object per product. The page reads this table for the list, labels, detail panel, and explosion.

```json
{
  "product": {
    "name": "示例四轴无人机",
    "en": "QUADCOPTER AIRFRAME",
    "disclaimer": "非官方结构研究；内部结构为艺术化示意"
  },
  "parts": [
    {
      "id": "gimbal",
      "num": "03",
      "name": "云台相机",
      "en": "GIMBAL CAMERA",
      "category": "camera",
      "description": "三轴机械云台与相机模组一体化设计，负责稳定成像与视频录制。",
      "specs": [
        { "k": "传感器", "v": "[传感器规格]", "source": "concept" },
        { "k": "等效焦距", "v": "[等效焦距]", "source": "concept" },
        { "k": "光圈", "v": "[光圈范围]", "source": "concept" },
        { "k": "视场角", "v": "[视场角]", "source": "concept" },
        { "k": "结构角色", "v": "三轴机械云台", "source": "model" }
      ],
      "related": ["airframe", "board", "vision"],
      "anchor3d": [0, -0.35, -1.1],
      "explode3d": [0, -1.4, -0.4],
      "labelSide": "right"
    }
  ]
}
```

## Field rules

| Field | Type | Rule |
| --- | --- | --- |
| `id` | string | stable kebab-case; never rename during a build |
| `num` | string | display order, `01`–`09` |
| `name` | string | Chinese display name |
| `en` | string | uppercase English label |
| `category` | string | `frame`, `power`, `camera`, `arm`, `propulsion`, `sensor`, `avionics`, `comms` |
| `description` | string | 60–100 Chinese characters, role + relationship |
| `specs` | array | 4–5 rows; `source` is `official`, `model`, or `concept` |
| `related` | array | 2–3 part IDs |
| `anchor3d` | number[3] | local/world anchor for the 3D label |
| `explode3d` | number[3] | direction and distance for separation |
| `labelSide` | string | optional `left` / `right`; otherwise choose by projected X |

## Source discipline

- `official`: public manufacturer specification; keep a URL or document reference in the README.
- `model`: a dimension or count chosen for this 3D model; mark “模型设计值”.
- `concept`: inferred or artistic; mark “示意 / 概念”.

Do not use `official` for a number you cannot cite.
