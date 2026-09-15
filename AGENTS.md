# Codex 论文阅读项目规范

## 目标

当用户在本项目提供论文链接、arXiv ID、DOI、PDF、论文题目或一组论文时，默认执行本文件定义的论文阅读流程，并产出可核验、可复用、图文友好的结构化理解材料。

## 触发条件

以下表达都视为论文阅读任务：

- “读一下这篇论文”
- “帮我分析这个 arXiv/DOI 链接”
- “总结这些论文并做综述”
- “输出 PPT/PDF/思维导图/海报”
- 直接发送 PDF、论文首页截图或论文题目

## 必须遵守

1. 先证据，后结论。论文观点、作者结论、实验结果和本项目的独立判断必须明确区分。
2. 关键结论尽量带定位：页码、章节、公式、图或表，例如 `p.5, Sec.3.2, Fig.4`。
3. 不编造实验设置、数据集、指标、参数量、硬件、基线或引用。无法确认时写“论文未明确”或“解析不确定”。
4. 数值结论必须回查原表或正文。比较提升时同时给出绝对值和相对值，避免只写“显著提升”。
5. 图表不能只复述标题。每张入选图要说明“它在证明什么、证据强度如何、是否支持正文结论”。
6. 阅读机器人、具身智能或 Agent 论文时，必须使用 `paper_reading/AGENT_ROBOTICS_TAXONOMY.md` 中对应清单。
7. 多篇论文任务必须做横向矩阵，不能把每篇摘要简单拼接。
8. 输出前执行 `paper_reading/WORKFLOW.md` 中的质量门槛。

## 默认流程

1. 读取 `config/reading_profile.json`。
2. 为论文分配稳定 `paper_id`，建议格式为 `firstauthor-year-shorttitle`，例如 `yao-2022-react`。
3. 原始文件放入 `papers/library/<paper_id>/source/`。
4. 提取文本、图表、公式和元数据，放入 `papers/library/<paper_id>/extracted/`。
5. 建立证据记录 `papers/library/<paper_id>/evidence.jsonl`。
6. 在 `outputs/<paper_id>/` 生成默认深读包。
7. 如用户指定 PDF、PPT、海报或思维导图，使用对应技能完成渲染并做视觉/内容复核。

## 默认深读包

- `00_one_page.md`：一页速览，包含问题、核心思想、方法、关键结果、局限与可复用点。
- `01_deep_note.md`：完整深读笔记，使用 `paper_reading/templates/deep_note.md`。
- `papers/library/<paper_id>/evidence.jsonl`：关键结论到原文位置的证据链。
- `mindmap.mmd`：Mermaid 思维导图源文件。
- `deck_storyboard.md`：8-12 页 PPT/图文 PDF 的故事线、图表计划和讲稿要点。

## 输出模式

- 用户未指定格式：生成默认深读包，不自动渲染重量级 PDF/PPT。
- “快速判断”：只生成 `00_one_page.md`，但保留证据定位。
- “深读”：生成默认深读包。
- “多篇综述”：生成 `multi_paper_matrix.md`、`synthesis.md`、`research_gaps.md` 和 `reading_order.md`。
- “输出 PDF”：在深读包基础上生成图文 PDF，并渲染成图片逐页检查。
- “输出 PPT”：在 `deck_storyboard.md` 基础上生成 PPT，并逐页检查溢出、重叠、图表和引用。
- “输出思维导图”：优先输出 Markmap/Mermaid；需要可交付文件时再生成 SVG/PDF/HTML。

## 工具路由

- PDF 阅读、拆分、渲染和图像检查：优先使用 `pdf` 技能。
- DOCX/Word 输出：使用 `documents` 技能。
- PPT 或演示文稿：使用 `presentations` 技能。
- 需要原创插画或不可从论文复用的视觉资产：使用 `imagegen` 技能。
- Markdown 思维导图：优先生成 Mermaid/Markmap 兼容文本。
- 本地大规模论文库问答：可接入 PaperQA2。
- 复杂 PDF 版面、公式、表格和图片提取：可接入 MinerU、Docling 或同类解析器。

## 交互约定

用户可以只说：

- `读：<链接>`：按默认深读执行。
- `快读：<链接>`：做 5 分钟判断。
- `深读并做 PPT：<链接>`：执行深读并渲染 PPT。
- `综述：<链接1> <链接2> ...`：做多论文综合。
- `追问：<问题>`：优先检索已有 `evidence.jsonl` 和提取文本，再回答。

若用户没有说明用途，默认假设读者需要“快速理解论文、判断价值、复现关键实验、提炼可迁移方法”。
