# 资源站记录格式

站点记录由实际搜索生成，不随交付包预置。文件位置及创建、增量更新规则见正文第 7 节；本文件定义数据格式。

根对象包含 `schema_version: 1` 和站点列表 `sites`。每个站点使用以下字段；未知值用 `unknown`、null 或空列表，不推断为免费、免登录或无密码。

| 字段 | 类型 | 内容 |
| --- | --- | --- |
| `domain` | 字符串 | 小写实际主机名，作为去重键，不盲目合并子域名 |
| `name` | 字符串 | 页面显示名称 |
| `home_url` | 字符串 | 实际访问的站点首页 |
| `search_url` | 字符串或 null | 可复用站内搜索入口 |
| `cost` | 字符串 | `free`、`mixed`、`paid`、`unknown` |
| `login` | 字符串 | `not_required`、`partial`、`required`、`unknown` |
| `cost_scope`、`login_scope` | 可选字符串 | `site`（有明确全站依据）、`observed_resources`（仅已查资源）、`unknown`；旧记录缺失时视为范围未知 |
| `access_notes` | 字符串列表 | 搜索、查看地址、下载各环节的费用、登录、积分或会员条件 |
| `resource_types` | 字符串列表 | 实际观察的平台、语言及资源类型 |
| `download_methods` | 字符串列表 | 文件直链、目录网盘、中国发行网盘、其他或发行方未知网盘 |
| `password_rules` | 对象列表 | 通用密码经验：`rule`、`source_url`、`checked_at`；资源专属密码留在当次结果 |
| `search_tips` | 字符串列表 | 已证实有效的名称变体或技巧 |
| `related_sites` | 字符串列表 | 相关友链 URL，不代表已核实可用 |
| `status` | 字符串 | `accessible`、`login_required`、`verification_required`、`restricted`、`unreachable`、`unknown` |
| `last_checked_at` | 字符串 | 带时区的 ISO 8601 实际核查时间 |
| `structure` | 可选对象 | 操作类型 `pattern`、技术线索 `technology`、`checked_at`、`evidence_urls` |
| `search_profile` | 可选对象 | 实际入口、方法、字段、范围、分页及界面步骤 |
| `evidence_urls` | 字符串列表 | 支持分类与判断的实际页面 |
| `search_history` | 对象列表 | 每次目标搜索记录，格式如下 |

## 搜索历史

| 字段 | 类型 | 内容 |
| --- | --- | --- |
| `checked_at` | 字符串 | 带时区的实际核查时间 |
| `recorded_at` | 可选字符串 | 整理或修正时间，不用于刷新核查时间 |
| `target` | 字符串 | 作品及平台、语言、版本要求 |
| `queries` | 字符串列表 | 实际关键词，在 `notes` 区分站点发现、引擎检索与站内查询 |
| `outcome` | 字符串 | `matched`、`no_match`、`blocked`、`inconclusive` |
| `resource_pages` | 字符串列表 | 匹配的来源或下载网页 |
| `notes` | 字符串 | 查询反馈、版本差异、获取条件、核查结果和阻碍 |
| `record_path` | 可选字符串 | 相对 skill 根目录的详细执行记录路径 |
| `link_status` | 可选字符串 | `obtained`、`not_obtained`，只表示是否取得地址 |
| `blocker` | 可选字符串 | 未取得地址的原因 |
| `search_status` | 可选字符串 | `complete`、`retry_needed`，独立于地址与访问状态 |
| `completion_reason` | 可选字符串 | 本轮完成的实际依据 |
| `evidence_source` | 可选字符串 | `user_feedback`、`live_check`、`historical_check` |

## 记录口径

- `matched` 表示页面匹配，不保证链接有效；`no_match` 仅指已执行查询未命中；未搜索目标或响应无法解释时用 `inconclusive`；登录、验证与访问限制用 `blocked`，不等同链接失效。
- 收费与登录分别判断，并注明适用范围；网盘自身的限制不改变来源站分类。每条密码规则须有出处、时间和范围。
- 详细记录区分来源网页、独立下载页、`download_entries`（按钮或取链入口）、`api_evidence_urls`（接口证据）与 `links`（实际地址）。返回错误的接口不能记为已取得地址。
- 多版本、镜像、分卷及本体／补丁按关系分组，每条线路保留密码、备注与核查状态；如需统计，站点数、独立资源数和线路数分开。直链提取、网盘基础核查及结果组合按正文执行。
- 同主机更新原记录；确认迁移时保留域名关联依据。查询与证据按 URL、关键词及时间去重，保留不同目标、失败经历及旧结论，修正时说明原因。
- 长期记录只留可复用经验、稳定页面和简要历史，完整当次结果另存；不堆积重复整页或工具日志。
