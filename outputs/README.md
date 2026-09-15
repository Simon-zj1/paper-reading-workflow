# 输出目录

默认深读包：

```text
outputs/<paper_id>/
├── 00_one_page.md
├── 01_deep_note.md
├── mindmap.mmd
└── deck_storyboard.md
```

证据链保存在 `papers/library/<paper_id>/evidence.jsonl`，避免输出副本和论文库记录发生漂移。

按需追加：

```text
├── multi_paper_matrix.md
├── synthesis.md
├── research_gaps.md
├── reading_order.md
├── paper_brief.pdf
├── paper_deck.pptx
├── mindmap.html
└── assets/
```

导出文件必须保留生成源文件。PDF 和 PPT 交付前需要逐页渲染检查。
