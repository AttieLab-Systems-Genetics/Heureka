# Tracking Token Usage in Heureka Bench

**Prompt:**
How many tokens have I used How can I keep track of token usage?

## Short answer

I can't report an exact token count for this session or your account — that figure isn't exposed to me as a tool or context value. It lives in the app's billing/usage layer, not in anything I can query. Here's where to find it and how to monitor it going forward.

## Where to check your actual usage

**Credits & Usage** — under your avatar (top-right of the app) → **Credits & Usage**. This shows:

- Current credit balance
- Usage history over time
- (Typically) a breakdown by session or project, depending on how Heureka Labs has instrumented it

This is the authoritative source. Any number I gave you here would be a guess, and I don't fabricate data — so go there directly for the real figure.

## Why I can't see it from inside a conversation

Token accounting happens at the platform/billing layer, outside the tool set I have access to (file I/O, code execution, literature search, the Archimedes model, etc.). I have no "usage" tool, so there's nothing in my toolbox to call for this — it's not a matter of the data being restricted, it's that the capability isn't wired into my tool schema.

## Practical ways to keep track going forward

1. **Check Credits & Usage periodically** — especially before starting a token-heavy task (large literature syntheses, long agent forks, big file reads). A quick glance before and after a heavy task gives you a delta.
2. **Watch for cost-heavy patterns in how you work with me:**
   - Reading very large files in full (vs. targeted `Grep`/offset reads) burns more tokens.
   - Spawning multiple parallel sub-agents (`Agent` tool) multiplies token spend — each fork runs its own full context.
   - Long-running conversations accumulate context; the harness summarizes older turns automatically, but very long sessions still cost more than fresh ones.
3. **Use `Referral & Earn`** if you want to offset usage — inviting collaborators can earn credits, visible from the same avatar dropdown.
4. **If you need usage broken out by project or task** for budgeting purposes, that's worth flagging directly to Heureka Labs support (via the GitHub issues link: <https://github.com/heurekalabs/arc/issues>) as a feature request if the current Credits & Usage view doesn't give you the granularity you want — I don't have a way to generate that breakdown myself.

## Bottom line

There's no in-conversation counter I can surface. **Credits & Usage** in the app is the place to look, and periodic checks there — especially around heavy analysis or multi-agent tasks — are the practical way to stay ahead of consumption.
