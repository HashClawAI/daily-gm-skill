# Module Definitions

Each module below is a self-contained prompt block. When assembling the cron job payload,
concatenate the enabled modules' prompts and wrap with the delivery/format header.

---

## Module 1: Zen / Buddhist Wisdom (`zen`)

**Requires:** None (uses model knowledge)

**Prompt block:**
```
【一、禅·每日一课】
- Rotate sources: Diamond Sutra commentaries, Chan/Zen koans, patriarch sayings, classic gathas
- Pick one theme or passage: give original text + plain-language explanation + life insight
- 200-300 characters
- Do not repeat recent days' content
```

**Customization:** Users may specify preferred traditions (Zen, Theravada, Tibetan, Stoicism, Taoism, etc.)
or specific texts to draw from. Replace the source list accordingly.

---

## Module 2: Chinese Metaphysics Daily Forecast (`metaphysics`)

**Requires:** User birth info (date, time, location) for BaZi chart

**Setup:** When enabling this module, collect:
- Birth date (solar calendar)
- Birth time (approximate hour is fine)
- Birth location (for regional adjustments)

Derive the Four Pillars (年柱/月柱/日柱/时柱), Day Master, and Five Elements balance.
Store these in the cron prompt so they don't need recalculation.

**Prompt block:**
```
【二、每日运程·命理】
BaZi: {year_pillar} {month_pillar} {day_pillar} {hour_pillar}
Day Master: {element}, tendency: {strong_or_weak}, favorable: {favorable_elements}, unfavorable: {unfavorable_elements}
- Use web_search to find today's Heavenly Stem & Earthly Branch (干支) and Chinese almanac (黄历)
- Analyze today's stem-branch interactions with the natal chart
- Include: overall fortune, career/wealth, relationships, health, do's and don'ts
- 200-300 characters
```

---

## Module 3: Calendar & Todos (`calendar`)

**Requires:** memory files; optionally `gog` skill for Google Calendar

**Prompt block:**
```
【三、待办日程】
- Read memory/<today>.md and MEMORY.md for pending tasks and reminders
- If gog skill is configured, check today's Google Calendar events
- List items if found; otherwise write "今日无特别日程" / "No scheduled items today"
```

---

## Module 4: Email Digest (`email`)

**Requires:** `himalaya` or `gog` email skill configured

**Prompt block:**
```
【四、邮件摘要】
- If himalaya or gog email is configured, fetch unread emails from last 24h
- Summarize top 5 with sender, subject, one-line summary
- If not configured, skip this section entirely
```

**Note:** This module is auto-skipped when no email tool is available.

---

## Module 5: Top 10 News Headlines (`news`)

**Requires:** web_search access

**Prompt block:**
```
【五、每日十大要闻】
- Use web_search for today's major news (1 search), then web_fetch one aggregator page for details
- Compile 10 most important headlines, one sentence each
- Cover: international, tech, finance, crypto, society
- Respect rate limits: space searches ≥ 2s apart, max 3 total searches across all modules
```

**Customization:** Users may specify preferred news categories or regions.

---

## Module 6: Academic Paper Picks (`academic`)

**Requires:** web_search access

**Prompt block:**
```
【六、每日学术文章】
- Use web_search for notable recent papers (AI, blockchain, science, philosophy, etc.)
- Recommend 1-2 papers: title, source, one-sentence summary, link
```

**Customization:** Users may specify preferred research domains.

---

## Assembly Template

When building the final cron prompt, use this structure:

```
现在是每天早上{time}，请向 {channel_description} 推送今日晨报。
将以下内容整合为一条消息发送，用分隔线区分各板块。

{module_1_prompt}

{module_2_prompt}

...

═══ 格式要求 ═══
- 全部内容整合为一条消息发送，用 message 工具发送到 channel={channel}, channelId={channelId}
- 各板块用 emoji 和分隔线区分，清晰易读
- 如果某个板块的工具未配置或查询失败，跳过该板块，不要因此失败
- 总篇幅控制在合理范围，不要超过2000字
- 效率优先：尽量减少搜索次数，能合并的查询就合并
```
