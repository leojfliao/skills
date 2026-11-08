# 自绘图标清单

图标本体在 `assets/icon-library.svg`，共 47 个，全部为自绘几何图形，统一 24×24 网格、线宽 1.5、`fill="none"`、描边继承 `currentColor`。直接用浏览器打开该文件可以看到带名称的完整预览网格。

## 使用规则

1. 按下面的语义表挑图标，不凭形状相似随手选。
2. 把对应的 `<symbol>` 整段复制到输出图的 `<defs>` 里，只复制用到的。
3. 同文件内引用：`<use href="#icon-database" x="..." y="..." width="24" height="24"/>`。
4. 不写跨文件 `href`（本地打开会被浏览器拦掉），不单独改某个图标的颜色、线宽或填充；只有焦点组件允许把整组描边换成红色。
5. 图标尺寸统一 24×24，与文字基线对齐，不放大缩小。
6. 语义缺失时用最接近的通用图标 + 文字标签说明，不临时画风格不一致的新图标。

## 用法示例

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 720" role="img" aria-label="示例分层架构">
  <defs>
    <symbol id="icon-database" viewBox="0 0 24 24">
      <ellipse cx="12" cy="5.5" rx="8" ry="3"/>
      <path d="M4 5.5v13c0 1.7 3.6 3 8 3s8-1.3 8-3v-13M4 12c0 1.7 3.6 3 8 3s8-1.3 8-3"/>
    </symbol>
  </defs>
  <g fill="none" stroke="#333333" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
    <use href="#icon-database" x="100" y="500" width="24" height="24"/>
  </g>
</svg>
```

父 `<g>` 上的描边属性会被 `<use>` 继承，symbol 内部路径不要单独设 `stroke`。

## 清单

### 云与区域

| id | 语义 | 何时用 |
|---|---|---|
| `icon-cloud` | 云 / 云服务商（抽象） | 不特指某厂商的云环境 |
| `icon-region` | 区域 | 地理或逻辑大区 |
| `icon-vpc` | 虚拟专有网络 | 云内隔离网络边界 |
| `icon-onprem` | 本地机房 | 自建数据中心 |
| `icon-edge` | 边缘节点 | CDN 边缘、就近接入点 |
| `icon-availability` | 可用区 / 多实例分布 | 同区域多可用区部署 |

### 计算

| id | 语义 | 何时用 |
|---|---|---|
| `icon-compute` | 计算实例 / 虚机 | 通用算力 |
| `icon-container` | 容器 | 容器化运行单元 |
| `icon-serverless` | 函数计算 | 事件触发的短任务 |
| `icon-kubernetes` | 编排平台 | 容器编排与调度 |
| `icon-batch` | 批作业 | 定时或离线批处理 |

### 接入

| id | 语义 | 何时用 |
|---|---|---|
| `icon-gateway` | 网关 | 统一入口、路由 |
| `icon-loadbalancer` | 负载均衡 | 流量分发 |
| `icon-api` | API 接口 | 对外契约层 |
| `icon-firewall` | 防火墙 | 网络访问控制 |
| `icon-dns` | 域名解析 | 服务发现与解析 |

### 服务

| id | 语义 | 何时用 |
|---|---|---|
| `icon-service` | 通用服务 | 不想强调实现形态的服务 |
| `icon-microservice` | 微服务集群 | 多个小服务互调 |
| `icon-agent` | 代理 / 智能体 | 旁路采集、代理执行 |
| `icon-scheduler` | 调度器 | 定时触发与编排 |

### 数据

| id | 语义 | 何时用 |
|---|---|---|
| `icon-database` | 通用数据库 | 不区分类型的库 |
| `icon-sql` | 关系型数据库 | 结构化查询场景 |
| `icon-nosql` | 非关系型数据库 | 文档 / KV / 图 |
| `icon-warehouse` | 数据仓库 | 分析型存储 |
| `icon-lake` | 数据湖 | 原始多模态沉淀 |
| `icon-objectstore` | 对象存储 | 文件、归档、备份 |

### 中间件

| id | 语义 | 何时用 |
|---|---|---|
| `icon-cache` | 缓存 | 热点加速 |
| `icon-queue` | 消息队列 | 异步解耦、削峰 |
| `icon-topic` | 发布订阅主题 | 一对多广播 |
| `icon-event` | 事件 | 事件驱动中的事件体 |
| `icon-stream` | 流处理 | 连续数据处理链路 |

### 安全

| id | 语义 | 何时用 |
|---|---|---|
| `icon-iam` | 身份与权限 | 认证、授权、角色 |
| `icon-key` | 密钥 | 凭据与密钥管理 |
| `icon-shield` | 安全防护 | 通用安全边界 |
| `icon-lock` | 加密 / 锁定 | 传输或存储加密 |

### 可观测

| id | 语义 | 何时用 |
|---|---|---|
| `icon-monitor` | 监控 | 指标看板 |
| `icon-log` | 日志 | 日志采集与检索 |
| `icon-trace` | 链路追踪 | 分布式调用链 |
| `icon-alert` | 告警 | 阈值触发与通知 |

### 端与外部

| id | 语义 | 何时用 |
|---|---|---|
| `icon-user` | 用户 | 人作为参与方 |
| `icon-web` | Web 端 | 浏览器客户端 |
| `icon-mobile` | 移动端 | App 客户端 |
| `icon-partner` | 合作方系统 | 外部机构对接 |
| `icon-external` | 外部依赖 | 不属于本系统的任意外部方 |

### 通用

| id | 语义 | 何时用 |
|---|---|---|
| `icon-network` | 网络互联 | 抽象网络拓扑 |
| `icon-boundary` | 边界框 | 信任域、分区 |
| `icon-note` | 说明 / 待确认 | 未定组件的占位框 |

## 语义缺失时

清单里没有对应语义时，按这个顺序处理：

1. 用 `icon-service`（软件组件）或 `icon-external`（外部方）+ 准确的文字标签。
2. 需要标"还没确认"的组件，用 `icon-note` 并放进「待确认」虚线框，写在图例里。
3. 仍然说不清时，宁可只画一个纯文字框，也不临时画一个与整套线宽不一致的新图标。

## 禁止

- 不引入某云厂商的官方图标包（打包分发有授权问题），用抽象图标 + 文字标签代替。
- 不把图标转成位图、不嵌入 `<image>`。
- 不给单个图标填充颜色、渐变或投影。
- 不修改 `viewBox`、线宽或圆角去「凑好看」。

## 验收

- [ ] 用到的每个 `id` 都在清单里
- [ ] 每个 `<use>` 的 `href` 都能在本文 `<defs>` 找到同名 `id`
- [ ] 图例列出了本次用到的全部图标含义
- [ ] 无跨文件引用、无外部图片、无位图
- [ ] 所有图标尺寸一致，描边继承统一
