---
name: mkp-banner
description: 生成 MKP 产品/模版宣传 banner。固定三层结构：背景层、左侧 copy 层、右侧可编辑产品 UI 层。适用于 MKP banner、模版推广 banner、产品功能 banner、PNG 草稿和最终 Figma 分层稿；默认尺寸 720x240，不做 2x 放大。
---

# MKP Banner Skill

## 1. 默认目标

生成 `720×240` MKP banner，默认不做 2x 放大。只有用户明确要求时才使用旧尺寸 `1440×500`。

输出必须分为三层：

- `00-background`：背景层，来自 Figma `背景 & copy color` page 或本地背景资产。
- `20-copy.zh-CN`：左侧营销 copy 层，使用用户输入文案。
- `10-ui.zh-CN`：右侧产品 UI 层，基于 Figma `组件` page，可编辑。

PNG 阶段可以先出草稿；最终 Figma 稿必须保持三层结构，右侧 UI 尽量用可编辑节点，不能把整组 UI 压成一张截图交付。

## 2. 输入

用户应提供：

- `title`
- `subtitle`
- `CTA`
- 产品 / 模版 brief

可选输入：

- 指定背景
- 指定布局
- 指定组件，例如 `CRM表`、`Progress`、`Metrics`、`Kanban（项目管理）`

如果左侧 copy 缺失，先询问；如果用户要求直接继续，可以临时拟定，并在 manifest 标记为待确认。

## 3. Figma 与本地来源

主文件：

- 文件名：`for AI banner`
- File key：`IFJ2aHOABZi2WsLJEEkm0A`
- URL：`https://www.figma.com/design/IFJ2aHOABZi2WsLJEEkm0A/for-AI-banner?node-id=0-1`

优先读取：

- `背景 & copy color` page：背景、左侧 copy 色值、组件卡片投影色。
- `组件` page：右侧产品 UI 组件来源。
- `模版` page：右侧 UI 组合布局参考。

本地 fallback：

- 背景：`assets/bg/BG01.png`、`BG02.png`、`BG03.png`、`BG04.png`
- 背景 / copy / 投影配色：`assets/bg/copy-color-map.json`
- 组件资产：`assets/Cards/Large/*`、`assets/Cards/Small/*`

如果 Figma MCP 不可用，可以使用本地资产出 PNG 草稿，但 manifest 必须标记 fallback 和具体 asset path。

## 4. 生成流程

1. 读取用户 copy 和 brief。
2. 从 Figma `背景 & copy color` 或本地 `copy-color-map.json` 选择背景、copy 色值和卡片投影参数。
3. 从 brief 提取场景、实体、用户指定组件和 UI 证据。
4. 将每个业务模版映射到具体 Figma 源组件或本地源资产。
5. 丢弃或澄清无法溯源的业务模版。
6. 参考 `模版` page 选择右侧 UI 构图。
7. 只改源组件已有内容槽位，保留产品结构、padding、字阶、图表位置和功能逻辑。
8. 输出 PNG 草稿；用户确认后写入 Figma 三层 frame。
9. 输出 manifest，记录尺寸、copy、背景、布局、组件来源、copy/投影配色来源、字体审计、fallback 和导出路径。

## 5. P0 红线：组件必须可溯源

右侧 UI 中每一个可见组件 / 卡片都必须基于 Figma `组件` page 中的源组件，或基于本地 `assets/Cards` 中由 Figma 组件导出的源资产。

禁止：

- 临时手画一个看起来像产品 UI 的新卡片。
- 只借用“表格语法”“卡片风格”“Base 风格”就自行创造组件。
- 用户给出业务模版名时，直接把业务模版画成新界面。
- manifest 只写“使用 Grid/table grammar”，但没有具体 Figma node 或本地资产来源。

允许：

- 复制 / 读取 Figma 源组件后，只改标题、字段、行数据、状态、数字等内容槽位。
- 在源组件已有产品逻辑内增删行列、增宽 / 增高容器。
- 同一个源组件可复制多次，用不同内容表示多个业务模版。
- Figma MCP 不可用时，使用本地 `assets/Cards/...` 导出图作为源。

manifest 中每个展示组件必须记录：

- `businessTemplate`
- `sourceComponent`
- `sourceNodeId` 或 `sourceAsset`
- `allowedChanges`

