---
name: internal-share-narrative
description: Writes narrative content for internal company talks in four modes: a technical-talk skeleton (context, pain points, constraints, approach, outcome, reflection), a project retrospective built around one turning point, a section-by-section speaker script with Q&A prep from existing slides or docs, and a commercial pitch narrative (one-sentence purpose, problem / solution, why now, bottom-up market, traction, unit economics, competition, the ask) with an objection-handling Q&A. Use when the user asks to "help me structure this internal talk", "write a talk outline", "turn this project into a story worth telling", "convert this material into a speaker script", "help me write a business pitch", "structure an investor narrative", "write a business case story for the board", or "how should I frame this retrospective". Text output only: no visual layout, no HTML or PPTX, no invented data, and no conclusions drawn on the user's behalf.
version: 1.3.0
agent_created: true
---

# Internal Tech Talk Narrative

Turn a loose topic, a pile of material, or a project experience into a **script that can be read aloud as-is**. The deliverable is Markdown, with no visual design whatsoever. Write everything in English.

Mode D adds commercial pitches and business cases. It produces narrative text only; the deck layout for it belongs to `fintech-presentation` in pitch mode.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| Talk outline, speaker script, retrospective story, Q&A prep, pitch narrative | This Skill |
| Compress a large body of material into a scrolling long-read HTML + PDF | `html-longread-story` |
| Turn a batch of conference PDFs into PPTX + HTML deck | `conference-pdf-to-deck` |
| A clickable single-file presentation deck, including a pitch deck | `fintech-presentation` |
| A system architecture or solution design diagram | `solution-architecture-diagram` |

If the user says "write a story" or "story material" but the context clearly points to a long-read HTML page or slides, confirm once against the table above before starting. Do not grab the work.

## 2. Input

Ask for all four at once, and only for what is missing:

1. **Topic**: what this talk is actually about, in one sentence.
2. **Audience**: peer engineers / cross-functional colleagues / leadership / investors / board. Sets terminology density and background.
3. **Duration**: minutes, and whether Q&A is included. Sets structural granularity.
4. **Existing material**: slides, docs, data, meeting notes. With none, build from scratch in mode A.

For **Mode D**, also confirm the **decision being asked for** (capital, sponsorship, approval, budget release) and whether a framework applies. Framework orders are documented in `fintech-presentation/references/pitch-frameworks.md`; use them as an ordering aid only, never as a licence to add claims. If the user supplies an outline, keep its order.

## 3. Pick the mode

| Signal | Mode |
|---|---|
| Only a topic or an idea, no draft material | A · Technical talk skeleton |
| "What we did", "what went wrong", "looking back", "retrospective" | B · Project retrospective story |
| Existing PPT / doc / outline that needs a script to present from | C · Material-to-script |
| "Pitch", "business case", "investor", "board approval", "the ask", "TAM" | D · Commercial pitch narrative |
| Several signals at once, or a vague "help me prepare a talk" | Ask one question; do not guess |

Ask exactly one short question: "Would you like a speaker script, a retrospective, a narrative skeleton, or a pitch narrative first?" Do not fire a list of questions.

Once decided, state the mode and its output shape before starting. All four templates and worked examples are in `@references/narrative-modes.md`; load only the section for the chosen mode.

## 4. General rules

- **Never invent numbers**. For any figure, ratio, or duration the user has not supplied, write `TBD: <what is needed>`. Do not plug in illustrative values.
- **Separate fact, inference, and recommendation**: copy supplied facts verbatim; prefix anything inferred with "Inference:"; prefix proposed actions with "Recommendation:". Never blend inference into fact.
- **Do not draw conclusions for the user**. If the material contains no "so we should", do not write one; it can be listed as an open question in the Q&A prep.
- **Duration conversion**: English delivery is estimated at **130–150 words per minute**; deck slides at 1.5–2 minutes each; Q&A is budgeted separately and never eats into body time.
- **Audience calibration**: peer engineers keep terminology and implementation detail; cross-functional audiences get one extra layer of background and compressed implementation detail; leadership decks move technical detail to an appendix, leaving only the problem, the decision, and the result in the body. Investor and board audiences get the answer first and the evidence after it.
- **Redaction**: on encountering real customer names, amounts, internal system codenames, or unpublished metrics, stop and ask the user to confirm. Do not copy the original text into the output.
- **Mode D only**: the company purpose is one declarative sentence; market size states its denominator; competition includes "do nothing"; the weakest point is addressed on its own section; the ask states amount, use and milestone; the close repeats the thesis. Never write "this proves" or "the results show".

## 5. Output contract

A single Markdown file (or a complete in-chat response), with this fixed order. Mark missing sections "N/A" rather than deleting them:

```markdown
# <Topic>
- Mode: A / B / C / D
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

Section names vary by mode; see the template file. Speaker scripts must be complete sentences that can be read aloud — no outline-style placeholders such as "expand on this here". In Mode D the "Likely Questions" table becomes an objection-handling table and must include at least one hard commercial objection.

## 6. The four modes in brief

**A · Technical talk skeleton**: context → pain points → constraints → approach → outcome → reflection and next steps. The constraints section is the one most often dropped, and it is what makes the approach credible; it must stand as its own section.

**B · Project retrospective story**: setting → key roles → turning point → the decision at the time → result and cost → reusable lessons. Write exactly one turning point; do not produce a chronological log.

**C · Material-to-script**: a speaker script per slide or section, with page numbers and timings, plus transitions and Q&A prep. Do not reorder the source, and do not drop source sections. If the order looks wrong, note it once at the end — never adjust it unilaterally.

**D · Commercial pitch narrative**: one-sentence purpose → problem → solution → why now → market (bottom-up) → traction and unit economics → business model → competition → team → the ask. Write action titles as full sentences, keep the strongest proof early, and reserve a section for the weakest point. Q&A prep becomes objection handling with an explicit source / TBD column. The narrative must not contain framework names or third-party branding.

Full section templates, time budgets, and worked examples are in `@references/narrative-modes.md`.

## 7. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write a Markdown file | Give the same structure in full in-chat, marked "not saved to disk" |
| Source is a scan or image with unreadable text | List the unreadable parts and ask for a text version; do not guess |
| Key data missing so the narrative thread breaks | Stop at "Open Items"; do not write a full script with inferred numbers |
| Material appears to contain non-public information | Stop, list the specific items, and ask for redaction or an abstract restatement |
| User also wants slides or layout | Deliver text only and point to `fintech-presentation` (pitch mode for Mode D) for the visual layer |
| User asks for a non-English script | Keep the same structure in the requested language; no mixed-language output |
| Mode D with no verified figures | Write `TBD` in every figure position; never use an illustrative number as if measured |

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
- [ ] **Mode D**: one-sentence purpose, denominator for market size, "do nothing" alternative, weakest point section, ask with amount / use / milestone
- [ ] **Mode D**: no framework or third-party names in the narrative, and no invented figures

## 9. Handover

Give the file path (if any), mode, word count, estimated duration, and open-item count, then one line:

"Once the open items are confirmed the content is final; hand it to `fintech-presentation` to lay it out as a deck."

Do not put production notes, status labels, or template comments inside the script itself.
