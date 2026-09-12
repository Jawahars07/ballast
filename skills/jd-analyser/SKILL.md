---
name: jd-analyser
description: Split a job description into what it actually REQUIRES versus what it merely describes, separate testable requirements from unfalsifiable personality traits, and return two scores - FIT (can you clear the stated bar) and EDGE (are you worth picking over the pile). Use the moment a posting arrives, before writing a line of any document. Prevents reading responsibilities as requirements, prevents over-claiming, and prevents a soft bar from scoring like a good opportunity.
---

# JD Analyser

**When to use:** the moment a job description arrives — pasted, URL, or screenshot — and before
any CV or cover-letter skill writes a line.

## Why this exists

A Power BI apprenticeship was once read wrong. Its *mission* bullets said "take ownership of
Power BI developments of increasing complexity", so a Power-BI-specialist CV was drafted with
DAX and data-modelling framing. The JD's actual requirement list was four lines long and did
not mention Power BI at all. Its first mission line read: *"we will support your skills
development on the Power BI tool."*

They were hiring to train. The candidate caught it and called the draft delusional.

Reading responsibilities as requirements inflates the CV, invites interview questions you
cannot defend, and mis-positions you against the role. This skill exists to make that
mistake structurally impossible.

---

## Step 0 — Quick score first

Before anything else, output one line:

`**<Company> — <Role>. FIT X/5 · EDGE Y/5.** <one-sentence reason>`

Never give a single fused number. A single number is what made this skill stop discriminating.

Then run the analysis. Do not build any document until the user gives a go-ahead. A fast,
honest verdict is worth more to them than a slow, thorough one they did not ask for yet.

## Step 1 — Load ground truth

1. **`facts.md`** — the only source of claimable facts. Its **Evidence Index** carries
   per-skill caveats, and those caveats are binding. If it says *"SharePoint — not verified,
   never claim"*, then SharePoint never appears, no matter what the JD wants.
2. **`profile.yml`** — location, duration, exclusions, deal-breakers, granted exceptions.
3. Any wider knowledge base, only if the JD touches something `facts.md` does not cover.

If neither file exists, say so and offer to scaffold them from `examples/` before continuing.
Do not proceed on guesses about the user's background.

## Step 2 — Bucket every line of the JD

Sort each statement into exactly one bucket. This is the core of the skill.

| Bucket | What it is | Headings it usually sits under | Counts toward score? |
|---|---|---|---|
| **BAR** | What you must already have to be considered | "Requirements", "Your profile", "Qualifications", "Profil recherché", "Type d'études" | **Yes — this is the score** |
| **WORK** | What you will do *after* being hired | "Mission", "Responsibilities", "Objectives", "You will join…" | No — never a requirement |
| **BONUS** | Explicitly optional | "nice to have", "a plus", "ideally", "familiarity with", "highly valued" | Tiebreaker only |
| **NOISE** | Employer branding | headcount, ESG targets, "why join us", awards, training-hours stats | No |

**Training-signal override.** If the JD says it will teach a skill — *"we will support your
skills development on X"*, *"you will learn"*, *"training provided"*, *"opportunity to develop
skills in X"* — then **X is WORK, not BAR**, even when X appears in the job title. Flag it
explicitly in the output. This is the single most common misread in the entire process.

**Study-path lines are BAR.** Postings often name target schools or degree types. Treat these
as a real signal about the intended profile, not decoration.

## Step 2.5 — Hard gates

A hard gate is a requirement no amount of CV craft can close. Check all six, in order.
If any FAILS the analysis is over: the score is capped at **2.0**, the recommendation is
automatically **don't build**, and you say which gate failed in one sentence.

Do not spend paragraphs arguing around a failed gate. Do not let strong evidence elsewhere
talk you past it.

| Gate | Fails when |
|---|---|
| **Degree field** | JD names a field the candidate does not hold (chemistry, law, pharmacy, design) |
| **Specialised school** | JD names a school type they are not in, as an exclusive requirement |
| **Stated language** | JD explicitly requires a language above the candidate's stated level |
| **Hard tool bar** | A named tool is a listed *requirement* and `facts.md` has no evidence for it |
| **Location** | Outside the commutable zone, with no exception granted in `profile.yml` |
| **Work authorisation** | Requires nationality, clearance, or status the candidate lacks |