缺少来源记录时，结果视为无效草稿，不能交付。

## 6. 背景、Copy 与投影配色

背景层决定整张 banner 的色温。左侧营销 copy 和右侧组件卡片投影都必须跟随所选背景的 canonical 配色。

配色优先级：

1. Figma 可用时，读取 `背景 & copy color` page。
2. Figma 不可用时，读取 `assets/bg/copy-color-map.json`。
3. 只有没有 canonical 记录的新背景，才使用色温判断兜底。

当前 canonical copy 配色：

- `BG01`：copy `#0A2957`，title 90%，subtitle 60%，CTA 100%。
- `BG02`：copy `#2C2E56`，title 90%，subtitle 60%，CTA 100%。
- `BG03`：copy `#0A5746`，title 90%，subtitle 60%，CTA 100%。
- `BG04`：copy `#57280A`，title 90%，subtitle 60%，CTA 100%。

组件卡片投影：

- 主卡、支持卡、前景小卡分别读取 `copy-color-map.json` 的 `cardShadow.main`、`cardShadow.support`、`cardShadow.foreground`。
- 投影色相必须跟随所选背景的 `copyColor / cardShadow`。
- 允许调整 opacity、blur、offset 来适配层级。
- 禁止暖色背景使用冷黑灰、蓝灰、绿灰投影。
- 禁止冷色背景使用暖棕橙投影。

右侧产品 UI 内部色彩保留源组件原始色，不强行随背景改色。

manifest 必须记录：

- `backgroundTone`
- `copyPrimary`
- `copyOpacity`
- `cardShadow`
- `copyColorSource`

## 7. 版式与密度

优先参考 Figma `模版` page。

常用布局：

- `主卡聚焦`：一个主组件 + 一个中/小支持组件。
- `堆叠主卡`：两到三张卡叠放，但只能有一个焦点。
- `平铺轻卡`：多张轻量卡片，适合低密度内容。
- `路径流转`：节点 / 小卡表达流程。

硬规则：

- 每张 banner 只能有一个视觉焦点。
- 右侧 UI 不能遮挡、触碰或干扰左侧 copy。
- `610px` 是右侧 UI 默认 guardrail，不是绝对限制；只要不影响左侧 copy，右侧 UI 可以更宽。
- 默认一个主卡 + 一到两个支持卡。
- 支持卡可轻微叠放或旋转，但不能遮挡主组件必须保留的结构节奏。
- 避免多个高密度大卡同时完整出现。

## 8. 字体规则

左侧营销 copy：

- 中文使用本地品牌字体：`/Users/bytedance/Desktop/飞书 brand /方正兰亭黑 Pro GB18030/`
- 字号、字重、行高、字间距参考 Figma 相同角色。
- 参考：`font-size 30px`、`line-height 46px`、`font-weight 600`、`letter-spacing 0`。

右侧产品 UI：

- 不能继承左侧品牌字体。
- 必须保留源组件文字角色。
- 中文 UI 标题 / 表头 / 标签：`PingFang SC Medium/Regular`。
- 关键大数字：`Roboto Bold`。
- 辅助数字 / 金额 / 图例数值：`SF Pro Text Regular` 或源组件指定字体。
- 涨跌值：`SF Pro Text Medium` 或源组件指定字重。

Figma 输出前必须审计 `10-ui.zh-CN` 中所有 text node 的 `fontName`、`fontSize`、`lineHeight`。如果 Figma MCP 环境无法加载 `PingFang SC` 或方正字体，不要静默 fallback 到 Inter；必须在 manifest 和回复中说明。

## 9. Brief 解析

brief 只决定右侧 UI 的组件组合和内容改写，不主动改写用户输入的左侧 copy。

解析时提取：

- 场景：销售、项目、制造、运营、AI、HR、财务等。
- 实体：客户、任务、负责人、订单、项目、指标、候选人等。
- UI 证据：表格、看板、KPI、Progress、Metrics、排行榜、流程等。
- 用户指定组件。

当用户写 `组件名（场景）`：

- 括号前是必须使用的组件族。
- 括号内是内容改写方向。
- 不要因为另一个组件也能表达就替换用户指定组件族。

## 10. 组件保真

选中的源组件是产品界面契约，不是风格参考图。

允许：

