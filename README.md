# Pyreact UI Builder Skill

这是一个面向任意 agent 的 Pyreact UI 编写 skill 仓库，用于指导 agents 在网易我的世界基岩版 ModSDK 环境中使用 Pyreact 编写业务 UI。完整规则、组件规范和示例写法以 [`SKILL.md`](SKILL.md) 为准。

## 适用场景

加载本 skill 的典型场景：

- 新建或修改基于 Pyreact 的页面、弹窗、HUD、列表、表单、详情面板。
- 正确使用 `Panel`、`Image`、`Label`、`Item`、`PaperDoll`、`Button`、`Input`、`Scroll`。
- 使用 `FilledButton`、`ImageButton`、`Animated`、hooks、`key`、`ref`、`style`。
- 将 Pyreact 页面挂载到 NetEase `ScreenNode` / JsonUI root。

本 skill 面向“用 Pyreact 写业务 UI”。如果任务要求修改 `pyreact/`、`PyreactRuntimeScript/` 或 `JsonUI/PyreactBase.json` 的框架内部实现，应按框架开发任务处理，而不是只依赖本 skill。

## 仓库结构

`SKILL.md` 位于仓库根目录，所有供 skill 引用的资源目录都与它同级：

```text
SKILL.md
README.md
JsonUI/
pyreact/
PyreactRuntimeScript/
PyreactExampleScript/
```

资源路径均按仓库根目录相对路径引用。

## 资源说明

- `SKILL.md`：skill 入口，包含触发条件、Pyreact UI 编写规范、组件 API、挂载流程和交付自检。
- `pyreact/`：Pyreact 公开 API 和框架源码参考，优先查看 `pyreact/__init__.py`。
- `PyreactRuntimeScript/`：运行时系统与原生 UI 提交逻辑参考。
- `JsonUI/`：Pyreact 所需 JsonUI 基础模板和示例模板。
- `PyreactExampleScript/`：业务页面示例，可作为组件组合、布局和动画参考。

## 使用方式

在支持自定义 skill / knowledge / instruction 资源的 agent 环境中，将本仓库作为 Pyreact UI skill 加载。Agent 接到 Pyreact UI 任务时应读取 `SKILL.md`，按其中的 props / `style` 分工、Python2 语法、稳定 `key`、标准挂载流程和网易 ModSDK 约束执行。

### 让 Agent 自我安装

将下面内容复制给支持文件操作的 agent，让它把本仓库安装到自己的可加载 skill / knowledge 目录：

```text
请把这个仓库安装为可加载的 Pyreact UI skill：

1. 克隆仓库到你的 agent 可加载的 skills、knowledge 或 instructions 目录，目录名建议保持为 pyreact-ui-builder。
2. 确认仓库根目录存在 SKILL.md。
3. 确认 JsonUI/、pyreact/、PyreactRuntimeScript/、PyreactExampleScript/ 与 SKILL.md 同级。
4. 后续遇到 Pyreact UI、网易 Minecraft Bedrock ModSDK UI、JsonUI 容器、ScreenNode 挂载、props/style 分工、Animated/hooks/key/ref 相关任务时，优先加载 pyreact-ui-builder skill。
```

最小挂载流程为：

1. 在 `UiInitFinished` 中 `RegisterUI(...)`。
2. 调用 `PushScreen(...)` 显示界面。
3. 在 `ScreenNode.Create()` 内调用 `render_app(...)`。
4. 在 `ScreenNode.Destroy()` 内通过 runtime `UnmountApp(...)` 清理。

## 核心原则

- 自定义业务组件必须使用 `@Component`。
- 布局、尺寸、定位、透明度写入 `Style(...)`；组件专属能力写 props。
- 动态列表、筛选、排序、tab 内容和动画列表必须使用稳定业务 `key`。
- 保持 Python2 写法，不使用 f-string、type hints 或 Python3-only 语法。
- 涉及网易 API / JsonUI 原生能力但文档未确认时，不要猜测实现。

## 许可证

Pyreact 原项目版权归原作者 EnderWolf006 所有：Copyright 2026 EnderWolf006。原项目地址为 <https://github.com/EnderWolf006/pyreact>。

本仓库保留 Pyreact 相关资源的 Apache License 2.0 许可证与归属要求，详见 [`LICENSE`](LICENSE) 和 [`NOTICE`](NOTICE)。在 Minecraft Bedrock Edition 项目中使用 Pyreact 时，需要按 `NOTICE` 在服务器/存档加载界面和切换维度界面展示 Pyreact、作者 EnderWolf006 与原 GitHub 仓库地址。
