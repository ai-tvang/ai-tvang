Product leader. I turn complex, rule-heavy domains into products people can trust: tax at Skatteguiden, data and regulatory platforms at Ørsted.

Since 2025 I have been building hands-on with AI coding agents (mainly Claude Code). I own the product decisions, the rules and the verification. The agent writes most of the code. Two projects I use every week are described below.

The common thread: AI does the reading and sorting, and the design makes its mistakes visible and cheap to fix.

## 1. Job-search pipeline

Built on Mads Lorentzen's open-source [ai-job-search](https://github.com/ai-job-search-dev/ai-job-search), extended in a private repository because it holds personal data.

An automated pipeline that finds, filters and ranks product roles in Denmark, and prepares applications for me to review and send myself.

### What it does

- Searches six job portals on a schedule: Jobindex, Jobnet, Jobbank, Jobdanmark, Freehire and LinkedIn.
- Filters with rules before any model is called: title tracks (product vs. project), an exclusion list and a commute zone based on my own postcode table.
- Extracts deadline, work model, workplace and contact person, and flags postings with a phone number so I can call before applying.
- Ranks the remaining jobs against my profile, with reasoning in Danish. A cheaper model ranks; a stronger model drafts and reviews.
- Logs every application, and every job I skip with the reason why, and prepares what I need for the unemployment fund (Jobnet Joblog).
- Notifies my phone only for new relevant jobs, borderline cases or portal failures.

### How I designed it

- **Nothing disappears silently.** Duplicates are flagged, never deleted, and a failed fetch is never treated as an expired posting.
- **Measured, not assumed.** New search terms are added only after a dry run shows they find relevant roles without extra noise.
- **Good jobs are not lost to a strict score.** Borderline cases get their own section with a low threshold.

### Contributions upstream

- [PR #637](https://github.com/ai-job-search-dev/ai-job-search/pull/637) (merged): Danish/Latin character folding for job keys, plus an audit and migration CLI with backup, collision checks and idempotency.
- [PR #653](https://github.com/ai-job-search-dev/ai-job-search/pull/653) (merged): one plain search term per portal call, with documented query syntax and a measurement table across six portals.
- [PR #761](https://github.com/ai-job-search-dev/ai-job-search/pull/761) (merged): keeps Notion rows in sync when a job key changes, by matching on URL and re-keying the row.
- Six issues on bugs found in daily use, all closed: [#534](https://github.com/ai-job-search-dev/ai-job-search/issues/534), [#535](https://github.com/ai-job-search-dev/ai-job-search/issues/535), [#536](https://github.com/ai-job-search-dev/ai-job-search/issues/536), [#537](https://github.com/ai-job-search-dev/ai-job-search/issues/537), [#538](https://github.com/ai-job-search-dev/ai-job-search/issues/538) and [#655](https://github.com/ai-job-search-dev/ai-job-search/issues/655). The search-term issue (#536) came with measured hit counts per portal.

**Stack:** Python, Claude Code, PowerShell, git.

## 2. School calendar agent (Aula)

Open source (MIT): [ai-tvang/aula-kalender-agent](https://github.com/ai-tvang/aula-kalender-agent)

Danish schools communicate through Aula: posts, message threads, PDFs, Word files and photos of handwritten notes. Dates for parent meetings, birthdays and duties are buried in all of it. This agent replaces 100+ Aula checks a year with one email on Sunday and a calendar that updates itself.

### What it does

- Reads the week's new posts, threads, attachments and week plans.
- Puts parent meetings, events, birthdays, reply deadlines and days off into Outlook, tagged with the child's name, and moves them when the school changes a date.
- Sends one email with "remember this week": homework, trips, PE kit and when each child finishes school, day by day.
- Lists the few things that need a parent's decision.
- Works with Claude or ChatGPT, on a subscription or an API key, so parents use the AI they already pay for.
- Installs on Windows with one line and a guided setup: MitID, AI login and Outlook in about 10 minutes.

### How I designed it

- **Only confirmed dates go in the calendar.** Unclear items go to the email, so the calendar can be trusted.
- **Plain-language rules.** When it gets something wrong, I fix the rule file, not the code.
- **Safe by default.** Logins are encrypted, and messages from other parents can't make the AI read files or reach the internet.

**Stack:** Python (uv), Aula client library, Microsoft Graph, Claude Code and Codex CLI in headless mode, Anthropic and OpenAI APIs, PowerShell.

## Contact

[LinkedIn](https://www.linkedin.com/in/thomas-christian-vang-1b625828) · Based in North Zealand, close to Copenhagen
