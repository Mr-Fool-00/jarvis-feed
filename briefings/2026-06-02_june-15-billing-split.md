# June 15 Billing Split — 8/10

## What it is
Starting June 15, Anthropic splits how Claude counts usage. Right now, everything — chatting, coding in the terminal, AND running automated routines — all comes out of one flat subscription. After June 15, automated/programmatic stuff (like Jarvis's scheduled runs) gets its own separate credit bucket, billed at regular API rates.

**The short version:** Your $200 Max 20x plan keeps covering your interactive Claude Code work. But Jarvis's cron runs will start drawing from a separate $200 credit pool at full API pricing. When that $200 is gone, either you get charged more (if you turn on overflow), or Jarvis gets cut off.

## Why you'd want to know this (specific to your stack)
Jarvis runs twice a day via Claude Code Routines. Those are scheduled, programmatic runs — they fall under the new programmatic pool. Each Jarvis run does WebSearch calls, commits files, and writes digests. At current Sonnet/Opus API rates, a typical Jarvis run might consume several hundred thousand tokens. If both runs per day add up to, say, 1-2M tokens per day of Sonnet input, you're looking at meaningful monthly costs at API rates — potentially eating through that $200 credit in 1-2 weeks.

## Why I think it's worth your attention
**The deadline is June 8** — that's when you need to claim your $200 credit in your Claude account settings. If you miss claiming it, you might not get it. And June 15 is 13 days from now. This isn't something you can defer to next week.

## What to do

1. **Before June 8**: Log into your Claude account → Billing → claim the $200 programmatic credit
2. **Before June 15**: Decide whether to enable overflow protection (yes = charges at API rates after credit runs out; no = Jarvis gets blocked after $200 runs out)
3. **Optional audit**: Check how many tokens the average Jarvis run uses by looking at session logs. If it's light, $200/month is probably fine. If it's heavy, you may want to optimize the run (fewer WebSearch calls, shorter prompts, etc.)
4. **Keep the flat plan for your interactive work**: Nothing changes for your Claude Code terminal sessions or Claude.ai chat — those stay on the unlimited flat rate

🔗 https://findskill.ai/blog/claude-code-pricing-after-june-15-decision-table/
🔗 https://devtoolpicks.com/blog/anthropic-splits-claude-subscriptions-agent-sdk-credit-june-2026
🔗 https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan
