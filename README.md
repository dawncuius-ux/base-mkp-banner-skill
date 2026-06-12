# Base MKP Banner_V1

用于生成 MKP 产品 / 模版宣传 banner 的 Codex skill。

默认能力：

- 生成 `720×240` banner，不做 2x 放大。
- 固定三层结构：背景层、左侧 copy 层、右侧产品 UI 层。
- 背景直接调用本地 `有机渐变/` 图片资产，不重新生成；左侧 copy 色、组件卡片投影色跟随对应背景色温的 canonical 配色。
- PNG 草稿阶段右侧 UI 必须使用 image 2.0 生成。
- Figma `组件` page 或本地 Cards 源资产用于约束产品真实性、结构、padding、字阶和内容槽位，不能临时手画产品 UI。
- 支持先输出 PNG 草稿，确认后写入 Figma 三层可编辑稿。

## 安装

把 `mkp-banner-skill` 目录复制到 Codex skills 目录：

```bash
cp -R mkp-banner-skill ~/.codex/skills/
```

重启 Codex 后即可使用。

## 使用

参考 [PROMPT_TEMPLATE.md](./PROMPT_TEMPLATE.md)。

新手使用介绍：  
[Base MKP Banner_V1 使用介绍](./docs/base-mkp-banner-v1-usage.md)

## 关键资产

- Skill：`mkp-banner-skill/SKILL.md`（skill name 仍为 `mkp-banner`，可继续用 `$mkp-banner` 调用）
- 背景主库：`mkp-banner-skill/有机渐变/`
- 背景兼容 fallback：`mkp-banner-skill/assets/bg/`
- 背景 / copy / 投影配色：`mkp-banner-skill/assets/bg/copy-color-map.json`
- 产品 UI 组件图：`mkp-banner-skill/assets/Cards/`

## Figma 依赖

主 Figma 文件：

https://www.figma.com/design/IFJ2aHOABZi2WsLJEEkm0A/for-AI-banner

优先读取：

- `背景 & copy color`
- `组件`
- `模版`
