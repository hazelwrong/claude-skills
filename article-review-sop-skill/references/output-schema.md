# 自动裁定模式：输出格式

对方要批量审核、要结构化结果、或明确要 JSON 时用这个模板。字段名保持英文 key（方便下游系统解析），内容用中文。

```json
{
  "final_decision": "通过上线 | 先发后改 | 不发重改 | 人工复核",
  "destination_library": "发布无问题库 | 发布后待优化库 | 不发布待优化库 | 成功发布库",
  "content_quality_status": "通过 | 未通过 | 待核实",
  "delivery_status": "完成 | 部分完成 | 未改",
  "technical_status": "本次范围无发现 | 待处理 | 未检查",
  "consumer_quality_issues": [],
  "total_score": 0,
  "scores": {
    "google_page_basics": 0,
    "search_intent": 0,
    "information_gain": 0,
    "site_uniqueness": 0,
    "product_data_support": 0,
    "source_outbound_links": 0,
    "compliance_trust": 0,
    "publishing_experience": 0
  },
  "hard_gates": [
    {"code": "HG-3", "reason": "无信息增量", "evidence": "...", "required_fix": "..."}
  ],
  "soft_tags": [
    {"code": "SOFT-IG", "owner": "编辑/SEO", "sla": "7天", "fix": "补充对比矩阵和内部数据"}
  ],
  "information_gain_assessment": {
    "level": "无增量 | 弱增量 | 中等增量 | 中等偏上 | 高增量",
    "evidence": ["..."],
    "similar_existing_urls": ["..."]
  },
  "source_link_assessment": {
    "authority_issues": [],
    "broken_or_risky_links": [],
    "recommended_fixes": []
  },
  "next_actions": [
    {"owner": "SEO", "action": "上线后补充TD", "due": "7个工作日"}
  ],
  "human_review_required": false
}
```

## 填写规则

- `hard_gates` 为空数组时，`final_decision` 才可能是「通过上线/先发后改」；只要 `hard_gates` 非空，`final_decision` 必须是「不发重改」，`destination_library` 必须是「不发布待优化库」——这两者不能矛盾。
- 命中硬闸门时必须填 `evidence`（具体证据）和 `required_fix`（怎么改），不能留空或写"有问题"三个字了事。
- `soft_tags` 里的每一项都要能在 tags-and-post-publish.md 的标签组表里找到对应的 owner/SLA，不要自己编一个不在表里的 owner。
- `total_score` 落在 60–64 区间，或任何模块因为信息不足无法打分时，`human_review_required` 设为 `true`，并在 `next_actions` 里写清楚缺什么信息。
- `information_gain_assessment.level` 要和 `hard_gates`/`soft_tags` 保持一致：level 是"无增量"就必须同时出现 HG-3；level 是"弱增量"就必须同时出现信息增量增强的软标签。
- 如果只拿到文章正文、没有页面 HTML/Schema/链接信息，`scores.google_page_basics` 和 `source_outbound_links` 这两项无法可靠打分，在对应字段注明"信息不足"并整体调低置信度，倾向于把 `human_review_required` 设为 `true`，而不是假设页面基础没问题直接打满分。

共同质量底线优先：content_quality_status未通过时不能判通过上线或先发后改；确定质量未达标写不发重改，关键证据不足写人工复核。弱/中等增量低于当前目标，不能仅靠历史总分放行。命中硬闸门不评分，total_score及相关未知分项用null，不能用0冒充实评分；数据缺失也用null。消费者内容审核集成Excel时不强制此JSON。
