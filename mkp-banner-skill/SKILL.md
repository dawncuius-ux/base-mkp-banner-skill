---
name: mkp-banner
description: Base MKP Banner_V1。生成 MKP 产品/模版宣传 banner。固定三层结构：背景层、左侧 copy 层、右侧 UI 层。PNG 草稿阶段右侧 UI 必须用 image 2.0 生成；Figma 组件/本地资产只作为真实性、结构、padding、字阶和内容槽位约束。最终 Figma 稿按确认效果克隆源组件并重建为可编辑 UI。默认尺寸 720x240，不做 2x 放大。
---

# Base MKP Banner_V1

## 1. 目标

默认生成 `720×240` MKP banner，不做 2x 放大。只有用户明确要求时才使用 `1440×500`。

最终画面必须是三层：

- `00-background`：背景层。
- `20-copy.zh-CN`：左侧营销 copy 层，使用用户输入文案。
- `10-ui.zh-CN`：右侧产品 UI 层；PNG 草稿阶段由 image 2.0 生成，最终 Figma 稿重建为可编辑节点。

## 2. 输入与素材

用户通常提供：

- `title`
- `subtitle`
- `CTA`
- 产品 / 模版 brief
- 可选组件，例如 `grid`、`Pie`、`Progress`、`Metrics`、`Kanban`

缺少左侧 copy 时先询问；如果用户要求继续，可以临时拟定，并在 manifest 标记待确认。

主 Figma 文件：

- 文件名：`for AI banner`
- File key：`IFJ2aHOABZi2WsLJEEkm0A`
- URL：`https://www.figma.com/design/IFJ2aHOABZi2WsLJEEkm0A/for-AI-banner?node-id=0-1`

优先读取：

- `背景 & copy color` page：背景、copy 色值、卡片投影色。
- `组件` page：右侧产品 UI 组件来源。
- `模版` page：右侧 UI 组合布局参考。
- `案例` page：720×240 左侧 copy 字号、行高、位置。

本地 fallback：

- 背景：`assets/bg/BG01.png`、`BG02.png`、`BG03.png`、`BG04.png`。这些是从飞书 wiki 有机渐变库沉淀下来的固定 720×240 资产，直接调用，不重新生成。
- 背景 / copy / 投影配色：`assets/bg/copy-color-map.json`
- 组件资产：`assets/Cards/Large/*`、`assets/Cards/Small/*`

## 3. 标准流程

1. 读取用户 copy、brief、指定组件。
2. 从 `assets/bg/BG01-BG04.png` 选择固定有机渐变背景，并读取对应 copy 色值与卡片投影色温。
3. 从 brief 提取场景、实体、UI 证据和业务模版。
4. 为每个展示组件绑定 Figma 源组件或本地源资产；无法溯源则丢弃或澄清。
5. 读取并记录源组件截图 / node id，先写出组件契约：固定结构、允许改写槽位、禁止新增元素。
6. 参考 `模版` page 选择右侧 UI 构图。
7. 只改源组件已有内容槽位，保留产品结构、padding、字阶、图表位置和功能逻辑。
8. 用 image 2.0 生成 PNG 草稿；Figma 组件/本地资产仅作为参考图和约束。
9. 用户确认后写入 Figma 三层 frame，并通过克隆源组件把右侧 UI 重建为可编辑节点。
10. 输出 manifest，记录尺寸、copy、背景、组件来源、image 2.0、字体审计、fallback 和导出路径。

## 4. P0 红线

### 4.1 PNG 草稿必须使用 image 2.0

右侧 UI 图必须由 image 2.0 生成。禁止用 HTML/SVG/Sharp/Canvas 手工重建右侧 UI 并当作 PNG 草稿交付。

允许代码处理：

- 背景层与左侧 copy 合成。
- 裁切、压缩、manifest、QA 标注。
- 用户确认后，在 Figma 中重建可编辑 UI。

### 4.2 组件必须可溯源

右侧每个可见组件 / 卡片必须来自 Figma `组件` page，或来自本地 `assets/Cards` 中由 Figma 组件导出的源资产。

生成前必须完成：

- 找到组件的 Figma node id 或本地源资产路径。
- 查看源组件截图，而不是凭组件名主观想象。
- 写出该组件的固定结构、可改内容槽位、禁止新增元素。
- 把组件来源写入 manifest 的 `componentSources`。

如果没有完成以上任一项，禁止进入 image 2.0 生成。

禁止：

- 临时手画“看起来像产品 UI”的新卡片。
- 只借用表格语法、Base 风格、卡片风格就自行创造界面。
- 新增源组件没有的控件、标题 icon、右上角胶囊、filter button、工具栏、徽标。
- 把源组件结构换成另一种 UI 模式。

允许：

- 替换标题、字段、行数据、状态、数字等已有内容槽位。
- 在源组件语法允许时增删行列，并增大容器。
- 同一源组件复制多次，用不同内容表达多个业务模版。

### 4.3 源组件契约优先

