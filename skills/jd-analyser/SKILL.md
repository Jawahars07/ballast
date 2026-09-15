---
name: jd-analyser
description: Split a job description into what it actually REQUIRES versus what it merely describes, then score the candidate against that JD alone - one number, MATCH out of 5, computed only from requirements an employer can actually test. Soft personality traits are listed but never scored. Everything that is not JD-match (employer history, target-role fit, warm contacts, unknowns) is reported as plain flags in words. Use the moment a posting arrives, before writing a line of any document.
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

`**<Company> — <Role>. MATCH X/5.**  <one-sentence reason>`

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

## Step 4 — MATCH: how well does the evidence answer this JD?

**One number. It scores the candidate against the job description, and nothing else.**

If any Step 2.5 gate failed, MATCH is **2.0**. Stop here, do not compute.
Otherwise, over **TESTABLE BAR items only**:

```
raw   = mean(EXCEEDS = 1.15, MEETS = 1.0, PARTIAL = 0.5, MISSING = 0) × 5
MATCH = min(raw, 5.0)
```

Then adjust by at most ±0.5 for BONUS coverage. Never let a WORK bullet or a SOFT item move MATCH.

EXCEEDS is worth more than MEETS on purpose. Clearing a bar with room to spare is a real
advantage, and an earlier version scored both at 1.0, which made EXCEEDS decorative.

**Show `raw` when it exceeds 5.0**, as `MATCH 5.0 (raw 5.4)`. A high raw means the stated bar
sits well below the candidate's level.

**If fewer than 3 BAR items are TESTABLE, say so in one line** — *"only 1 of 5 stated
requirements is testable, so this score reflects very little"* — and lean on the flags below.

### What MATCH deliberately does not include

An earlier version of this skill folded four things into a second score: company history,
archetype preference, how selective the bar was, and whether the candidate had a warm contact.
That was a mistake, and it made the output hard to read. **Three of those four are not properties
of the job description at all.** They are facts about the employer, about the candidate's own
preferences, and about their network.

Mixing them into a score meant the number stopped answering the only question it was asked:
*how well does this evidence answer this posting?*

They still matter. They belong in **Step 5 as plain flags**, in words, where a person can weigh
them instead of having them silently averaged.

## Step 5 — Flags: everything that matters but is not JD-match

Gates are already resolved in Step 2.5. State each of these in **one line**, as a fact, never as
a number. Omit any that do not apply.

| Flag | Say |
|---|---|
| **Bar softness** | If most BAR items are soft traits: *"the stated bar is mostly personality traits, so the CV has to do the sorting"* |
| **Company history** | Prior applications and, critically, **how far they got**. An interview is a positive. Repeated CV-screen rejections are a negative, and they compound |
| **Archetype** | Whether the role sits inside the target roles in `profile.yml`, or outside all of them |
| **Warm path** | A named contact, a referral, or nothing |
| **Duration / start / location** | Against stated preferences. If absent from the posting, **say so and ask** |
| **Differentiated asset** | Anything built that names this employer or its exact problem |

Then give the recommendation as **one sentence of judgement**, not a lookup:
*build now · build if the pipeline is thin · don't build · need info first* — and say which flag
drove it.

A high MATCH with bad flags is still a skip. A moderate MATCH with a warm contact and a
differentiated asset is still a build. That judgement is easier to make, and easier to argue
with, in words than inside a weighted average.

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
**<Company> — <Role>. MATCH X/5.**  <one-line reason>

### BAR (what they actually require)       ← table from Step 3, each row marked T or S
### WORK (what you'd do, not requirements)  ← bulleted, training-signals flagged
### BONUS / NOISE                           ← one line each
### Flags                                   ← Step 5, one line each, words not numbers
### Positioning brief                       ← the four lines from Step 6
### Recommendation                          ← one sentence, naming the flag that drove it
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

## Worked examples, second set — the soft-bar problem

Five postings analysed in five days. Under the *original* formula, which scored soft personality
traits as if they were testable, the first four all returned **5.0**. They were not equivalent
opportunities.

**All five still show a high MATCH.** That is the finding rather than a failure: in this market
the stated bars are overwhelmingly soft, so MATCH saturates almost everywhere. **When it
saturates, the flags decide** — which is exactly why they are written in words.

| Role | Testable | MATCH | The flag that decided it |
|---|---|---|---|
| Company E — AI Builder | 2 of 6 | 5.0 (raw 5.38) | **Differentiated asset.** An open-source agent-skills project published four days before applying, against a mission bullet reading *"configure, test and document agents"*. Primary archetype. **BUILD NOW** |
| Company F — Category Manager | 1 of 5 | 5.0 (raw 5.75) | **Warm path + asset.** A category analysis whose deliverable was written for a manager at this exact employer, plus a named contact from an earlier application. **BUILD** |
| Company G — Project coordination | 4 of 7 | 5.0 (raw 5.56) | **Archetype.** Outside every target role. Every mission verb is *participate, assist, help*; one is *write the meeting minutes*. Most testable bar of the five, least reason to want it. **SKIP UNLESS THIN** |
| Company H — Corporate development | 3 of 5 | 5.0 (raw 5.50) | **Company history.** Five prior applications, **four rejected at CV screen**. Their filter already holds the profile and has removed it four times. **SKIP** |
| Company I — Digital communications | 2 of 3 | 5.0 | **Mission block.** No language bar stated, but the deliverable is editorial copy in a language the candidate does not write. **DON'T BUILD** |

Read that table across and the point is hard to miss: **the score was never the interesting
column.** Four of the five were decided by something a score cannot see, which is why those
things belong in words rather than folded into a weighted average.

## The pattern to watch

Three consecutive rejections above were disqualified in the **requirements** block while their
**responsibilities** matched the candidate well. That is the signature failure mode of most job
searches: postings that read like a fit and are closed by a gate you did not check.

Screen the requirements block first. Every time.

The second pattern: **a soft bar is a warning, not an invitation.** When a posting's stated
requirements are all personality traits, the CV has to do all the sorting, because the
requirements do none of it. Those roles need the most differentiated package you can build, or
they need skipping. What they never need is a confident 5.0.
