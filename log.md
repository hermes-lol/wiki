# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-04-21] create | Wiki initialized
- Domain: 经济社会运行的基本原理
- Structure created with SCHEMA.md, index.md, log.md
- Purpose: 记录从经济视角观察到的人类社会运行规律

## [2026-04-21] ingest | 2026年4月21日新闻要闻
- 新增源文件: raw/articles/2026-04-21-news-summary.md
- 更新实体: entities/霍尔木兹海峡.md (新增4月19日扣船事件、4月21日停火到期)
- 更新索引: index.md (添加今日要闻记录)

## [2026-04-21] ingest | RSS新闻批量入库
- 新增源文件:
  - raw/articles/2026-04-18_地缘冲突与大宗商品价格.md
  - raw/articles/2026-04-19_居民高储蓄资金滞留金融体系.md
  - raw/articles/2026-04-16_用幂律思维重构投资逻辑.md
- 新增实体: entities/能源冲击.md, entities/居民储蓄.md
- 新增概念: concepts/幂律思维.md, concepts/资金空转.md
- 更新索引: index.md (共新增6个页面)
- 主题: 地缘冲突、能源安全、居民储蓄、货币传导、投资理论

## [2026-04-22] import | RSS订阅源批量导入
- 从 supsub.net 导入 OPML 订阅源
- 新增9个订阅源，总计30个订阅源
- 同步获取418篇新文章
- 主要来源：财经、宏观分析、地缘政治、虚拟货币等

## [2026-04-22] sync | RSS新闻批量入库
- 关键入库文章：
  - 美伊和谈后金价反涨（黄金定价逻辑）
  - 伊朗战事相关原油期货疑似内幕交易遭调查
  - 战争与金融勾连：内幕交易操纵石油市场
  - 美伊局势波动对大类资产影响分析（天风证券）
- 新增源文件: raw/articles/2026-04-08-meiyi-he tan-hou-jin-jia-fan-zhang.md, raw/articles/2026-04-19-yilang-zhanshi-neimu-jiaoyi.md, raw/articles/2026-04-19-zhengzhi-yu-jinrong-neimu-jiaoyi.md
- 更新现有: concepts/黄金定价逻辑.md, concepts/战争金融化.md
- 主题: 美伊局势、黄金定价、战争金融化、内幕交易

## [2026-04-22] create | 场景培育概念页面
- 来源: 国办发〔2025〕37号《关于加快场景培育和开放推动新场景大规模应用的实施意见》
- 新增概念: concepts/场景培育.md
- 核心内容: 场景定义、政策背景、22类重点领域、实施机制、制度创新本质
- 更新索引: index.md (总计22个页面)

## [2026-04-23] ingest | 发展改革委场景工作
- 新增源文件:
  - raw/articles/2026-04-23-fagaiwei-changjing-xinxing-zhengce-gongju.md
  - raw/articles/2026-04-23-fagaiwei-biaozhi-changjing.md
- 更新概念: concepts/场景培育.md (新增发改委角色、核心表态、下一步部署)
- 来源: 国新办发布会、新华网、光明网
- 主题: 发改委场景政策定位、"两硬两软"验证体系、下一步工作部署

## [2026-04-22] update | 场景培育概念页面（地方实践）
- 来源: 《中国场景培育和开放研究报告（2025）》、合肥骆岗公园案例、上海国企开放案例
- 更新内容: 新增"地方实践"章节（七大路径）、典型案例（合肥骆岗公园）、总体趋势
- 文件: concepts/场景培育.md

## [2026-04-22] sync | RSS新闻批量入库（第4次）
- 新增源文件:
  - raw/articles/2026-04-22_美伊停火僵局.md
  - raw/articles/2026-04-22_美股泡沫与价值投资警示.md
  - raw/articles/2026-04-22_美股大涨油价破百.md
  - raw/articles/2026-04-22_国内外新闻汇总.md
- 关键主题: 美伊停火僵局、美股泡沫警示、油价突破百美元、国资投资风险
- 更新实体: entities/霍尔木兹海峡.md (新增4月22日事件)
- 新增交叉引用: [[战争金融化]]、[[幂律思维]]、[[黄金定价逻辑]]、[[资金空转]]
- 索引更新: 22→26页

## [2026-04-24] sync | RSS新闻批量入库（第5次）
- 新增源文件:
  - raw/articles/2026-04-24_白酒三强兴衰.md
  - raw/articles/2026-04-24_改变思维结构才能稳定盈利.md
  - raw/articles/2026-04-24_国内外重大新闻汇总.md
- 新增概念:
  - concepts/品牌溢价与战略定力.md
  - concepts/交易心理与概率思维.md
- 新增实体: entities/中国人口负增长.md
- 主题: 白酒产业竞争、投资心理学、人口负增长、制造业灵活用工
- 新增交叉引用: [[幂律思维]], [[资金空转]], [[黄金定价逻辑]], [[场景培育]], [[居民储蓄]]
- 索引更新: 26→32页

## [2026-04-25] sync | RSS新闻批量入库（第6次）
- 新增源文件:
  - raw/articles/2026-04-25-ai-重塑科技业就业结构.md
- 新增概念: concepts/ai就业重构.md
- 主题: AI就业重构、科技行业结构性裁员、7000亿美元AI投资与9.2万人裁员并行
- 新增交叉引用: [[幂律思维]], [[中国人口负增长]], [[交易心理与概率思维]], [[资金空转]]
- 洞察: AI驱动的就业变化具有"永久性转变"特征，岗位流失与新增岗位出现结构性错配
- 索引更新: 32→34页

## [2026-04-25] ingest | Hermes每日经济分析报告
- 新增源文件: raw/articles/经济动态-就业信号与高频数据-20260425.md
- 主题: 比亚迪招聘就业信号、高频数据软着陆、加密爆仓地缘传导、中美科技博弈、AI产业周期
- 核心洞察:
  - 比亚迪保产能式招聘与汽车消费-34.4%形成矛盾信号
  - 地产量升但政策克制，专项债发行进度28.7%偏慢
  - 狗狗币-39%反映恐慌性去杠杆，链条: 美伊恶化→霍尔木兹受阻→风险偏好收缩
  - 费城半导体本月+38.5%，AI算力新周期确认
  - 阿塞拜疆抛售22吨黄金，可能反映新兴市场外汇压力
- 索引更新: 34→35页

## [2026-04-25] organize | 清理hermes目录，归类到raw/articles
- 移动文件: hermes/七大大模型深度对比-2026年4月.md → raw/articles/七大大模型深度对比-AI大模型生态格局-202604.md
- 删除目录: hermes/（已空）
- 索引更新: 35→36页
- 统一命名规范: 主题名称-核心关键词-YYYYMMDD.md，统一存放 raw/articles/