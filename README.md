Product leader. I turn complex, rule-heavy domains into products people can trust: tax at Skatteguiden, data and regulatory platforms at Ørsted.

Since 2025 I have been building hands-on with AI coding agents (mainly Claude Code). I own the product decisions, the rules and the verification. The agent writes most of the code. Two projects I use every week are described below.

The common thread: **the system is sometimes wrong, so the design has to make that visible and cheap to fix.**

---

## 1. Job-search pipeline

An automated pipeline that finds, filters and ranks product roles in Denmark, and prepares applications for me to review and send myself.

Built on [Mads Lorentzen's open-source ai-job-search](https://github.com/MadsLorentzen/ai-job-search), extended in a private repository (it holds personal data).

**What it does**
- Scheduled runs (Windows Task Scheduler) search six job portals: Jobindex, Jobnet, Jobbank, Jobdanmark, Freehire and LinkedIn.
- A rule-based prefilter runs before any model is called:
  - title tracks (product vs. project)
  - an exclusion list
  - commute zone, using my own postcode table after the Danish address API was shut down
- An enrich step extracts the deadline, work model, workplace and contact person, and flags postings with a phone number so I can call before applying.
- An LLM ranks the remaining jobs against my profile and writes the reasoning in Danish. A cheaper model ranks, and a stronger model drafts and reviews.
- A tracker logs every application and decline with a reason. It also prepares what I need to register applications for the unemployment fund (Jobnet Joblog).
- I get a phone notification only for new relevant jobs, borderline cases or portal failures.

**Where it is wrong, and how I designed for it**
- **Duplicates are flagged, never deleted.** A scaling company can post two similar roles that are genuinely different, so a fuzzy match must never make a real job disappear without review.
- **Failed fetches are not "expired" postings.** The original code marked a timeout as a closed job. I measured it, reported it, and the pipeline now separates the two.
- **Model assumptions get checked.** One example: an agent "corrected" a real contact person's name because it looked like a placeholder.
- **Search coverage is measured, not assumed.** When the pipeline missed relevant roles, I traced them to missing search terms and added new terms only after a dry run showed no extra noise.
- **Borderline cases have their own section**, with a low threshold, so a good job is not lost to a strict score.

**Contributions upstream**
- Five issues filed on bugs found in daily use (#534-#538), with measured hit counts per portal.
- [PR #637](https://github.com/MadsLorentzen/ai-job-search/pull/637): Danish/Latin character folding for job keys, plus an audit and migration CLI with backup, collision checks and idempotency.
- [PR #653](https://github.com/MadsLorentzen/ai-job-search/pull/653): one plain search term per portal call, with documented query syntax per portal, backed by a measurement table across six portals.

Stack: Python, Claude Code, PowerShell, git.

---

2. School calendar agent (Aula)
Open source (MIT): https://github.com/ai-tvang/aula-kalender-agent

Danish schools communicate through Aula: posts, message threads, PDFs, Word files and photos of handwritten notes. Dates for parent meetings, birthdays and duties are buried in all of it. This agent replaces 100+ Aula checks a year with one email on Sunday and a calendar that updates itself.

What it does

Every week it reads new posts, threads, attachments and week plans, and:
- puts parent meetings, events, birthdays, reply deadlines and days off into Outlook, tagged with the child's name, and moves them when the school changes a date
- sends one email with "remember this week": homework, trips, PE kit and when each child finishes school, day by day
- lists the few things that need a parent's decision

Works with Claude or ChatGPT, on a subscription or an API key, so parents use the AI they already pay for.
One-line install on Windows with a guided setup: MitID, AI login and Outlook in about 10 minutes.

How I designed it

Only confirmed dates go in the calendar. Unclear items go to the email, so the calendar can be trusted.
Plain-language rules. When it gets something wrong, I fix the rule file, not the code.
Safe by default. Logins are encrypted, and messages from other parents can't make the AI read files or reach the internet.

Stack: Python (uv), Aula client library, Microsoft Graph, Claude Code and Codex CLI in headless mode, Anthropic and OpenAI APIs, PowerShell.
The code is private because it handles my family's data.

---

## Contact

[LinkedIn](https://www.linkedin.com/in/) · Based in North Zealand, close to Copenhagen