**Contract duration is NOT a hard gate.** A shorter-than-preferred contract is a flag and a
demotion, never an automatic no. That is the user's call, not the filter's.

### The stated-language rule, in both directions

Read the language bar **only as written**, and apply it symmetrically. Never infer an
unstated requirement from the posting's own language or the company's country.

- JD states English only, written in English → write English, do not hedge about the local language.
- JD states "intermediate English", written entirely in French → still just English. Do not
  invent a French bar from the fact that the posting is in French.
- JD states French proficiency → that is a real gate. Fail it honestly.

### The title trap

The job title is marketing copy; the responsibilities define the domain. Read the
responsibilities before trusting the title.

One posting titled *"Analytical Development & AI"* read like a data role. Its responsibilities —
*"correlations between the structural characteristics of molecules and multi-technique
analytical profiles"* — made it analytical **chemistry**. Always ask what the word means
**in this industry**.

## Step 3 — Map BAR to evidence, and mark each item TESTABLE or SOFT

One row per BAR item. No exceptions, no merging two requirements into one row.

| # | BAR item (quoted from JD) | T/S | Evidence in facts.md (with line ref) | Verdict |
|---|---|---|---|---|

**T = TESTABLE.** An employer could screen someone out on it using a fact or a document: a
degree field, a stated language level, a named tool, a certification, years of experience, a
specific deliverable. These are the items that actually sort candidates.

**S = SOFT.** A trait no application can falsify and no recruiter screens on at CV stage:
*dynamic · rigorous · proactive · team player · good communicator · organised · methodical ·
high potential · entrepreneurial spirit · curious · takes initiative · comfortable working
autonomously · strong appetite for X.*

**Only TESTABLE items are scored.** List the soft ones, mark them S, and leave them out of the
arithmetic entirely.

Why this exists: three postings analysed in the same week all returned 5.0, because their stated
requirements were almost entirely personality traits. One was an exact-archetype role at a
company whose stack the candidate used daily. One was an out-of-archetype support internship
whose every mission verb was *assist*, *participate*, *help*. A formula that scores those
identically is not measuring anything a person can act on.

Four verdicts, and only these four:

- **EXCEEDS** — evidence is clearly above what is asked
- **MEETS** — evidence matches what is asked
- **PARTIAL** — related evidence exists at lower depth or scale; say exactly how it falls short
- **MISSING** — nothing in `facts.md` backs it

## Step 4 — FIT: can they clear the stated bar?

If any Step 2.5 gate failed, FIT is **2.0**. Stop here, do not compute.

Otherwise, over **TESTABLE BAR items only**:

```
raw = mean(EXCEEDS = 1.15, MEETS = 1.0, PARTIAL = 0.5, MISSING = 0) × 5
FIT = min(raw, 5.0)
```

Then adjust by at most ±0.5 for BONUS coverage. Never let a WORK bullet or a SOFT item move FIT.

EXCEEDS is worth more than MEETS on purpose. Clearing a bar with room to spare is a real
advantage, and an earlier version scored both at 1.0, which made EXCEEDS decorative.

**Always report `raw` when it exceeds 5.0**, as `FIT 5.0 (raw 5.4, saturated)`. Saturation is
information. It means the stated bar sits well below this candidate's level, which usually
signals a large applicant pool rather than a great opportunity.

**If fewer than 3 BAR items are TESTABLE, FIT is not meaningful.** Say so in one line —
*"only 1 of 5 stated requirements is testable, so FIT does not discriminate here"* — and let
EDGE carry the decision.

## Step 4.5 — EDGE: is this worth the hours?

FIT answers whether the bar is clearable. It cannot answer whether to spend six hours building
the package, because a bar that everyone clears produces a 5.0 and three hundred applicants.

Four components, 0–1.25 each, summed. Max 5.0.

| Component | 1.25 | 0.6 | 0.2 |
|---|---|---|---|
| **Differentiated asset** | Something they built or wrote that names this company or its exact problem | A strong shipped project that maps to the role | Coursework only, nothing an employer could not find elsewhere |
| **Archetype fit** | A **primary** target role in `profile.yml` | **secondary** or **adjacent** | outside every listed archetype |
| **Bar selectivity** | A demanding bar they clear that removes most applicants — the bar itself is the moat | Mixed: at least one requirement does real screening | Everyone who reads it clears it, no moat |
| **Warm path** | Named contact, prior interview, or referral | Prior application to this company | Cold portal only |

