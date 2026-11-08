---
name: internal-share-narrative
description: Writes narrative content for internal company tech talks in three modes: a technical-talk skeleton (context, pain points, constraints, approach, outcome, reflection), a project retrospective built around one turning point, and a section-by-section speaker script with Q&A prep from existing slides or docs. Use when the user asks to "help me structure this internal talk", "write a talk outline", "turn this project into a story worth telling", "convert this material into a speaker script", or "how should I frame this retrospective". Text output only: no visual layout, no HTML or PPTX, no invented data, and no conclusions drawn on the user's behalf.
version: 1.2.0
agent_created: true
---

# Internal Tech Talk Narrative

Turn a loose topic, a pile of material, or a project experience into a **script that can be read aloud as-is**. The deliverable is Markdown, with no visual design whatsoever. Write everything in English.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| Talk outline, speaker script, retrospective story, Q&A prep | This Skill |
| Compress a large body of material into a scrolling long-read HTML + PDF | `html-longread-story` |
| Turn a batch of conference PDFs into PPTX + HTML deck | `conference-pdf-to-deck` |
| A clickable single-file presentation deck | `fintech-presentation` |
| A system architecture or solution design diagram | `solution-architecture-diagram` |

If the user says "write a story" or "story material" but the context clearly points to a long-read HTML page or slides, confirm once against the table above before starting. Do not grab the work.

## 2. Input

Ask for all four at once, and only for what is missing:

1. **Topic**: what this talk is actually about, in one sentence.
2. **Audience**: peer engineers / cross-functional colleagues / leadership. Sets terminology density and background.
3. **Duration**: minutes, and whether Q&A is included. Sets structural granularity.
4. **Existing material**: slides, docs, data, meeting notes. With none, build from scratch in mode A.

## 3. Pick the mode

| Signal | Mode |
|---|---|
| Only a topic or an idea, no draft material | A · Technical talk skeleton |
| "What we did", "what went wrong", "looking back", "retrospective" | B · Project retrospective story |
| Existing PPT / doc / outline that needs a script to present from | C · Material-to-script |
| Several signals at once, or a vague "help me prepare a talk" | Ask one question; do not guess |

Ask exactly one short question: "Would you like a speaker script, a retrospective, or a narrative skeleton first?" Do not fire a list of questions.

Once decided, state the mode and its output shape before starting. All three templates and worked examples are in `@references/narrative-modes.md`; load only the section for the chosen mode.

## 4. General rules

- **Never invent numbers**. For any figure, ratio, or duration the user has not supplied, write `TBD: <what is needed>`. Do not plug in illustrative values.
- **Separate fact, inference, and recommendation**: copy supplied facts verbatim; prefix anything inferred with "Inference:"; prefix proposed actions with "Recommendation:". Never blend inference into fact.
- **Do not draw conclusions for the user**. If the material contains no "so we should", do not write one; it can be listed as an open question in the Q&A prep.
- **Duration conversion**: English delivery is estimated at **130–150 words per minute**; deck slides at 1.5–2 minutes each; Q&A is budgeted separately and never eats into body time.
- **Audience calibration**: peer engineers keep terminology and implementation detail; cross-functional audiences get one extra layer of background and compressed implementation detail; leadership decks move technical detail to an appendix, leaving only the problem, the decision, and the result in the body.
- **Redaction**: on encountering real customer names, amounts, internal system codenames, or unpublished metrics, stop and ask the user to confirm. Do not copy the original text into the output.

## 5. Output contract

A single Markdown file (or a complete in-chat response), with this fixed order. Mark missing sections "N/A" rather than deleting them:

```markdown
# <Topic>
- Mode: A / B / C
- Audience: <audience>
- Duration: <n> minutes (Q&A included / excluded, <n> minutes)
- Core message: <one sentence>

## Narrative Thread
<3-6 sentences explaining why this order works>

## Sections
### 1. <section name> · <suggested duration>
- Points to cover:
- Speaker script:
- Supporting material:

(repeat for each section)

## Transitions
<one line between each pair of sections, listed individually>

## Likely Questions
| Question | Answer points | Source / TBD |
|---|---|---|

## Open Items
- TBD: <what exactly is missing, and who to ask>

## Fact and Inference Log
- Fact: <items from the user's material>
- Inference: <structural inferences added by this Skill>
```

Section names vary by mode; see the template file. Speaker scripts must be complete sentences that can be read aloud — no outline-style placeholders such as "expand on this here".

## 6. The three modes in brief

**A · Technical talk skeleton**: context → pain points → constraints → approach → outcome → reflection and next steps. The constraints section is the one most often dropped, and it is what makes the approach credible; it must stand as its own section.

**B · Project retrospective story**: setting → key roles → turning point → the decision at the time → result and cost → reusable lessons. Write exactly one turning point; do not produce a chronological log.

**C · Material-to-script**: a speaker script per slide or section, with page numbers and timings, plus transitions and Q&A prep. Do not reorder the source, and do not drop source sections. If the order looks wrong, note it once at the end — never adjust it unilaterally.

Full section templates, time budgets, and worked examples are in `@references/narrative-modes.md`.

## 7. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write a Markdown file | Give the same structure in full in-chat, marked "not saved to disk" |
| Source is a scan or image with unreadable text | List the unreadable parts and ask for a text version; do not guess |
| Key data missing so the narrative thread breaks | Stop at "Open Items"; do not write a full script with inferred numbers |
| Material appears to contain non-public information | Stop, list the specific items, and ask for redaction or an abstract restatement |
| User also wants slides or layout | Deliver text only and point to `fintech-presentation` for the visual layer |
| User asks for a non-English script | Keep the same structure in the requested language; no mixed-language output |

## 8. Pre-delivery checklist

- [ ] Mode choice is evidenced or confirmed with the user
- [ ] All output sections present; missing ones marked "N/A"
- [ ] Speaker scripts are complete sentences with no outline placeholders
- [ ] Word count matches the duration (130–150 words/minute)
- [ ] Every number has a source; missing ones sit in "Open Items"
- [ ] Facts, inferences, and recommendations are separately labelled
- [ ] No conclusions supplied on the user's behalf; no "this shows that" / "the results prove" phrasing
- [ ] Transition count = section count − 1
- [ ] At least 3 Q&A items, including 1 hard question
- [ ] No real customer names, unpublished metrics, or internal system codenames
- [ ] Output is consistently English with consistent terminology

## 9. Handover

Give the file path (if any), mode, word count, estimated duration, and open-item count, then one line:

"Once the open items are confirmed the content is final; hand it to `fintech-presentation` to lay it out as a deck."

Do not put production notes, status labels, or template comments inside the script itself.
