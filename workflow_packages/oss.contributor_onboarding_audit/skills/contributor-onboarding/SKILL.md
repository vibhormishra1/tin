---
name: contributor-onboarding
description: Audit a GitHub repository's contributor onboarding experience across five dimensions and produce a prioritized fix list.
---

## Step 1 — Read the repository

If no GitHub repository is connected, stop here. Write reports/OSS_CONTRIBUTOR_AUDIT.md
with only this message and do not continue:

> This workflow requires a connected GitHub repository.
> Connect one from the project integrations page and run again.

When connected, read these in order:

1. **README.md** — full contents.
2. **CONTRIBUTING.md** — full contents. Note explicitly if absent.
3. **Open issues** — scan for labels: `good first issue`, `help wanted`,
   `beginner`, `starter`. Note how many exist, their age and whether they
   are actionable (has a clear description, not just a title).
4. **Recent pull requests** — last 20. For each external PR (author is not
   a maintainer), note time to first review response.
5. **Issue templates** — `.github/ISSUE_TEMPLATE/` or `.github/ISSUE_TEMPLATE.md`.
   Note if present or absent.
6. **FUNDING.yml** — `.github/FUNDING.yml`. Note if present or absent.
7. **Repo metadata** — description, topics, homepage URL from the repository
   settings. Note if any are empty.
8. **Recent commits** — last 10. Note the date of the most recent commit
   to establish whether the project looks alive.

Do not read files beyond this list unless the README explicitly references
a setup guide in another path (e.g. `docs/DEVELOPMENT.md`). Read that one
file if referenced.

## Step 2 — Score each dimension

Evaluate each of the five dimensions below. For each, produce:
- **Verdict**: PASS, NEEDS WORK, or MISSING
- **Evidence**: one sentence citing the specific file, label, or data point

If `skip_dimensions` input lists a dimension, note it as SKIPPED in the
scorecard and do not evaluate it.

### 2.1 CLARITY

- Does README explain what the project does in a way a newcomer understands
  within 60 seconds of reading?
- Is there a one-liner description in the repo metadata (About field)?
- Are there screenshots, a demo link, or a code example above the fold
  (before any table of contents or installation section)?
- Is the project's target audience stated explicitly?

Verdict guide:
- PASS: all four present.
- NEEDS WORK: one or two missing.
- MISSING: no README or README does not explain what the project does.

### 2.2 ENTRY POINTS

- Are there issues labeled `good first issue` (or equivalent)?
- Are those issues recent (created or updated within the last 60 days)
  and actually actionable (description explains what to do, not just
  a title)?
- Is there a CONTRIBUTING.md?
- Does CONTRIBUTING.md explain how to find work to do, not just how to
  submit a PR?

Verdict guide:
- PASS: CONTRIBUTING.md exists with find-work guidance AND at least one
  actionable good-first-issue under 60 days old.
- NEEDS WORK: CONTRIBUTING.md exists but is thin, or good-first-issues
  are stale or have no description.
- MISSING: no CONTRIBUTING.md and no labeled entry-point issues.

### 2.3 SETUP FRICTION

- Are local development setup steps documented (in README, CONTRIBUTING.md,
  or a referenced setup doc)?
- Are the steps complete enough to follow cold: dependencies, environment
  variables, how to run tests?
- Is there a Makefile, justfile, docker-compose, or script that reduces
  setup to one command?
- Are common setup failure modes or platform-specific notes mentioned?

Verdict guide:
- PASS: documented steps that a newcomer could follow from a fresh clone,
  plus a one-command shortcut.
- NEEDS WORK: steps exist but are incomplete, outdated, or assume prior
  knowledge not stated.
- MISSING: no setup documentation found.

### 2.4 RESPONSIVENESS SIGNALS

- What is the median time to first response on the last 10 external PRs?
  (An external PR is one whose author is not listed as a maintainer or
  org member. If you cannot determine this, note it.)
- Are there unreviewed external PRs older than 14 days?
- Is the repo alive? (Most recent commit within the last 90 days.)
- Is there a stated response-time expectation anywhere (README,
  CONTRIBUTING.md, issue templates)?

Verdict guide:
- PASS: median first-response under 48 hours, no external PRs unreviewed
  past 14 days, recent activity.
- NEEDS WORK: some responses but slow (over 48 hours median) or a few
  stale unreviewed PRs.
- MISSING: no external PRs reviewed in the sample, or repo appears
  inactive (last commit over 90 days ago).

### 2.5 CONVERSION ASK

- Does the repo ask anything of interested visitors? (Star, sponsor,
  join community, try the product.)
- Is there a FUNDING.yml, GitHub Sponsors link, or Open Collective
  mention?
- Is there a community link: Discord, Slack, GitHub Discussions,
  or similar?
- Is there a "star this repo if you find it useful" or equivalent
  nudge in the README?

Verdict guide:
- PASS: at least two of the four present.
- NEEDS WORK: one present.
- MISSING: none present.

## Step 3 — Generate the fix list

For each NEEDS WORK or MISSING verdict, generate one or more fix entries.
Each fix must include:

1. **Priority** (1 = highest impact on a first-time contributor)
2. **Dimension** it belongs to
3. **Problem**: what is missing or broken, citing the specific file or
   absence of a file
4. **Fix**: the exact change to make. Name the file, the section, the
   content to add or change. Do not give generic advice like "improve
   your README". Give specific advice like "Add a ## Prerequisites
   section before ## Installation in README.md listing Python 3.11+
   and pip".
5. **Effort estimate**: "5 minutes", "30 minutes", "one afternoon"

Rank all fixes by impact on a cold first-time contributor's experience,
not by how easy they are to implement.

Cap the list at 10 fixes. If fewer apply, list only what applies.

If `focus` input was provided, weight fixes in that area higher in the
ranking (move them up, do not inflate their count).

## Step 4 — Pick the one thing to do today

From the fix list, select the single item that:
- Has the highest impact on first-time contributor experience, AND
- Can be completed in under 30 minutes

State it as a direct instruction, not a suggestion. Name the file,
the section, and the exact change.

## Step 5 — Write the report

Write `reports/OSS_CONTRIBUTOR_AUDIT.md` with exactly these sections:

    ## Summary scorecard

    | Dimension | Verdict | Evidence |
    |---|---|---|
    | Clarity | ... | ... |
    | Entry points | ... | ... |
    | Setup friction | ... | ... |
    | Responsiveness | ... | ... |
    | Conversion ask | ... | ... |

    ## Prioritized fix list

    ### 1. [Short title]

    **Dimension:** ...
    **Problem:** ...
    **Fix:** ...
    **Effort:** ...

    ### 2. [Short title]

    ...

    ## One thing to do today

    [Direct instruction — one specific change, one specific file, under 30 minutes.]

If all five dimensions pass, write the scorecard with all verdicts and
evidence, then write:

> No critical fixes found. Optional improvements:

followed by up to 3 low-effort upgrade suggestions.

If the repository is empty or has only one file, write a minimal report
noting insufficient content to audit and list what is missing as
Priority 1 through N.

Do not write any file other than `reports/OSS_CONTRIBUTOR_AUDIT.md`.
Do not open pull requests, modify the repository, post or send anything.