Selectivity measures **how many applicants the bar removes**, not how testable it is. A bar can be perfectly testable and still remove nobody: *basic project management* and *knows Trello* are checkable and almost universal.

**Bar selectivity is deliberately inverted against intuition.** A hard bar you clear is *good
news*, because it removes competitors. A soft bar you clear is *weak news*, because it removes
nobody. A single-score formula gets this exactly backwards: the easier the posting, the higher
it scores.

## Step 4.6 — The decision, from both numbers

| FIT | EDGE | Call |
|---|---|---|
| < 3.5 | any | **Don't build.** The stated bar cannot be defended |
| ≥ 3.5 | ≥ 3.5 | **Build now.** Highest priority |
| ≥ 3.5 | 2.0–3.5 | **Build if the pipeline is thin.** Real but not differentiated |
| ≥ 3.5 | < 2.0 | **Skip unless desperate.** They clear the bar and so does everyone; nothing sorts them |

Report both, always, as `FIT 4.8 / EDGE 4.7`. One number hides the trade-off that matters.

## Step 5 — Deal-breaker check

Gates are already resolved in Step 2.5. Here, report the non-gating constraints from
`profile.yml`: contract duration against the preferred length, start date, contract type.

If location or duration is **absent** from the posting, say so explicitly and ask. Never
assume the city. Never assume the length.

## Step 6 — Positioning brief

Output these four lines. Every downstream document skill builds from them, not from its own
read of the JD.

- **Lead with:** the BAR items scored EXCEEDS or MEETS — these are why they qualify
- **Mention once:** BONUS-matching evidence, factually, at `facts.md`'s stated depth, no persona
- **Do not claim:** every `facts.md` caveat this particular JD tempts, named explicitly
- **Archetype:** one phrase (e.g. "engineering student who already ships dashboards"), never a
  seniority the JD did not ask for

## Anti-inflation rules

1. **Never build a persona out of WORK bullets.** They describe the job, not the candidate.
2. **Claim at `facts.md`'s depth, not the JD's.** If the file says "working proficiency, not a
   DBA", the CV says working proficiency. The JD wanting more does not change the evidence.
3. **A low bar is good news — say so.** Clearing four modest requirements comfortably is a
   stronger and more defensible position than a stretched claim to seniority. Do not inflate a
   trainee role into a specialist role to make the fit look better.
4. **One line per real skill.** If a tool is a genuine differentiator it earns one honest line,
   not a reordered skills grid, a rewritten headline, and three bullets.
5. **Anything not in `facts.md` does not go on the CV.** No exceptions, ever.

## Output shape

```
**<Company> — <Role>. FIT X/5 · EDGE Y/5.** <one-line reason>

### BAR (what they actually require)       ← table from Step 3, each row marked T or S
### WORK (what you'd do, not requirements)  ← bulleted, training-signals flagged
### BONUS / NOISE                           ← one line each
### FIT                                     ← from TESTABLE rows only; show raw if saturated
### EDGE                                    ← the four components, one line each
### Deal-breakers                           ← or "none / unknown: <what>"
### Positioning brief                       ← the four lines from Step 6
### Recommendation                          ← from the Step 4.6 matrix
```

## Worked examples

Four consecutive real reads, anonymised. Note that three of the four were killed by a gate.

**Company A — Power BI Apprentice. 4.5/5. BUILD.**
BAR = 4 items: apprentice-level student, *basic* IT concepts, Office especially Excel,
proficient English → EXCEEDS, EXCEEDS, MEETS, MEETS. No gate failed. Training signal present
("we will support your skills development on the Power BI tool"), so **Power BI is WORK, not
BAR** — the entire job title is a training subject. Lead on the four requirements. Power BI
gets one honest line at the depth `facts.md` records. Do not claim SharePoint.

**Company B — E-commerce & Content Creation Trainee. 2.5/5. DON'T BUILD.**
Two gates failed. Hard tool bar: Adobe Photoshop and Illustrator listed as requirements, and
`facts.md` records Adobe as an aspiration, not experience. Specialised school: the posting
named four design and photography schools. A genuine multimedia coursework module lifted it off
the floor but does not clear Adobe.

