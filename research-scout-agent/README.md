# Tech News Research Scout

A personal AI agent that scouts recent tech industry news and delivers a short, sourced briefing on demand — built so I can catch up on what happened in tech without manually browsing multiple news sites.

**Who it's for:** anyone who wants a fast, honest digest of recent tech news a few times a week, with sources attached and rumors clearly flagged, instead of scrolling multiple sites.

## Setup (reproducible by a stranger)

1. Go to [claude.ai](https://claude.ai) and create a new **Project**.
2. Name it (e.g. "Tech News Research Scout").
3. Open the project's **Project instructions** field and paste the following exactly:

   > "You are my tech news research scout. When I ask for an update, search the web for notable tech industry news from the last 2-3 days — product launches, major company announcements, AI/software developments, and significant funding or acquisition news. Summarize the 5-8 most relevant stories in a short briefing: one or two sentences per story, plus the source. Group similar stories together instead of listing near-duplicates separately. Skip rumors or unconfirmed leaks unless clearly labeled as such. If nothing significant happened, say so honestly rather than padding the list with minor stories."

4. No additional setup is required — Claude's built-in web search tool is available by default inside a Project, with no separate connector, account, or API key needed. This makes the whole build free to run.
5. Start a new chat inside the project and ask for an update (see usage example below).

## Usage example

**Input:** "What's new in tech this week?"

**Output (real run):** The agent returned 5 sourced stories covering things like a major AI lab's model safety classification, a competing AI company's data-retention policy change, and a large ride-share company's layoffs tied to a robotaxi investment push — each with a one-to-two sentence summary and a named source. It also proactively flagged two additional items as "aggregator-only, not independently verified" without being explicitly asked to make that distinction, and explicitly stated "no major unconfirmed leaks found" rather than inventing a rumor to fill space.

## Architecture

```
[My request: "What's new in tech?"]
            |
            v
[Claude Project — custom instructions]
            |
            v
   [Web search tool (built-in)]
            |
            v
[Sourced, grouped, rumor-flagged briefing]
```

No custom code, server, or database — the entire "system" is a set of instructions plus Claude's built-in web search tool.

## Evaluation results (current)

Five test cases were designed and run against the live agent:

| # | Test case | Result |
|---|---|---|
| 1 | Normal run — ask for a general update | **Passed.** Returned 5 real, dated, sourced stories, no fabrication. |
| 2 | Quiet news period — agent's behavior when little happened | **Not yet observed.** No genuinely slow news day has occurred during testing so far; this remains untested. |
| 3 | Duplicate/near-duplicate story grouping | **Passed.** Two related stories about the same underlying trend were grouped into a single item rather than listed twice. |
| 4 | Rumor filtering | **Passed.** No confirmed rumors existed during the test window, and the agent explicitly said so rather than fabricating one; it also proactively flagged two under-verified items as secondary. |
| 5 | Source accuracy check | **Passed.** I independently verified one reported story against multiple outlets and confirmed the agent's summary matched the real reporting, with no invented details. |

## Limitations

- **Test Case 2 (quiet news day) remains unverified** — I have not yet observed how the agent behaves when there is genuinely little tech news, since every test run so far has landed on an active news day.
- **No autonomous scheduling.** The agent only runs when I manually ask for an update inside the chat — it does not run itself on a timer or notify me proactively.
- **Depends entirely on web search quality and recency at the moment of the request** — if search results are thin, outdated, or biased toward certain sources, the briefing inherits that limitation.
- **No memory across runs.** Each request is independent; the agent does not track what it already told me in a previous session, so it cannot say "here's what's new since last time" without me specifying a time window myself.
- **Not fact-checked at scale.** I manually verified one story's accuracy as a spot-check (Test 5); the agent does not automatically verify every story it reports.

## Built with AI — transparency note

This agent's instructions, evaluation design, and troubleshooting were developed together with Claude (Anthropic) as a build partner: Claude helped draft the initial agent instructions, proposed the five test cases, and helped debug deployment issues on a related project. All actual test runs, the source-accuracy verification in Test 5, and the final judgment on pass/fail for each test case were performed and confirmed by me, not generated or assumed by the AI.
