# claude-skills

Private repo for custom Claude Code skills, so they can be version-controlled and pulled into any project/session.

## Skills

- [`article-review-sop`](article-review-sop/SKILL.md) — 六站内容矩阵的文章审核与上线后优化 SOP（v3.1）。文章发布判定、硬闸门/软闸门、评分、标签与 SLA、上线后复审。

## Usage

To use a skill in a Claude Code session, copy or symlink the skill folder into that project's `.claude/skills/` directory (or the equivalent global skills directory), e.g.:

```bash
cp -R article-review-sop /path/to/project/.claude/skills/
```

## Updating

Edit the skill folder here, commit, and push. Re-sync into consuming projects as needed.
