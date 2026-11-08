# Three Narrative Modes: Templates and Samples

This file provides three ready-to-use section templates, a time budget, filled-in samples, and common pitfalls. The samples all use neutral, fictional topics, and the numbers only demonstrate the format; they stand for no real system or business.

## 0. Time Budget

| Total Duration | Q&A | Body | Number of Sections | Per Section |
|---|---|---|---|---|
| 15 min | 3 min | 12 min | 4 sections | 2–4 min |
| 30 min | 5 min | 25 min | 6 sections | 3–8 min |
| 45 min | 10 min | 35 min | 6–7 sections | 4–10 min |
| 60 min | 15 min | 45 min | 7–8 sections | 5–12 min |

The body word count is estimated at 130–150 words per minute. Leave one minute of slack each for the opening and the close; do not fill the slot completely. When the total exceeds 60 minutes, split it into two sessions rather than solving it by speeding up.

---

## Mode A · Technical Talk Narrative Skeleton

Use when: you have only one topic or idea, no draft material, and need to build a structure that explains it clearly.

### Section Template

| # | Section | Role | 30-min Suggestion |
|---|---|---|---|
| 1 | Context | Let people outside the project follow along | 3 min |
| 2 | Pain Points | Explain why the old setup could not hold up, with tangible consequences | 5 min |
| 3 | Constraints | What cannot be changed, chosen, or waited on | 4 min |
| 4 | Approach | How to choose and roll out under those constraints | 8 min |
| 5 | Outcome | Where the change shows up, and the measurement basis | 4 min |
| 6 | Reflection and Next Steps | What fell short and what is still missing | 1 min |

**The constraints section is the one most often left out.** Without constraints, the approach sounds like "that is just the obvious way to do it," and the audience cannot tell whether it applies to their own setting. Write at least three constraints: technical debt, organizational boundaries, and time or compliance limits.

### Filled-in Sample (Excerpt)

```markdown
# Batch Job Scheduling Overhaul
- Mode: A
- Audience: cross-functional colleagues, including non-technical roles
- Duration: 30 min (includes 5 min Q&A)
- Core message: Pulling scheduling out of the scripts buys us explainability, not raw performance.

## Narrative Thread
First, place batch processing within the overall pipeline, then spell out three concrete headaches caused by script-based scheduling.
Next, cover the three constraints we had to preserve, which explains why we chose centralized scheduling rather than rewriting the jobs.
Then walk through the changes in rollout order, show the before-and-after on one measurement basis, and close with what remains unsolved.

## Section Content
### 1. Context · 3 min
- Points to cover: where the batch pipeline sits; how many batches run per day; who depends on the results
- Speaker script: Let me start with why this matters to everyone here. Every day we run a set of scheduled data jobs,
  and the downstream reports and a few reconciliation steps all wait on their output. When everything works, nobody notices it,
  but a single delay holds up several teams the next morning. That is the layer I want to talk about today,
  and why we moved it out of the scripts.
- Supporting material: a pipeline location diagram (to be supplied by the architecture-diagram Skill)

### 3. Constraints · 4 min
- Points to cover: the jobs themselves cannot change; no downtime; only a two-hour nightly window
- Speaker script: Before anything else, let me be clear about what we could not touch. First, the business logic of dozens of jobs was out of scope;
  rewriting them carried too much risk. Second, the pipeline is in use all day, so there was no downtime window.
  Third, we only had a two-hour window at night to make changes. Those three facts ruled out a full rewrite,
  so the only option was a way to lift out the scheduling layer on its own.
- Supporting material: the three-item constraints list

### 5. Outcome · 4 min
- Points to cover: time to locate failures; number of reruns; measurement basis and reporting period
- Speaker script: The most visible change is how quickly we find the cause when something breaks. We used to stitch together a timeline from logs on several machines;
  now one place shows which job, which step, and how long it waited. The three figures here are sample numbers, all on the same reporting period and the same measurement basis;
  the exact values are to be supplied from the measured records in the appendix.
- Supporting material: to be supplied: three measured before-and-after values and the reporting period
```