组件名不是设计指令，源截图才是设计指令。image 2.0 prompt 必须描述“复用哪个源组件的结构”，而不是描述“画一个某某看板 / 表格 / 卡片”。

组件契约格式：

- `source`：组件名、node id 或本地资产路径。
- `fixedStructure`：必须保留的布局、控件、图表、线框、圆角、阴影、padding、字阶。
- `editableSlots`：可替换的标题、列名、行数据、状态、指标值等。
- `forbidden`：源组件中不存在的 icon、按钮、筛选器、标签、工具栏、交互逻辑。

违反任一 `forbidden` 项即视为 P0。

## 5. 视觉与版式

### 5.1 背景、copy 与投影

背景决定整张 banner 色温。左侧 copy 和组件卡片投影必须跟随所选背景的 canonical 配色。

默认背景使用本地固定资产：

- `BG01`：蓝色有机渐变。
- `BG02`：紫蓝有机渐变。
- `BG03`：绿色有机渐变。
- `BG04`：暖橙有机渐变。

这些背景已从飞书 wiki 有机渐变库下载并裁成 `720×240`，生成 banner 时直接铺满画布。禁止在常规生成流程里用 image 2.0、CSS、SVG 或临时渐变重新生成背景；只有需要新增背景资产时，才先下载/裁切/沉淀到 `assets/bg/` 和 `copy-color-map.json`，再调用。

配色优先级：

1. 读取 `assets/bg/copy-color-map.json` 中所选 BG 的 copy 色值、透明度和卡片投影。
2. 需要对齐 Figma 时，再读取 Figma `背景 & copy color` page 校验。
3. 只有新增背景才允许按色温兜底判断。

当前 canonical copy 配色：

- `BG01`：copy `#0A2957`，title 90%，subtitle 60%，CTA 100%。
- `BG02`：copy `#2C2E56`，title 90%，subtitle 60%，CTA 100%。
- `BG03`：copy `#0A5746`，title 90%，subtitle 60%，CTA 100%。
- `BG04`：copy `#57280A`，title 90%，subtitle 60%，CTA 100%。

卡片投影读取 `cardShadow.main`、`cardShadow.support`、`cardShadow.foreground`，色相跟随背景色温；右侧产品 UI 内部色彩保留源组件原始色。

### 5.2 布局密度

优先参考 Figma `模版` page。

常用布局：

- `主卡聚焦`：一个主组件 + 一个中/小支持组件。
- `堆叠主卡`：两到三张卡叠放，但只有一个视觉焦点。
- `平铺轻卡`：多张轻量卡片，适合低密度内容。
- `路径流转`：节点 / 小卡表达流程。

硬规则：

- 每张 banner 只能有一个视觉焦点。
- 右侧 UI 不能遮挡、触碰或干扰左侧 copy。
- `610px` 是默认 guardrail，不是绝对限制；只要不影响左侧 copy，右侧 UI 可以更宽。
- 默认一个主卡 + 一到两个支持卡。
- 支持卡可轻微叠放或旋转，但不能遮挡主组件必须保留的结构节奏。
- 旋转卡片时，先正面复刻内部结构，确认 padding、间距、文案容量正确，再整体旋转卡片。

## 6. 字体

左侧营销 copy：

- 中文默认使用 `Noto Sans SC`。
- Figma 输出前必须用 `figma.listAvailableFontsAsync()` 确认精确 `family/style`。
- 默认 `font-weight 600` 用 `Noto Sans SC Bold` 承接；当前 Figma 可用字重为 `Black`、`Bold`、`DemiLight`、`Light`、`Medium`、`Regular`、`Thin`，没有 `SemiBold`。
- 如果用户明确要求方正兰亭黑，只有 Figma 能成功 `loadFontAsync` 精确 family/style 时才可使用；不要用乱码 family 或未知中文字体冒充。
- 禁止静默 fallback 到 `Inter`。如果 `Noto Sans SC` 不可用，必须在回复和 manifest 中说明，并询问是否允许替代字体。

720×240 固定 copy 规格：

- copy frame：`x=40`、`y=50`、`width≈315`、`height=140`。
- Title：`font-size 30px`、`line-height 46px`、`font-weight 600`、`letter-spacing 0`。
- Subtitle：`font-size 14px`、`line-height 22px`、`font-weight 600`、`letter-spacing 0`；可按语义换行，不改写用户文案。
- CTA：`font-size 14px`、`line-height 22px`、`font-weight 600`，位于 copy frame 底部节奏，参考 `y=138`。
- 禁止为了避让 UI 或填满容器而缩放左侧 copy 字号；应调整右侧 UI 安全区、换行或构图。

右侧产品 UI：

- 不能继承左侧营销字体。
- 中文 UI 标题 / 表头 / 标签：优先保留源组件字体角色，通常为 `PingFang SC Medium/Regular`。
- 关键大数字：`Roboto Bold`。
- 辅助数字 / 金额 / 图例数值：`SF Pro Text Regular` 或源组件指定字体。
- 涨跌值：`SF Pro Text Medium` 或源组件指定字重。

