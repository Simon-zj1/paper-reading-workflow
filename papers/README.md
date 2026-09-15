# 论文库

目录约定：

```text
papers/
├── inbox/                         # 刚收到、尚未整理的文件
└── library/<paper_id>/
    ├── README.md
    ├── meta.json
    ├── evidence.jsonl
    ├── source/                    # 原始 PDF、网页快照、补充材料
    └── extracted/                 # Markdown、图表、公式和解析中间文件
```

`paper_id` 使用 `firstauthor-year-shorttitle`，例如：

```text
yao-2022-react
brohan-2023-rt2
black-2024-pi0
```

同一论文更换版本时，保留相同 `paper_id`，在 `meta.json` 中更新版本和访问时间。
