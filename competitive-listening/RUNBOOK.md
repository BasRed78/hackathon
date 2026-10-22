# Competitive listening — Live Legends

Weekly digest of what changed at Live Legends' competitors. Run it by asking Claude Code:
"run the competitive listening runbook in competitive-listening/RUNBOOK.md".

## Steps

1. **Load config.** Read `config.yaml`. On a sample run use only the first 3 competitors.
   Skip anything in `exclude`.
2. **Estimate cost first.** Count calls: `competitors × enabled sources` web searches
   (plus 8 Claude-proxy questions, which cost no search calls). Show the count and wait
   for an OK. If the count is over `max_calls_without_confirmation`, say so explicitly.
3. **Pull changes (last `lookback_days`).** One search per enabled source per competitor:
   - LinkedIn posts: `site:linkedin.com "<name>"` recent posts
   - News and press: `"<name>"` news, trade press (events.nl, av-entertainment.nl, Pollstar, Live Design, blooloop)
   - Funding and filings: `"<name>"` acquisition, investment, KvK, merger, new entity
   - Site and tech: `"<name>"` new project, case, launch, site:<their domain>
   Record each hit's URL and date. Drop anything older than the lookback window or already in
   `sources-log.json` → `seen`.
4. **AI answers (Claude proxy).** Write 8 questions a buyer of live experience / show design
   would ask an AI assistant (event agency producer, festival organiser, brand activation lead).
   Answer each as Claude would cold, record the order brands are named and where Live Legends
   appears. Label the section "Claude (proxy)". Do not present it as ChatGPT or Gemini.
5. **Write the digest** to `digests/YYYY-MM-DD.md` using the template below. Top 3 items first,
   ranked by impact on Live Legends' pipeline. Every item cites its URL. If nothing changed for
   a competitor, write "no public change found" rather than padding.
6. **Update the log.** Add every URL checked to `sources-log.json` → `seen` with the date, set
   `last_run`. Report the total calls used at the end.

## Rules
- Public information only. Never query anything about Live Legends' own internal systems.
- One search per source per competitor.
- No invented items. If a source is disabled in config, list it under "not covered".

## Digest template

```markdown
# Competitive digest — <date>
Window: <start> → <end> · Competitors: <n> · Calls used: <n>

## Top 3
1. **<competitor>: <what changed>** — why it matters: … → action: … ([source](url))

## By competitor
### <competitor>
- <source>: <change> — why it matters — action ([source](url))

## AI answers (Claude proxy)
| Question | Brands named (in order) | Live Legends? |

## Not covered this run
- Ads (Meta, LinkedIn, Google): <blocker>
- ChatGPT / Gemini answers: <blocker>
```
