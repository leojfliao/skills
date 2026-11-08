# 版式库与交互规则

骨架 `assets/deck-shell.html` 已实现全部 CSS 变量与交互状态机。本文件说明该选哪种版式、每段结构怎么写、哪些坑会毁掉一页。

## 1. 版式对照

| 版式 | 适用 | 一页上限 |
|---|---|---|
| `cover` 封面 | 开场，标题 + 一句主张 | 1 个标题、1 行副标题 |
| `thirds` 三栏要点 | 并列的三个能力、三个阶段、三类风险 | 3 张卡，每卡 2 行 |
| `metric` 关键数字 | 用一个数字撑起一页的结论 | 1 个主数字 + 3 条辅助指标 |
| `split` 左右对比 | 现状 vs 目标、方案 A vs B、前后对照 | 每侧 4 条 |
| `timeline` 时间线 | 阶段推进、里程碑、演进路径 | 4 个节点 |
| `figure` 图示页 | 嵌入架构图、方案图、流程图 | 1 张图 + 1 行图注 |
| `table` 数据表 | 精确数值需要逐项比对时 | 6 行 × 4 列 |
| `closing` 收尾 | 一句话结论、下一步、Q&A | 1 句 + 3 条行动项 |

不要连续两页用同一版式。连续三页都是要点列表时，把中间一页改成数字页或对比页。

## 2. 结构片段

所有片段放在 `<section class="slide" data-title="页面标题">` 内。`data-title` 必须非空，目录与总览从它生成。

### 封面

```html
<section class="slide cover" data-title="封面">
  <p class="eyebrow">内部分享 · 20XX 年 X 月</p>
  <h1 class="title">一句话说清这次讲什么</h1>
  <p class="sub">补充一句为什么值得听</p>
  <p class="note">分享人 / 团队 · 时长 45 分钟</p>
</section>
```

### 三栏要点

```html
<section class="slide" data-title="三个能力">
  <h2 class="title">这版方案解决三件事</h2>
  <div class="grid-3">
    <div class="card"><span class="idx num">01</span><h3>能力一</h3><p>一句说明，不超过两行。</p></div>
    <div class="card"><span class="idx num">02</span><h3>能力二</h3><p>一句说明，不超过两行。</p></div>
    <div class="card"><span class="idx num">03</span><h3>能力三</h3><p>一句说明，不超过两行。</p></div>
  </div>
</section>
```

### 关键数字

```html
<section class="slide" data-title="效果">
  <h2 class="title">改造后最直观的变化</h2>
  <div class="metric">
    <span class="value num">128<span class="unit">ms</span></span>
    <span class="label">端到端处理时延</span>
    <span class="delta">较改造前下降 <b class="num">37%</b></span>
  </div>
  <div class="metric-row">
    <div><span class="num">3</span><span class="label">接入系统数</span></div>
    <div><span class="num">99.9%</span><span class="label">可用性</span></div>
    <div><span class="num">0</span><span class="label">人工干预次数</span></div>
  </div>
</section>
```

数字没有确切来源时，不用示意数填充，把该位置留空并标注待补。

### 左右对比

```html
<section class="slide" data-title="前后对比">
  <h2 class="title">改动前后</h2>
  <div class="split">
    <div class="col"><h3>现状</h3><ul><li>要点</li><li>要点</li></ul></div>
    <div class="col accent"><h3>目标</h3><ul><li>要点</li><li>要点</li></ul></div>
  </div>
</section>
```

只有右侧 `.accent` 用红色细边或红色小标题，不做整块红底。

### 时间线

```html
<section class="slide" data-title="推进节奏">
  <h2 class="title">分四步落地</h2>
  <div class="timeline">
    <div class="tl-item"><span class="when num">Q1</span><h3>梳理</h3><p>一句说明</p></div>
    <div class="tl-item"><span class="when num">Q2</span><h3>试点</h3><p>一句说明</p></div>
    <div class="tl-item"><span class="when num">Q3</span><h3>推广</h3><p>一句说明</p></div>
    <div class="tl-item"><span class="when num">Q4</span><h3>收口</h3><p>一句说明</p></div>
  </div>
</section>
```

