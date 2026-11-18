# 题库

[dsh-nihongo-test](https://github.com/utoisu/dsh-nihongo-test) 的数据目录备份（私有）。
插件本身只有代码，题库、试卷、成绩单都在这里。

## 目录

```
banks/     五个题库（词汇 / 语法 / 阅读理解 / 日译中 / 中译日）
papers/    试卷（含题目快照，建卷后就不再随题库变）
reports/   成绩单（客观题逐题判定、实务双评/三评记录）
runtime/   进行中的考试（倒计时、已答内容）
wrong.json 错题库（答错过的客观题，含错误次数）
audit_log.jsonl  操作审计日志（导入、放行、改题、交卷、判分）
exports/   导出的 Markdown（不纳入版本管理）
```

## 怎么用

这是插件的数据目录。把 `dsh-nihongo-test` 装进 DSH 后，在 profile 的 `cordis.patch.yml` 里把这个目录指给插件：

```yaml
- id: dsh-nihongo-test
  config:
    dataDir: '/绝对路径/本仓库'
```

插件按文件 mtime 自动重读，改完不用重启。

## 条目格式

```jsonc
// 客观题（banks/vocab.json、grammar.json）
{ "id": "V-0001", "type": "vocab", "stem": "…", "options": ["…","…","…","…"],
  "answer": "B", "explanation": "…", "difficulty": "L3", "source": "2014 二笔真题（解析版）",
  "exam_kind": "real", "year": 2014, "level": "2", "tags": ["真题"], "status": "active" }

// 阅读（一篇一条，带小题）
{ "id": "R-0001", "type": "reading", "passage": "…",
  "questions": [{ "stem": "…", "options": ["…"], "answer": "A", "explanation": "…" }] }

// 实务段（整篇 + 参考译文）
{ "id": "JC-0001", "type": "ja2zh", "text": "…", "ref": "…", "ref_src": "official",
  "chars": 656, "full_text": true }
```

`exam_kind`：`real` 真题 / `mock` 模拟 / `textbook` 教材；`level`：`2` 二笔 / `3` 三笔；`status`：`active` 在库（参与组卷）/ `candidate` / `needs_human` / `disputed` / `rejected`。

## 说明

- 内容是真题原文、教材段落与参考译文，仅作个人练习与备份，不作为出版或再分发使用。
- 纯文本 JSON，可以直接用编辑器改；改坏了有 git 兜底。
