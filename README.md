# 内部分享工具包 · 三个 Skill

三个可复用的 Skill，覆盖一次公司内部分享的完整链路：**先想清楚系统怎么画，再想清楚怎么讲，最后装进能点击的演示页**。

> **非官方声明**：本仓库是个人整理的通用方法与素材，不是汇丰官方品牌规范，也不由汇丰发布。视觉部分只采用公开可查的视觉特征，不含 logo 图形、内部模板、真实客户或业务数据。依据与核验日期见 `visual-system.md`。

## 三个 Skill

| Skill | 管什么 | 输入 | 输出 |
|---|---|---|---|
| `solution-architecture-diagram` | 系统怎么搭 | 组件清单、调用关系、部署环境 | 单个自包含 SVG 架构 / 方案图 |
| `internal-share-narrative` | 讲什么、按什么顺序讲 | 主题、听众、时长、已有材料 | Markdown 叙事大纲 / 复盘故事 / 逐段讲稿 + 问答预案 |
| `fintech-presentation` | 长什么样、怎么点 | 已定稿的大纲或讲稿 | 单个自包含 HTML 交互演示页 |

三者共用 `visual-system.md` 的同一套视觉约定。推荐顺序：图 → 稿 → 页；也可以单独使用任意一个。

## 该找谁（避免误触发）

| 用户真实想要 | 用哪个 |
|---|---|
| 画架构图、系统上下文图、部署图、方案设计图 | `solution-architecture-diagram` |
| 理叙事、写分享大纲、写讲稿、做项目复盘 | `internal-share-narrative` |
| 做可点击切换的单文件演示页 | `fintech-presentation` |
| 把大量材料压成滚动长文 HTML + PDF | 已有的 `html-longread-story` |
| 把一批讲义 PDF 转成 PPTX + HTML deck | 已有的 `conference-pdf-to-deck` |
| 给已生成的 PPTX 做排版体检 | 已有的 `pptx-layout-qc` |

后三项是本机已有的 Skill，不在本仓库内，列出来只为划清触发边界。

## 目录结构

```text
skills/
├── README.md
├── visual-system.md                    视觉规范主副本
├── fintech-presentation/
│   ├── SKILL.md
│   ├── references/visual-system.md     规范副本，保证 Skill 独立可用
│   ├── references/layout-and-interaction.md
│   └── assets/deck-shell.html          可运行的单文件演示页骨架
├── internal-share-narrative/
│   ├── SKILL.md
│   └── references/narrative-modes.md
└── solution-architecture-diagram/
    ├── SKILL.md
    ├── references/visual-system.md     规范副本
    ├── references/diagram-rules.md
    ├── references/icon-catalog.md
    ├── assets/icon-library.svg         自绘图标集，可直接打开预览
    └── assets/diagram-shell.svg        分层架构图骨架
```

视觉规范在三个位置各存一份，内容完全一致；要改规范时，先改根目录的 `visual-system.md`，再把内容同步到两个 `references/visual-system.md`。这样每个 Skill 单独拷走也能用，不依赖跨目录引用。

## 安装

目标平台 WorkBuddy，安装位置为用户级 `~/.workbuddy/skills/`。把对应的 Skill 目录整个复制到该目录下即可，三个 Skill 互不依赖，可只装其中一个。安装由使用者自行确认后执行，本仓库不提供自动安装脚本。

## 平台与环境

| 项 | 说明 |
|---|---|
| 目标平台 | WorkBuddy 用户级 Skill |
| Python | 不需要；三个 Skill 均无脚本 |
| 外部依赖 | 无。无 MCP、CLI、API、账号、Token、网络调用 |
| 产物查看 | 任意现代浏览器打开 `.html` / `.svg` 即可，离线可用 |
| 已验证系统 | macOS（本次制作环境） |
| 未验证系统 | Windows、Linux 上的浏览器渲染细节；未做跨平台截图比对 |
| 字体 | 使用系统字体栈回退，不随包分发字体文件 |

## 降级与边界

| 情况 | 行为 |
|---|---|
| 无法生成 HTML 演示页 | 输出结构化 Markdown 分页大纲，标注「未渲染」 |
| 无法生成 SVG 图 | 输出组件清单 + 关系表 + 分层 Markdown 表，标注「未绘图」 |
| 输入材料不足 | 先列出缺失项并停下，不编造组件、数字或结论 |
| 材料含未公开数据 | 提示脱敏后再继续，不把原文写进产物 |
| 用户要 PPTX、滚动长文或批量 PDF 转换 | 转介到上表对应的已有 Skill，不硬做 |

## 使用入口

- 「画一下这个方案的系统架构图，把云、库和中间件标出来」
- 「帮我理一下这次内部分享怎么讲，45 分钟，听众是跨部门同事」
- 「把这版大纲做成能点击切换的演示页，用那套红黑白风格」

## 未覆盖范围

不生成 PPTX / Word / PDF 成品，不做 UI 界面设计稿，不绘制流程图与时序图，不接入真实数据源，不做自动发布或在线托管，不提供汇丰官方品牌资产。
