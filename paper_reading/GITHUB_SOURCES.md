# GitHub 方案调研

检索日期：2026-09-15

筛选标准：

- 与论文阅读、文献综合、文档解析或研究输出直接相关；
- 仓库定位清晰，不只是通用聊天机器人；
- 有公开文档、可复用设计或实际社区采用；
- 能补齐流程中的具体环节；
- 对机器人、具身智能和 Agent 的研究阅读有直接价值。

## 结论

没有单一仓库覆盖完整闭环。最合理的做法是组合：

```text
发现 -> 解析 -> 证据检索 -> 深读/批判 -> 多篇综合 -> 可视化输出 -> 知识沉淀
```

| 环节 | 参考仓库 | 借鉴点 | 局限 |
|---|---|---|---|
| 论文证据问答 | [Future-House/paper-qa](https://github.com/Future-House/paper-qa) | Agentic RAG、页码引用、证据重排、矛盾检测、本地论文库 | 需要模型/API；主要解决问答，不负责完整阅读输出 |
| 文档解析 | [opendatalab/MinerU](https://github.com/opendatalab/MinerU) | PDF/DOCX/PPTX 到 Markdown/JSON，公式、表格、图片和阅读顺序 | 复杂安装与模型资源；解析后仍需阅读和批判 |
| 新论文发现 | [tatsu-lab/gpt_paper_assistant](https://github.com/tatsu-lab/gpt_paper_assistant) | 每日 arXiv 扫描、作者匹配、相关性与新颖性评分、Slack/网页输出 | 主要处理标题和摘要，不深入正文 |
| 多问题调研 | [assafelovic/gpt-researcher](https://github.com/assafelovic/gpt-researcher) | planner/executor、多问题并行检索、报告生成、Web/本地文档 | 面向通用研究，论文级证据链需要额外约束 |
| 系统综述筛选 | [asreview/asreview](https://github.com/asreview/asreview) | 主动学习排序、人工反馈循环、大规模筛查 | 偏系统综述筛选，不生成深度理解材料 |
| 人类可读解释 | [dair-ai/ML-Papers-Explained](https://github.com/dair-ai/ML-Papers-Explained) | 按主题组织、一个核心概念配一段浓缩解释 | 不是自动化系统，覆盖范围有限 |
| 单篇问答历史方案 | [mukulpatnaik/researchgpt](https://github.com/mukulpatnaik/researchgpt) | 与单篇论文对话的早期产品范式 | 仓库已归档，作为历史参考 |
| 论文转海报 | [Paper2Poster/Paper2Poster](https://github.com/Paper2Poster/Paper2Poster) | 从论文自动生成多模态海报的完整问题定义和输出思路 | 生成质量、版权和图表真实性仍需人工检查 |
| 论文转 PPT | [icip-cas/PPTAgent](https://github.com/icip-cas/PPTAgent) | 参考模板、内容规划、生成后评估，而不是直接 text-to-slides | 需要针对论文论证重新设计故事线 |
| Agent 论文索引 | [zjunlp/LLMAgentPapers](https://github.com/zjunlp/LLMAgentPapers) | 按 personality、memory、planning、tool use、RL、multi-agent 分类 | 是阅读清单，不是阅读流水线 |
| Agent 论文索引 | [AGI-Edgerunners/LLM-Agents-Papers](https://github.com/AGI-Edgerunners/LLM-Agents-Papers) | 覆盖 survey、planning、memory、interaction、training、safety、benchmark | 更新与分类标准需要人工维护 |
| 具身智能阅读指南 | [TianxingChen/Embodied-AI-Guide](https://github.com/TianxingChen/Embodied-AI-Guide) | 领域知识地图、技术路线和资源组织 | 偏方向指南，不等于单篇精读 |
| LLM 资源总览 | [Hannibal046/Awesome-LLM](https://github.com/Hannibal046/Awesome-LLM) | 大模型方向总索引、资源发现 | 范围过宽，不能直接用于论文精读 |

## 对框架的直接启发

### 1. PaperQA2：证据优先

PaperQA2 的价值不是“生成摘要”，而是把检索、证据重排、引用回答和矛盾检测做成一个可追踪流程。本项目因此要求：

- 每条关键结论有 `location`；
- 单篇阅读先建 `evidence.jsonl`；
- 作者主张与独立判断分开；
- 多篇综合单独处理相互冲突的结论。

### 2. MinerU：结构恢复

机器人、具身智能和 Agent 论文常包含：

- 多栏排版；
- 系统框图；
- 算法框；
- 复杂公式；
- 真机实验图；
- 大表格。

仅依赖纯文本抽取容易丢失这些信息。因此解析层优先保留阅读顺序、图表标题、公式和页面定位。

### 3. GPT Paper Assistant：发现与筛选分离

它把每天的新论文和作者追踪变成可配置的相关性/新颖性评分。本项目借鉴为：

- 先做快速判断，不把“发现”误当成“理解”；
- 阅读前先写清关注标准和排除条件；
- 保存判断理由，避免重复读低价值论文。

### 4. GPT Researcher：先规划问题

多篇综述不是根据标题逐个总结，而是先形成一个研究问题集，再让每篇论文回答这些问题。本项目使用：

- 对比矩阵；
- 共识与分歧；
- 证据强弱；
- 研究空白和验证实验。

### 5. ASReview：筛查需要反馈循环

论文库规模上来后，需要：

- 排序候选论文；
- 记录纳入和排除原因；
- 用人工判断逐步校准筛选标准。

这适合后续扩展成课题级论文库，而不是只服务单篇阅读。

### 6. Agent 与具身智能索引：领域检查表

通用论文模板不足以判断 Agent/机器人工作。领域清单必须覆盖：

- 状态、观察、动作和环境；
- 规划、记忆、反馈、工具和多 Agent 协作；
- 本体、控制频率、仿真、真机、安全和恢复；
- 长程、分布外、真实世界和成本评测。

### 7. Paper2Poster / PPTAgent：输出层不是摘要换皮

论文转 PPT 或海报时，需要重新组织：

- 一页一个论点；
- 图表与解释相邻；
- 结果先给出判断，再补实验细节；
- 生成后做视觉、溢出、版权和引用检查。

## 不直接整套安装的原因

1. 这些项目覆盖的是不同层，强行选一个会产生明显短板。
2. 大模型、OCR、PDF 解析和 GPU 依赖会增加维护成本。
3. 用户当前最重要的是稳定输入输出约定和阅读质量，而不是先部署最大系统。
4. 后续可以按瓶颈接入：解析不准接 MinerU，库内问答接 PaperQA2，追踪新论文接 GPT Paper Assistant。

## 推荐接入顺序

1. 先使用当前工作区完成单篇深读、多篇矩阵和输出提纲。
2. 论文量超过约 50 篇后，接入 PaperQA2 做本地证据检索。
3. 复杂 PDF 经常出错时，接入 MinerU。
4. 需要每日跟踪时，接入或改造 GPT Paper Assistant。
5. 需要系统综述时，用 ASReview 做筛选层。
6. 需要批量生成 PPT/海报时，再评估 PPTAgent/Paper2Poster 类工具。

仓库热度和维护状态会变化，实际接入前应重新检查最近提交、issue、许可证和依赖。
