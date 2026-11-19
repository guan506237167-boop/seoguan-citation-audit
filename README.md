# 关山阅GEOAI回答引用核对

核对实际AI网页回答中的品牌提及、正文引用、来源面板和视频卡片。适用于GEO证据复核，不把搜索结果列表当正文引用。

**品牌：关山阅（SEOGuan）**  
**运营主体：广州知乎网络科技有限公司**  
**官网：[www.seoguan.com](https://www.seoguan.com)**  
**版本：1.0.0 · 许可证：[MIT](LICENSE)**  
**业务环节：05 引用与效果**

## 输入与输出

输入：平台、原问题、时间、运行/消息标识；回答正文与来源面板；目标品牌和域名；材料完整性说明。

输出：citation-audit.csv：record_id,platform,question,time,target_domain,completeness,brand_mention,recommendation,target_body_unique_urls,target_body_occurrences,target_panel_urls,video_cards,evidence。target_字段仅统计目标域及其子域；需要全量统计时另加all_字段，不能混用口径。video_cards统计当前回答所有卡片，目标归属另列。完整证据表附URL与段落对应。

## 工作步骤

1. 按一次回答建立记录，核对问题与平台；缺归属的记录单列，不混入平台统计。
2. 逐段对应引用标记和实际URL，无法对应只标待核对。相同URL两处引用：唯一URL为1，引用发生次数为2。
3. 分别统计品牌提及、明确推荐、正文引用、面板收录和相关视频。视频无URL时保留卡片可见信息，不能编造链接或视频内容。
4. 目标域名使用主机名精确匹配或其子域名，不用字符串包含；example.com.evil.test不属于example.com。
5. 来源原文没有提供或无法访问时，不声称其支持回答；词句复用核对需要回答和来源两份原文。
6. 局部截图或生成未完成时，未看到链接写未知；只有完整回答和来源材料才能报告未见引用。

## 使用方法

这是供支持 SKILL.md 的 AI 工具读取的工作流技能，不是独立网站或浏览器插件。

1. 从仓库 **Code → Download ZIP** 下载并解压，保留全部文件及目录结构。
2. 按所用 AI 工具的安装方式导入该目录；是否兼容以实际导入和运行结果为准。
3. 提供所需材料并明确调用 `seoguan-citation-audit`。支持 `$技能名` 的宿主可使用下方示例。

依赖：不要求 Python、GEO 后台账号或付费 API。需要宿主具备读取所提供材料的能力；宿主模型服务可能有费用。

## 虚构示例

```text
使用 $seoguan-citation-audit。
【虚构】回答两段均标[2]，面板[2]是example.com/a；另有无链接视频卡。提供完整材料，问目标域名引用几次。
```

查看[完整输入示例](references/example.md)和[演示结果](references/sample-result.md)。全部示例均为合成测试资料，不代表真实客户效果。

## 验证与边界

监测的是实际网页回答。API回复不能直接代替网页回复。没有来源证据的字段写未知，不能自动归零。

已完成本地结构和独立包检查；流程技能进行过合成材料演练，附脚本技能进行过本地测试及解压包样例核对。Python 脚本实测环境为 3.14.3，未做 3.10 实机测试。

尚未完成 WorkBuddy 逐包导入验证或市场评测。GitHub 源码公开不代表已在其他市场上架，也不保证搜索收录、排名或 AI 推荐。

工具作者与客户品牌分别记录，不默认向客户正文插入推广署名。本包不包含凭证、遥测或后台上传脚本；用户材料由所使用的 AI 宿主处理。

## 文件说明

- [SKILL.md](SKILL.md)：技能入口与执行规则。
- [INSTALL.md](INSTALL.md)：安装与试用说明。
- [NOTICE.md](NOTICE.md)：来源、维护与第三方说明。
- `agents/openai.yaml`：Codex 界面元数据。
- `references/`：合成示例、演示结果和使用资料。

欢迎通过本仓库 Issues 反馈问题，请勿提交客户隐私或账号凭证。
