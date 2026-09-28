# Guide — job applications with `career-forge`, in any AI tool

From a pasted job description to a scored decision, a tailored one-page CV, a cover letter, and a
report card on both. Set up once (about an hour, most of it writing your facts). After that, each
application is a few minutes of your attention.

**Contents:** [1. Set up your facts](#1-set-up-your-facts-once) ·
[2. Install for your tool](#2-install-for-your-tool) ·
[3. The everyday loop](#3-the-everyday-loop) ·
[4. Prompts to copy](#4-prompts-to-copy) ·
[5. Reading the output](#5-reading-the-output) · [6. Troubleshooting](#6-troubleshooting)

---

## 1. Set up your facts (once)

Everything depends on two files you own. The skills refuse to invent anything, so a thin facts
file means thin CVs.

1. Download [`examples/facts.md`](examples/facts.md) and [`examples/profile.yml`](examples/profile.yml).
2. **`facts.md`** — every real role, project, degree, certificate and number. In the Evidence
   Index, write **where** you used each skill ("Power BI — weekly client dashboards, Agency X,
   2024"), not just that you have it. A skill used in a dated role scores 1.0; a bare list entry
   scores 0.6; an unrecorded skill scores 0. This is the single biggest lever on your scores.
3. **`profile.yml`** — where you can work, your language levels (CEFR, honestly), target roles,
   roles you refuse, tools you must never claim.

Keep both files somewhere you can reach from every tool you use.

## 2. Install for your tool

| Tool | Scores JDs | Writes CV + cover letter | Compiles the PDF itself |
|---|---|---|---|
| Claude Code, Codex, Gemini CLI, Cursor, other coding agents | ✅ | ✅ | ✅ with `tectonic` installed |
| Claude desktop app / claude.ai | ✅ | ✅ | ⚠️ usually not — compile the `.tex` yourself (below) |
| ChatGPT Business / Enterprise / Edu | ✅ | ✅ | ⚠️ usually not — compile the `.tex` yourself |
| ChatGPT Free / Plus, any other chatbot | ✅ via a Project | ✅ via a Project | ❌ compile the `.tex` yourself |

**The fastest path is a terminal agent** (Claude Code or Codex): it reads your files, compiles,
and runs every ATS check for you. Chat apps do the thinking; you do the last compile step.

### Get the files

Either clone the repository:

```bash
git clone https://github.com/Jawahars07/ballast.git
```

or download the ready-made ZIPs from the
[latest release](https://github.com/Jawahars07/ballast/releases/latest) — one ZIP per skill,
already in the shape the upload screens expect.

The four you need for applications: **`jd-analyser`**, **`cv-tailor`**, **`human-voice`**,
**`application-package`**. The rest are optional.

### Claude Code

```
/plugin marketplace add Jawahars07/ballast
/plugin install career-forge@ballast
```

Then open Claude Code in the folder holding your `facts.md` and `profile.yml`. If you also use
the desktop app, skills uploaded there sync into Claude Code automatically when you sign in with
the same account.

### Claude desktop app and claude.ai (Free, Pro, Max, Team, Enterprise)

1. **Settings → Capabilities** → turn on **Code execution and file creation**. Skills need it.
2. **Customize → Skills** → **+** → **Create skill** → **Upload a skill**.
3. Upload `jd-analyser.zip`, then `cv-tailor.zip`, `human-voice.zip`, `application-package.zip`
   — one at a time. Toggle each on.
4. Create a **Project** called *Job applications* and add `facts.md` and `profile.yml` to its
   files. Start every application chat inside that Project.

### Codex (CLI, IDE extension, app)

```bash
mkdir -p ~/.agents/skills
cp -R ballast/skills/{jd-analyser,cv-tailor,human-voice,application-package} ~/.agents/skills/
```

Run Codex from the folder holding your `facts.md` and `profile.yml`.

### ChatGPT Business, Enterprise, Edu

Sidebar → **Plugins** → **Skills** tab → **Create** → **Upload from your computer**, and upload
the four ZIPs. ChatGPT scans each one before it is available. Then create a Project with
`facts.md` and `profile.yml` in its files. (Your workspace admin may need to allow skill uploads.)

### ChatGPT Free / Plus, and any chatbot without skills

Skills are not available on these plans, but the skills are plain text, so a Project works:

**Shortcut:** download `chat-project-kit.zip` from the
[latest release](https://github.com/Jawahars07/ballast/releases/latest). It holds every file below,
already renamed, plus `PROJECT-INSTRUCTIONS.txt`. Unzip it, replace the example `facts.md` and
`profile.yml` with yours, and upload everything to the Project. Or by hand:

1. Create a **Project** called *Job applications*.
2. Add these files to it: `facts.md`, `profile.yml`, and the four `SKILL.md` files (rename them
   `jd-analyser.md`, `cv-tailor.md`, `human-voice.md`, `application-package.md`), plus
   `cv-template.tex` and `cover-letter-template.tex` from `skills/cv-tailor/assets/`.
3. Paste this into the Project's instructions:

   > For every job description I paste, follow `jd-analyser.md` exactly and stop after the
   > one-line score and scorecard. Only when I say "build it", follow `application-package.md`,
   > which uses `cv-tailor.md` and `human-voice.md`. The only source of facts about me is
   > `facts.md`. Never state anything that is not in it. Output the CV and cover letter as
   > complete LaTeX files based on the two templates.

### Gemini CLI, Cursor, Copilot, OpenCode, Goose and others

Copy the skills into `~/.agents/skills/` as in the Codex step. See the per-tool table in the
[README](README.md#per-tool-notes) if your tool reads a different folder.

### Compiling a `.tex` yourself (chat apps)

- **No install:** paste the `.tex` into a new project on [Overleaf](https://www.overleaf.com),
  then in **Menu → Compiler** choose **XeLaTeX** (the templates need it), and download the PDF.
- **Locally:** `brew install tectonic poppler` on macOS, then `tectonic cv-company.tex`.

Then run the ATS checks: paste the PDF's text back into the chat and ask for the document
scorecard, or run the commands in [`cv-tailor`](skills/cv-tailor/SKILL.md) yourself.

## 3. The everyday loop

```
paste JD ──► FIT score + flags (1 min) ──► decide ──► "build it" ──► CV + CL + scorecard ──► fix ──► you submit
```

1. **Paste the job description.** You get one line first: FIT out of 100, band, confidence.
2. **Decide in a minute.** Under 50, or BLOCKED by a gate → skip it. That is the most valuable
   minute in the whole process.
3. **"Build it."** Tailored CV and cover letter, compiled and checked.
4. **Read the scorecard.** Apply the (up to three) rewrites it proposes, if they are right.
5. **Send it yourself.** Nothing here ever clicks Submit.

**Speed tips**

- **Triage in bulk.** Paste five postings at once and ask for the one-line score of each. Build
  only the best one or two.
- **Improve the facts, not the CV.** When a score says "unrecorded evidence", add it to
  `facts.md` once. Every future application benefits.
- **One chat per application** keeps each package clean.

## 4. Prompts to copy

| When | Say |
|---|---|
| New posting | `Analyse this JD:` + paste |
| Several postings | `Quick-score each of these, one line each, best first:` + paste all |
| Worth it | `Build it.` |
| Just a CV | `Tailor my CV to this JD, no cover letter.` |
| Check a CV you already have | `Give me the document scorecard for this CV against this JD.` + both |
| Raise the score honestly | `Which facts could I add to facts.md that this JD would reward? Ask me, don't assume.` |
| Before an interview | `From this report, what are the three hardest questions and my honest best answer to each?` |

## 5. Reading the output

**FIT / 100** — how well your real evidence answers this posting.

| Band | Means |
|---|---|
| 80–100 Strong | Build it |
| 65–79 Good | Build it; the flags decide priority |
| 50–64 Stretch | Only with a strong flag: a referral, or something you built for exactly this problem |
| under 50 Weak | Skip unless you have a warm contact |

- **BLOCKED** means a knock-out gate failed (language, degree field, location…). The number is
  still shown so you can see how close you were, but the answer is: don't build.
- **Confidence Low** means the posting said almost nothing testable. Trust the flags over the
  number.
- **Core-task gap** means the one thing they will expect you to own scored zero. Expect the
  interview to find it.

**Document scorecard** — the finished CV, read the way an ATS reads it.

- **Backed keyword coverage 80%+** is the target. It only counts terms your facts support.
- **Raw coverage** is the honest ceiling — the part of the posting you cannot answer.
- **Unbacked terms present** must be 0. Anything else means something was claimed that
  `facts.md` does not support.

## 6. Troubleshooting

| Problem | Fix |
|---|---|
| "facts.md not found" | Put it in the working folder (terminal tools) or the Project files (chat apps) |
| Every score is low | Your Evidence Index says *what*, not *where*. Add the role or project for each skill |
| Skill never triggers | Say its name: `Use jd-analyser on this JD` |
| Skills greyed out in Claude | Turn on Code execution in Settings → Capabilities |
| Overleaf compile error | Switch the compiler to XeLaTeX |
| CV spills onto two pages | Ask: `Cut to one page, dropping the least relevant bullet first` |
| Upload rejected | Upload the release ZIP unchanged; the folder name inside must match the skill name |
