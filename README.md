# Drone 3D Disassembly Skill

这是一个可复用的 DSH skill，用来制作「产品 3D 结构拆解 + 标注 + 详情 + 响应式」单页。主案例是一个通用的 9 模块四轴无人机，方法论适用于无人机、相机、机器人、发动机、家电等硬表面产品。

## 安装

把整个 `drone-3d-disassembly` 文件夹放到：

```text
<你的 DSH 工作区>/skills/drone-3d-disassembly/
```

确认文件存在：

```text
skills/drone-3d-disassembly/SKILL.md
```

然后**新开一个 DSH 会话**，使用：

```text
/drone-3d-disassembly
```

当前长会话可能不会热加载新 skill；新会话会在启动时读取 skills 目录。

## 内容

```text
SKILL.md
references/
  parts-schema.md      部件数据表结构
  product-case.md      9 个通用模块案例、视觉、参数占位
  code-patterns.md     场景、拆解、标注、选择、响应式、构建
  prompts.md           参考图、拆件、建模、Three.js、QA 提示词
  qa.md                验收清单
```

## 不包含

- 页面之外的交付物
- 排版、发布和媒体包装内容

这个 skill 只负责把 3D 交互页做出来。