### 图示页

```html
<section class="slide" data-title="系统架构">
  <h2 class="title">整体分层</h2>
  <figure class="figure">
    <svg viewBox="0 0 1200 480" role="img" aria-label="系统分层示意">…内联路径…</svg>
    <figcaption>图 1 · 系统分层与调用方向（示例）</figcaption>
  </figure>
</section>
```

SVG 必须整段内联，带 `viewBox` 和 `role="img"` / `aria-label`。不写 `<img src>`、不写 `<use href="外部文件#...">`：浏览器在 `file://` 下常会拦掉跨文件引用。

### 数据表

```html
<section class="slide" data-title="指标对照">
  <h2 class="title">三项指标</h2>
  <table class="data">
    <thead><tr><th>指标</th><th>改造前</th><th>改造后</th><th>说明</th></tr></thead>
    <tbody><tr><td>时延</td><td class="num">203 ms</td><td class="num">128 ms</td><td>峰值口径</td></tr></tbody>
  </table>
</section>
```

数值列加 `class="num"` 右对齐；文字列左对齐。

### 收尾

```html
<section class="slide closing" data-title="小结">
  <p class="kicker">一句话收在这里</p>
  <ul class="actions"><li>下一步一</li><li>下一步二</li><li>下一步三</li></ul>
  <p class="note">Q&amp;A</p>
</section>
```

## 3. 骨架交互（不要重写）

`deck-shell.html` 的脚本维护一个当前页索引，负责：切换 `.active`、更新进度条宽度、更新页码、同步目录高亮、写 URL hash、`Esc` 总览网格、点击跳转、打印展开。

增删页面只做两件事：增删 `<section class="slide">`，以及保证每页的 `data-title` 非空。目录、总览、页码、进度条全部自动重算，不要手动改脚本里的页数常量，也不要另写一套翻页逻辑。

删除页面后刷新一次，确认末页进度条到 100%、目录项数与页数一致。

## 4. 常见坑

| 坑 | 表现 | 处理 |
|---|---|---|
| 跨文件 SVG 引用 | 本地双击打开时图标空白 | 把用到的 `<symbol>` 定义复制进本页 `<defs>`，或整图内联 |
| 打印时页面透明 | 导出 PDF 只剩第一页或全白 | 打印样式里对 `.slide` 强制 `opacity:1`、`position:relative`、清掉 `transform` |
| 表格溢出 | 右侧列被裁掉 | 减列或拆页，不靠缩小字号解决 |
| 数字跳动 | 翻页时数值左右晃 | 数字统一走等宽字体与 `tabular-nums` |
| 红色过量 | 页面像促销海报 | 只保留一个红色焦点，其余退到黑白灰 |
| 深色页文字发灰 | 反白页对比不足 | 深色底用纯白文字，注释不低于 `#D7D8D6` |
| 焦点丢失 | 键盘跳转后按方向键无反应 | 跳转后把焦点交回页面容器，不留在按钮上 |
| 中文断行难看 | 标题末字单独成行 | 标题控制在 18 字内，或用 `<br>` 手动断在语义处 |
| 图表无数据 | 画了轴没画点，仍占整页 | 没有确切数值就换成数据表或删掉该页 |
| 目录不同步 | 总览里出现旧标题 | 标题只写在 `data-title`，不在脚本里另存一份 |

## 5. 验收

- [ ] 每种版式至少被正确使用一次，或说明为何不需要
- [ ] 没有连续两页同版式
- [ ] 每页一个主观点，支撑项不超过 5 个
- [ ] 所有内联 SVG 有 `viewBox` 与无障碍标注
- [ ] 打印预览逐页检查无截断
- [ ] 键盘全流程走通，焦点始终可见
