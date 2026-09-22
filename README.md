# claude-skills

Private repo for custom Claude Code skills, so they can be version-controlled and pulled into any project/session.

## Skills

- [`article-review-sop-skill`](article-review-sop-skill/SKILL.md) — 六站全文消费者质量审核、优化后复审与正式发布审核。质量目标为信息增量中等偏上；共享内容标准，保留发布闸门、评分、标签与 SLA。

## Usage

To use a skill in a Claude Code session, copy or symlink the skill folder into that project's `.claude/skills/` directory (or the equivalent global skills directory), e.g.:

```bash
cp -R article-review-sop-skill /path/to/project/.claude/skills/
```

## Updating

Edit the skill folder here, commit, and push. Re-sync into consuming projects as needed.