Sample transition lines:
- Context → Pain Points: "It sounds like this has been running fine all along, so what actually hurts?"
- Pain Points → Constraints: "The problem is clear, but the scope we can change is smaller than you would think."
- Constraints → Approach: "Under those three limits, there were really only two paths left."
- Approach → Outcome: "After the change, we measured again using the same measurement basis."
- Outcome → Reflection: "The numbers look good, but a few things are still unresolved today."

---

## Mode B · Project Retrospective Story

Use when: you are telling "what we built, where we stumbled, and what we see in hindsight," with the focus on the decision-making process rather than a results report.

### Section Template

| # | Section | Role | 45-min Suggestion |
|---|---|---|---|
| 1 | Setting | Time, scope, parties involved, and the goal at the time | 4 min |
| 2 | Key Roles | Who owned what and who held the decision | 4 min |
| 3 | Turning Point | **Write only one**: the event that invalidated the original plan | 8 min |
| 4 | The Decision Then | What we knew, what options existed, and why we picked it | 10 min |
| 5 | Result and Cost | What we gained, what we paid, and any side effects | 6 min |
| 6 | Reusable Lessons | What would still hold for another team | 3 min |

**The most common failure is writing a retrospective as a chronological log.** The test: if the whole piece reads like a weekly status report once you strip out the names and dates, it is a chronological log. Organize everything around one turning point, and let the rest of the information appear only when it supports that turn.

### Filled-in Sample (Excerpt)

```markdown
# A Retrospective on a Cross-Team Interface Migration
- Mode: B
- Audience: peer engineers
- Duration: 45 min (includes 10 min Q&A)
- Core message: The integration test stalled not because the interface was wrong, but because the two sides had different default assumptions about retries.

## Narrative Thread
Do not tell it in chronological order. Start by recreating the day the integration test stalled, then look back at why each side designed things the way it did.
Finally, lay out the three paths available at the time and explain why we chose the most costly one.

### 3. Turning Point · 8 min
- Points to cover: what happened in the integration test; the debugging path; the real cause
- Speaker script: On day three of the integration test, a single request produced two records downstream. Our first thought was that the upstream had sent it twice,
  but after a full day of digging through call logs we confirmed it had sent only once. Only when we put the two timeout configurations side by side did it click:
  the upstream treated a timeout as a failure and retried itself, while the downstream treated a timeout as simply no response received, with the request still valid.
  Neither side had a bug; the default assumption had just never been written into the interface contract.
- Supporting material: to be supplied: a comparison of the timeout and retry settings at the time

### 4. The Decision Then · 10 min
- Points to cover: the three paths; the cost of each; the basis for the decision
- Speaker script: We had three paths. First, turn off retries on the upstream: smallest change, but it pushed all failure handling onto the caller.
  Second, add deduplication downstream: neither side has to change the contract, but we maintain a dedup table forever.
  Third, redefine the interface semantics and write an idempotency key into the contract, with changes on both sides. We chose the third, because it turned the assumption into an explicit agreement instead of leaving it to tacit understanding.
  The cost was two extra weeks on the migration.
- Supporting material: a cost comparison table for the three paths

### 5. Result and Cost · 6 min
- Points to cover: whether similar issues dropped afterward; what the two extra weeks bought; the new maintenance cost
- Speaker script: What is worth noting is that cross-team interface reviews gained one standing check: spell out the failure semantics and who owns retries.
  That is what those two weeks really bought. The cost is real too: the contract grew more complex,
  and new teams have one more layer to understand.
- Supporting material: to be supplied: the count of similar issues after the migration
```

---

## Mode C · Material to Speaker Script

Use when: you already have a PPT, document, or outline and want to "speak from the material." The core rule is **no reordering, no cutting, and no rewriting of the original meaning**.

### Section Template

Map one-to-one to the pages or sections of the source material; each section contains:

```markdown
### <Page Number> · <Source Material Title> · <Suggested Duration>
- Source material points: <copy verbatim or take a minimal extract; do not reword>
- Speaker script: <complete sentences you can read aloud, 2–5 sentences>
- Background to add: <terms the audience may not follow, one-line explanation; write "None" if there are none>
- Supporting material: <figure number / table number / demo action>
```

Distribute time evenly across pages, then fine-tune by information density, with no page under 40 seconds. If the source material has an ordering problem, note it separately in the "Order Suggestions" at the end; **do not adjust it on your own**.

### Filled-in Sample (Excerpt)

```markdown
# Data Platform Quarterly Briefing · Speaker Script
- Mode: C
- Audience: leadership
- Duration: 20 min (includes 5 min Q&A), 8 pages
- Core message: <reuse the source material conclusion; do not write a new one>

### P1 · Quarterly Overview · 1 min
- Source material points: three workstreams, two deliveries
- Speaker script: This page is the overall picture for the quarter. The three workstreams are onboarding, governance, and reliability,
  and the first two are already delivered. The next seven pages follow that order,
  with the last page reserved for what has not been closed out yet.
- Background to add: None
- Supporting material: P1 overview chart

### P4 · Governance Coverage · 3 min
- Source material points: measurement basis note, values at three points in time
- Speaker script: What this page most needs is a clear measurement basis. The denominator is the total number of objects in scope for governance,
  and it shifts each quarter as onboarding progresses, so the three points are not a simple like-for-like comparison.
  With that in mind, the curve shows the pace of coverage rollout, not a quality score. The exact values are sample numbers;
  see the measurement basis note below the chart. I will not read them out one by one.
- Background to add: "Governance coverage" here means both the metadata and the owner are registered
- Supporting material: P4 line chart, measurement basis note
```

---

## Q&A Prep

Fixed format, at least 3 items, and one of them must be a "hard question."

```markdown
| Question | Answer Points | Basis / To Be Supplied |
|---|---|---|
| Can this approach be copied directly to another team | State the preconditions where it holds, then name the parts that do not generalize | Inference: based on the three limits in the constraints section |
| What is the measurement basis for the numbers | Reporting period, denominator, whether retries are included | To be supplied: measured records |
| Why not choose a simpler approach | Go back to the constraints at the time; no after-the-fact rationalization | Fact: section 3 of the user material |
| What is the plan for the part that did not get done (hard question) | Acknowledge it is unfinished, give a timing and an owner, make no promises | To be supplied: next-step schedule and owner |
```

Do not delete the hard question, and do not invent a neat answer on the user's behalf; list it in the open items together with the basis it needs.

## Common Pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| Constraints section missing | The approach sounds obvious | Write at least three non-negotiable conditions |
| Retrospective turns into a status report | Listed by date, with no turning point | Keep only one turning point and organize everything else around it |
| Speaker script written as a bullet outline | Phrases like "expand on this here" | Every line must be a complete sentence you can read aloud |
| Transition lines missing | Hard jumps between sections | The number of transition lines equals the number of sections minus one |
| Numbers with no source | The script has figures but cannot answer where they came from | Always mark the basis or move them into open items |
| Leadership script packed with detail | Config parameters and class names show up in the body | Move detail to the appendix; keep only problem, decision, and result in the body |
| Treating "it worked well" as the conclusion | No cost, no unresolved items | Result and cost must appear together |
| Forgetting to set aside Q&A | The script already runs over once read through | Set aside Q&A first, then budget the body from the remaining time |
| Template fields read aloud | "to be supplied:" and "inference:" appear in the speaker script | Those belong only in lists and annotation areas, never in the speaker script |
| Inconsistent terminology / mixed-language output | Half the terms in one language and half in another | Keep one language throughout and give the full term on first use |

## Acceptance Checklist

- [ ] Section count matches the time budget table, and the body is not filled to the brim
- [ ] Every section has a speaker script you can read aloud
- [ ] Transition lines are complete
- [ ] Q&A includes one hard question
- [ ] Open items are specific about "who to ask for what"
- [ ] Fact / inference / recommendation are kept separate
- [ ] No illustrative numbers passed off as measured data
