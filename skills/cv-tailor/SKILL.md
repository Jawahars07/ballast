---
name: cv-tailor
description: Build a tailored one-page LaTeX CV and matching cover letter from a job description, compile them with tectonic, and verify the PDF the way an ATS parser reads it. Use for any resume, CV, cover letter, LaTeX, or ATS document request. Never fabricates a fact that is not in the facts file.
---

# CV Tailor

Turns a job description into a one-page LaTeX CV and a matching cover letter, compiled and
verified. Every claim traces to `facts.md`.

## Prerequisites

- **`tectonic`** — the LaTeX engine. `brew install tectonic`, or see tectonic-typesetting.github.io.
  It is a single binary with no TeX Live install, and it runs with shell-escape disabled, which
  is why this skill uses it rather than `pdflatex`.
- **`pdftotext`** (poppler) — for the ATS verification pass. `brew install poppler`.
- **`facts.md` and `profile.yml`** in the working directory. Scaffold them from `examples/`
  if missing. Do not proceed without them.

## Load first, every build

1. **The `jd-analyser` output.** Build from its positioning brief, never from your own read of
   the JD. Skipping this is how a trainee role gets mis-built as a specialist role.
2. **`facts.md`**, including its Evidence Index and every caveat in it.
3. **`profile.yml`** for constraints and exclusions.
4. **The `human-voice` skill — load it, every build, before writing a single bullet.** It covers
   CVs as well as prose. Working from memory is the documented failure mode here, not a
   theoretical one.

## Non-negotiables

- **Never fabricate.** Only facts present in `facts.md`. No invented metrics, employers, or dates.
- **Never write a gap-disclosure sentence into the CV or cover letter.** Gaps get tracked
  internally, in the report, for the user's own judgement. They never appear on the page the
  employer reads. When a JD exposes a real gap, the fix is to find a genuine stronger proof
  point, not to write an apologetic paragraph.
- **Never submit.** This skill does not click Apply or Send, ever.
- **One page** unless the user explicitly asks otherwise.
- **No hidden text.** No white text, no zero-opacity layers, no off-page keyword stuffing.
  Current ATS platforms flag this as fraud, and it will end the application.
- **Compile with `tectonic`.** Never `\uppercase` inside `titlesec` — it breaks under tectonic.
  Use `\scshape`.
- **Result over template.** Every bullet needs a real outcome and a real number where `facts.md`
  supports one. What is *not* required is that every bullet wear the same grammar. Applying one
  sentence formula literally across a whole role produces exactly the uniform cadence that reads
  as machine-written. **Within each role, at least one bullet must break the shape.**
- **Keyword placement, not just presence.** The JD's top two or three terms belong in the profile
  line and the first bullet of the most relevant role. ATS scoring weights that placement higher
  than the same keyword buried in the last line of page one.

## Workflow

1. **Quick score first.** One line, out of 5, with the reason. Wait for a go-ahead before building.
2. **Run `jd-analyser`.** Take its positioning brief as the spec.
3. **Copy the template**, do not start from scratch:
   ```bash
   cp assets/cv-template.tex             cv-<company-slug>.tex
   cp assets/cover-letter-template.tex   cl-<company-slug>.tex
   ```
4. **Fill from `facts.md`.** Mirror the JD's exact wording only where real evidence backs it.
   Mark anything unsupported as a gap in your report to the user; never invent evidence to close one.
5. **Compile:**
   ```bash
   tectonic cv-<company-slug>.tex
   tectonic cl-<company-slug>.tex
   ```
6. **Verify** with the pass below. Never ship a document that fails it.
7. **Report** exact filenames, compiler result, each verification result, and any honest gaps.
   State the numbers: "1 page, all six headings found, 0 hidden characters, 4/4 JD terms backed."

## ATS verification pass

An ATS does not see your layout. It sees `pdftotext` output. Verify against that, not against
how the PDF looks.

```bash
PDF=cv-<company-slug>.pdf

# 0. Page count — must be 1
pdfinfo "$PDF" | grep '^Pages:'

# 1. Extraction must not be empty (a CV built from images scores zero)
pdftotext "$PDF" - | wc -w

# 2. Name, email and phone must appear in the first few lines
pdftotext "$PDF" - | head -5

# 3. Standard headings present and in order
pdftotext "$PDF" - | grep -inE '^(education|experience|projects|skills|languages|certifications)'

# 4. Hidden or zero-width characters — must be 0
pdftotext "$PDF" - | grep -cP '[\x{200B}-\x{200D}\x{FEFF}\x{00AD}]'

# 5. Layout vs plain extraction must agree in word count (a big gap means
#    multi-column or table layout the parser will scramble)
A=$(pdftotext "$PDF" - | wc -w); B=$(pdftotext -layout "$PDF" - | wc -w)
echo "plain=$A layout=$B delta=$((A-B))"

# 6. No language claimed above the level recorded in facts.md
pdftotext "$PDF" - | grep -inE '(fluent|bilingual|native|C1|C2|B2)'
```

Then run the `human-voice` Part 2 pre-ship scan on the same PDF.

**Read the matches, never the exit code.** `grep | sort -u` exits 0 on empty input, so chaining
`&& echo FAIL` fires either way. Look at the output.

**Interpreting check 5:** a large delta means the parser is reading your columns as interleaved
text. Tab-aligned dates are fine for text search but can confuse portal autofill widgets that
infer structured fields from layout. If a portal is Workday- or SuccessFactors-shaped, consider
supplying a plain `.docx` alongside the PDF, with dates stacked rather than tab-aligned.

## Design pattern

The bundled template follows the conventions that survive ATS parsing:

- `lmodern`, single column, no tables in the body, no text boxes, no headers or footers
- Contact row hyperlinked, but never a bare URL printed as visible text
- Small-caps section headings with a rule underneath, standard heading names
- Dark, mid, and light greys only — colour that survives a black-and-white print
- Tight but not cramped spacing; one page is a constraint, not a target to overflow

Edit the template's variables at the top. Do not restructure the body unless you have a reason
you can state.

## Assets

- `assets/cv-template.tex` — one-page CV skeleton
- `assets/cover-letter-template.tex` — matching cover letter

Both are plain LaTeX source. Read them before running them, as you should with any file that
goes through a typesetter.