**Company C — Digital Marketing Media Trader Apprentice. 2.0/5. DON'T BUILD.**
Stated-language gate failed: *"Proficiency in French, oral and written"*, against an A1 level.
The role briefs local agencies on local-market campaigns daily, so the requirement is
load-bearing, not administrative. The responsibilities matched the candidate's experience
well — which is exactly the trap this skill exists to catch.

**Company D — Analytical Development & AI Apprentice. 2.0/5. DON'T BUILD.**
Degree-field gate failed: the JD required a masters in chemistry or biochemistry. Title trap
fired. The language rule held in the other direction: the posting is written in French but
states only "intermediate English", so no French bar was invented. Duration (12 months) was
flagged as a demotion, not a gate.

## Worked examples, second set — why FIT and EDGE had to be split

These three were analysed within two days of each other and every one returned **5.0** under the
old single-score formula. They are not equivalent opportunities, and the split says so.

**All three still show FIT 5.0.** That is not a failure of the fix, it is the finding. In this
market the stated bars are overwhelmingly soft, so FIT saturates almost everywhere and was never
the number worth reading. EDGE separates them cleanly: **3.70 / 3.55 / 0.65**.

**Company E — AI Builder, agency digital and tech team. FIT 5.0 (raw 5.38, only 2 of 6 testable) / EDGE 3.70. BUILD NOW.**
Six BAR items and only **two are testable**: the degree field, and translating a business need
into a concrete solution. *Appetite for AI*, *process logic*, *rigour and autonomy* and *good
communication* are all SOFT, so FIT is flagged non-discriminating here too.
EDGE carries it: differentiated asset **1.25**, because the candidate had published an
open-source agent-skills project four days earlier and the posting's mission bullet read
*"configure, test and document agents"*; archetype primary **1.25**; bar selectivity **1.0**,
since the degree-field requirement genuinely screens; warm path **0.2**, cold portal.
`1.25 + 1.25 + 1.0 + 0.2 = 3.70` → BUILD NOW.

**Company F — Sourcing Category Manager. FIT 5.0 (raw 5.75, only 1 of 5 testable) / EDGE 3.55. BUILD.**
Only **1 of 5** BAR items is testable, that one being student status. The rest were *dynamic,
organised, methodical, proactive, takes initiative, high potential, entrepreneurial spirit* —
all SOFT. FIT is reported and explicitly flagged as non-discriminating, and the decision rests
entirely on EDGE: differentiated asset **1.25**, because facts.md recorded a category analysis
whose deliverable had been written for a manager at this exact company; archetype secondary
**0.8**; bar selectivity **0.25**; warm path **1.25** from a named contact in an earlier
application. `1.25 + 0.8 + 0.25 + 1.25 = 3.55` → BUILD.
**A high EDGE on a meaningless FIT is still a build**, for a completely different reason than
Company E.

**Company G — Project coordination internship, IT operations. FIT 5.0 (raw 5.56, 4 of 7 testable) / EDGE 0.65. SKIP UNLESS THIN.**
Four testable items, all cleared comfortably: student status, a stated English bar the candidate
exceeded, *"**basic** project management skills"* against a real PM foundations certificate, and
project tooling against two tools already in facts.md.

This is the **most testable bar of the three**, and the lowest EDGE, which is the whole point:
testable is not the same as selective. EDGE collapses it: differentiated asset **0.2**, archetype
**0.0** (outside every target role), bar selectivity **0.25** (*basic* project management and
*knows Trello* remove nobody), warm path **0.2**.
`0.2 + 0.0 + 0.25 + 0.2 = 0.65` → SKIP UNLESS THIN. Every mission verb was *participate*,
*assist*, *help*, and one was *write the meeting minutes*. **They clear the bar, and so does
everyone else.** Under the old formula this scored identically to Company E, which is the bug
that forced this rewrite.

## The pattern to watch

Three consecutive rejections above were disqualified in the **requirements** block while their
**responsibilities** matched the candidate well. That is the signature failure mode of most job
searches: postings that read like a fit and are closed by a gate you did not check.

Screen the requirements block first. Every time.

The second pattern: **a soft bar is a warning, not an invitation.** When a posting's stated
requirements are all personality traits, the CV has to do all the sorting, because the
requirements do none of it. Those roles need the most differentiated package you can build, or
they need skipping. What they never need is a confident 5.0.
