# 合成材料的本地演示结果

此结果仅用于展示输出方式，不是客户案例或平台认证。

# 引用统计口径修订后复测

日期：2026-11-18。独立评估agent重新读取更新后的 seoguan-citation-audit/SKILL.md，并按新字段实际完成以下合成案例。未联网、未读取真实回答、未修改产品文件。

这是收到作者修改说明后的定向回归复测，不是盲测。结论针对统计口径是否能明确表达，不证明真实宿主安装或网页抓取能力。

## 完整合成输入

- platform：synthetic
- question_id：Q01
- 原问题：工业泵怎么选？
- time：2026-11-18（未提供时分或时区，不补写）
- run：R1
- 材料完整性：完整回答，共两段，第1段引用[1]，第2段引用[2]
- 面板[1]：https://example.com/a
- 面板[2]：https://other.test/b
- 视频：无
- 目标域名：example.com
- 目标品牌与两段自然语言全文：未提供
- 来源网页原文：未提供

## 实际执行与答复

按主机名匹配，https://example.com/a 属于目标域名；https://other.test/b 不属于目标域名。

| 正文位置 | 标记 | URL | 主机名 | 计入目标域 | 正文发生次数 |
|---|---|---|---|---|---:|
| 第1段 | [1] | https://example.com/a | example.com | 是 | 1 |
| 第2段 | [2] | https://other.test/b | other.test | 否 | 1 |

因此，目标域正文唯一URL为1、正文引用发生1次、面板来源1条；全量正文唯一URL为2、正文引用发生2次、面板来源2条。全量视频卡为0，本例没有卡片需要另行判断目标归属。面板出现不再累加为正文引用发生次数。

### citation-audit.csv

```csv
record_id,platform,question,time,target_domain,completeness,brand_mention,recommendation,target_body_unique_urls,target_body_occurrences,target_panel_urls,video_cards,all_body_unique_urls,all_body_occurrences,all_panel_urls,evidence,question_id
R1,synthetic,工业泵怎么选？,2026-11-18,example.com,完整（合成引用结构）,未知,未知,1,1,1,0,2,2,2,第1段[1]对应example.com/a；第2段[2]对应other.test/b；两条面板记录不另计正文发生次数,Q01
```

record_id采用提供的运行标识R1，保留question_id=Q01与原问题全文。目标品牌与自然语言正文未提供，brand_mention、recommendation保持未知；不能由目标域URL存在推断品牌提及或明确推荐。引用对应关系来自本次完整合成结构，来源原文未给，不能声称它支持回答内容或完成词句复用核验。

## 复测结论

新schema允许在同一行明确表示目标域1个1次与全量2个2次，target_与all_不再需要靠表外猜测统计范围；target_domain也被保留。原评审第3项的接口清晰度建议在本案例范围内关闭。无需进一步修改技能文件。