Figma 输出前审计 `20-copy.zh-CN` 和 `10-ui.zh-CN` 中所有 text node 的 `fontName`、`fontSize`、`lineHeight`。

Figma 字体审计必须检查可加载性：

- 不能只看 `listAvailableFontsAsync()`；写入或改写文字前必须对目标 `family/style` 执行 `loadFontAsync()`。
- 如果源组件包含 `PingFang SC`，但当前 Figma MCP 不能加载对应字重，未改写的源节点可保留，改写过的中文 UI 文案使用 `Noto Sans SC Medium/Regular` 作为明确 fallback。
- fallback 必须写入 manifest 和回复；禁止静默改成 `Inter`。

## 7. Brief 解析

brief 只决定右侧 UI 的组件组合和内容改写，不主动改写用户输入的左侧 copy。

提取：

- 场景：销售、项目、制造、运营、AI、HR、财务、零售等。
- 实体：客户、任务、负责人、订单、项目、指标、门店、库存、会员等。
- UI 证据：表格、看板、KPI、Progress、Metrics、排行榜、流程等。
- 用户指定组件。

当用户写 `组件名（场景）`：

- 括号前是必须使用的组件族。
- 括号内是内容改写方向。
- 不要因为另一个组件也能表达就替换用户指定组件族。

## 8. 组件保真与容量

选中的源组件是产品界面契约，不是风格参考图。

改写前必须检查：

- 源 Figma node 或本地资产。
- 源宽高和最终缩放比例。
- padding、行距、字号、字重、行高、字间距。
- 图表 / icon 的位置和尺寸。
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

## 9. 组件特殊规则

- `Progress`：优先读取 Figma node `1:6172`；只改文案；圆弧保持正圆；百分比使用 `Roboto Bold`；缩小时整体缩放。
- `Metrics`：保留标题、大号指标值、灰色对比标签、绿色变化值、小图表；主数值使用 `Roboto Bold`；对比标签和涨跌值不能合并成一行。
- `todo`：保留标题、check icon 列、行距和底部 padding；不要把 placeholder/skeleton 挤成真实事项。
- `Kanban/Grid`：保留真实 outline、列结构、表格线；可改行列内容或新增同语法行列；不要增加 filter button、标题 icon、右侧胶囊控件。
- `S-Column`：只改标题、轴标签、柱值；保持 120×120 源比例、柱宽、柱圆角、底部标签节奏；缩放时整体缩放。
- `M-Kanban`：必须保留源组件的大白卡、顶部标题、头像/负责人胶囊行、三列线框卡片结构；没有标题 icon、右上按钮、筛选器或工具栏；可把销售签约内容改为客服工单 / FAQ / 售后事项，但不能增加源组件没有的产品逻辑。
- `S-NPS`：保留标题、半圆仪表盘、中心分数、刻度与色段；不要改成饼图、环图或折线图；缩放时整体缩放，禁止拉伸圆弧。

## 10. Figma 输出与 QA

Figma 输出：

- Frame 默认 `720×240`，不做 2x 放大。
- 顶层只能有三层：`00-background`、`20-copy.zh-CN`、`10-ui.zh-CN`；三层 frame 均为 `720×240`。
- 背景铺满画布。
- copy 层和 UI 层独立可选中。
- 右侧 UI 按确认后的 image 2.0 PNG 效果重建为可编辑节点，但必须优先克隆 Figma `组件` page 的源组件 / 源 frame。
- 克隆后只编辑源组件已有文本、数据、状态等槽位；不要在 Figma 终稿里手画一个相似组件替代源组件。
- 如果支持卡需要旋转，先在正面状态完成内部文本、padding、间距、容量检查，再整体旋转 clone。
- 默认保留嵌套 instance；不要为了改文案盲目 detach 嵌套 instance。只有确实需要改内部结构时才 detach，并先确认目标节点仍存在。
- 允许保留隐藏的 image 2.0 参考底图，但最终交付的右侧 UI 不能只是一张扁平截图。
- 隐藏 manifest / reference image 必须放进 `00-background` 内或放在 banner frame 外，不能成为第四个顶层 layer。

manifest 至少记录：

- `imageGeneration.engine`
- `imageGeneration.referenceComponents`
- `componentSources`
- `backgroundTone`
- `copyPrimary`
- `cardShadow`
- `fontAudit`
- `fallback`
- `outputPath`

交付前确认：

- 三层结构正确。
- 左侧 copy 与用户输入一致，默认 `Noto Sans SC Bold`。
- copy 色、卡片投影与背景色温一致。
- PNG 草稿右侧 UI 使用 image 2.0。
- 每个 UI 卡片都有来源记录。
- 没有源组件不存在的控件。
- 没有文字溢出、碰撞、贴边、裁切。
- chip / pill / badge 能容纳内容。
- 圆弧、图表、头像不变形。
- 没有 Figma 选框、水印、设备框、乱码、混合语言或脏透明边。