- 替换已有数据 / 文本槽位。
- 根据 brief 改写业务内容。
- 在源组件语法允许时新增行 / 列，并增大容器。
- 调整组件整体缩放、位置、叠放关系。

禁止：

- 新增源组件不存在的控件、图标、筛选器、按钮、徽标、标题 icon、右上角胶囊、工具栏。
- 把源组件结构替换成另一种 UI 模式。
- 把内容标签改造成产品控件。
- 为了塞内容而压缩 padding、行高、字号、字间距。

改写组件前必须检查：

- 源 Figma node 或本地资产。
- 源宽高和最终缩放比例。
- 内容 padding。
- 文本 baseline、字号、字重、行高、字间距。
- 图表 / icon 的位置和尺寸。
- 内容容量。
- 容器、pill、chip、badge、label、数据槽位是否能装下文字。

P1 错误：

- 文字溢出、裁切、贴边、被遮挡。
- 相邻内容组碰撞。
- chip / pill / badge 无法容纳文字。
- 最后一行离底部过近，底部 padding 明显小于源组件。
- 支持卡遮挡主组件必须保留的结构节奏。
- 圆弧、图表、头像被拉伸变形。

P1 修复顺序：

1. 增大容器或整体缩放组件。
2. 调整组件位置 / 叠放，避开主内容。
3. 缩短内容文案并保留语义。
4. 减少可见内容数量。

不要通过缩小源组件字号、压缩行高或减少源 padding 解决。

## 11. 组件特殊规则

`Progress`：

- 优先读取 Figma node，例如 `1:6172`。
- 只改文案，不改图形位置、大小、间距。
- 圆弧保持正圆，不拉伸。
- 百分比使用 `Roboto Bold`。
- 如果需要更小，缩放整个组件；不要重排内部结构。

`Metrics`：

- 保留标题、大号指标值、灰色对比标签、绿色变化值、小图表。
- 主数值使用 `Roboto Bold`。
- 涨跌值使用 `SF Pro Text Medium`。
- 对比标签和涨跌值不能合并成一行。

`todo`：

- 保留标题、check icon 列、行距和底部 padding。
- 源组件是两条真实事项 + 一个 placeholder/skeleton 时，不要把 placeholder 挤成第三条真实事项。

`Kanban/Grid`：

- 内部真实 outline、列结构、表格线必须保留。
- 可以改行列内容，可以新增同语法行 / 列并增宽容器。
- 不要增加源组件没有的 filter button、标题 icon、右侧胶囊控件。

`S-Column`：

- 使用 Figma `S-Column` 源组件时，只改标题、轴标签、柱值。
- 保持 120×120 源比例、柱宽、柱圆角、底部标签节奏。
- 缩放时整体缩放，不单独拉伸柱形或容器。

## 12. Figma 输出要求

Frame：

- 默认 `720×240`。
- 不做 2x 放大。
- 三层 frame 均为 `720×240`。
- 背景铺满画布。
- copy 层和 UI 层独立可选中。

右侧 UI：

- 可编辑优先。
- 从源组件复制，或按源节点坐标重建。
- 不允许把整组 UI 直接作为扁平截图交付，除非用户只要 PNG。

## 13. QA Checklist

交付前必须确认：

- 尺寸符合用户要求，默认 `720×240`。
- 三层结构正确：`00-background`、`20-copy.zh-CN`、`10-ui.zh-CN`。
- 左侧 copy 与用户输入一致。
- 左侧 copy 使用所选背景的 canonical 色值和 opacity。
- 组件卡片投影使用所选背景的 canonical `cardShadow`。
- 右侧 UI 不遮挡左侧 copy。
- 只有一个明确视觉焦点。
- 右侧每一个 UI 卡片都有 `sourceComponent` + `sourceNodeId/sourceAsset`。
- 没有新增源组件不存在的控件 / 图标 / 按钮 / 筛选器。
- padding、行距、字阶、圆角、阴影、图表位置保留源组件节奏。
- 所有文字 fit，无溢出、裁切、贴边、碰撞。
- chip / pill / badge 能容纳内容。
- 支持卡不破坏主组件信息阅读。
- 右侧 UI 字体角色符合源组件；字体不可加载时明确说明。
- 没有 Figma 选框、水印、设备框、乱码、混合语言或脏透明边。
- manifest 已记录组件来源、copy/投影配色来源、fallback、字体审计和输出路径。
