# Generic Quadcopter Case

A compact 9-part exploded-view example. Use it as a starting point, not as a claim of official engineering data.

## 9 modules

| id | name | en | role | label side | explode direction |
| --- | --- | --- | --- | --- | --- |
| `airframe` | 机身承力壳 | AIRFRAME SHELL | 主承力结构与机身外壳 | right | up |
| `battery` | 智能飞行电池 | INTELLIGENT FLIGHT BATTERY | 动力与续航 | left | up/back |
| `gimbal` | 云台相机 | GIMBAL CAMERA | 三轴稳定与成像 | right | down/front |
| `arm` | 折叠机臂 | FOLDING ARMS | 四轴展开与折叠 | right | out/up |
| `motor` | 无刷动力电机 | BRUSHLESS MOTOR | 驱动桨叶 | right | out/up |
| `propeller` | 低噪快拆桨 | LOW-NOISE PROPELLER | 产生升力 | left | out/up |
| `vision` | 全向视觉系统 | OMNI VISION | 避障与环境感知 | left | down/front |
| `board` | 飞控主板 | FLIGHT CONTROL BOARD | 飞控、电源与传感 | left | up/front |
| `antenna` | 图传天线 | TRANSMISSION ANTENNA | 图传与遥控链路 | right | up/back |

## Visual notes

- Body: blue-gray metal with dark seams, a dark top battery plate, and a subtle cyan status ring.
- Arms: four rounded arms with visible hinge joints; fold lines should read at a glance.
- Motors: dark cylinders with a bright metal outer ring and a small top cap.
- Props: two-blade, slightly swept; use a thin extruded shape with a subtle twist.
- Gimbal: a compact box with a large front lens ring; hang it below the nose.
- Vision: small dark camera dots at the front and bottom; keep them subtle.
- Board: a green/teal PCB plate with a few dark chips; visible only in the exploded view.
- Antenna: two small rear fins/antennae; keep them from merging with the arms.

## Camera

- 3/4 front view; FOV 38° desktop.
- Model centered; leave ~25% screen width for side labels.
- Key light upper-left, rim light behind-right, low fill.
- Dark floor grid, soft contact shadow, light fog.
- Base camera distance should not fill the frame; labels need air.

## Public specs

Do not hardcode a specific brand's numbers. Fill from the product's official documentation:

```text
重量 [weight]
续航 [flight time]
速度 [max speed]
图传 [transmission range]
传感器 [sensor]
等效焦距 [equivalent focal length]
光圈 [aperture]
视场角 [field of view]
视频 [video modes]
```

Every value must keep its source. If a number cannot be cited, label it `模型设计值` or `概念`.

## Disclaimer

This is a non-official structural study. The outer form is inspired by the product; the internal arrangement, exploded order, and some proportions are artistic choices. Do not present it as an official repair diagram or engineering dataset.
