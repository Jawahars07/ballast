# Ballast

**Ballast doesn't make the ship faster. It stops it going over.**

Eleven agent skills that encode **when not to do something**.

[![validate](https://github.com/Jawahars07/ballast/actions/workflows/validate.yml/badge.svg)](https://github.com/Jawahars07/ballast/actions/workflows/validate.yml)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-open%20standard-000)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Zero executable surface](https://img.shields.io/badge/executable%20surface-zero-brightgreen)](SECURITY.md)
[![Self-authored](https://img.shields.io/badge/provenance-100%25%20self--authored-blueviolet)](PROVENANCE.md)

Most agent skills add a capability. These add a check. Each one exists because I did something
wrong, and the rule is what stopped it happening twice.

They follow the [Agent Skills open standard](https://agentskills.io), so they work in Claude
Code, Codex, Cursor, Gemini CLI, Copilot, OpenCode, Goose and ~40 other tools. **Install takes
one command** — see [Install](#install).

**Pure markdown. No scripts, no dependencies, no network calls, nothing to execute.**
[Enforced in CI](.github/workflows/validate.yml), not just promised. See [SECURITY.md](SECURITY.md).

---

## The idea

An agent that can do more is not the same as an agent that decides better.

Give a model a job description and it will confidently write you a CV for a role you cannot
hold. Not because it is stupid, but because job descriptions are written to blur exactly that
line, and nobody told it where the line is.

So these skills are mostly rules about restraint:

- Read the **requirements** block, never the responsibilities
- Claim at the depth your evidence supports, not the depth the posting wants
- If it is not in your facts file, it does not go on the page
- Fill the form, never click Submit
- Say "1 page, 0 hidden characters, 4 of 4 terms backed" — never "should be fine"

---

## Three plugins

### `career-forge` — read a JD honestly, then write documents that survive scrutiny

| Skill | What it does |
|---|---|
| **`jd-analyser`** | Sorts every line of a posting into **BAR** (what they require) · **WORK** (what you'd do) · **BONUS** · **NOISE**. Scores from BAR only. Six hard gates that cap the score at 2.0 and stop the process |
| **`human-voice`** | Removes the AI tells. Part 1 for prose, Part 2 for CVs — because the CV tell is **rhythm**, not vocabulary, and everyone optimises for the wrong one |
| **`cv-tailor`** | One-page LaTeX CV and cover letter, compiled with `tectonic`, verified the way an ATS parser actually reads a PDF |
| **`application-package`** | Runs the three above in order and produces a tracked package. Stops before submission |
| **`job-triage`** | Screens a list of postings on the requirements block before anything reaches your short-list |
| **`apply-prefill`** | Fills application forms in your browser. **Never clicks Submit.** Ten named halts that fire rather than guess |

**The insight this plugin is built around:** three roles in a row matched my profile perfectly on
responsibilities and were each disqualified by a requirement I had not read. Responsibilities
describe the job. Requirements decide whether you can hold it. Almost every application tool
optimises against the wrong half of the page.

**The one that catches people:** if a posting says *"we will support your skills development on
X"*, then X is a **training subject, not a requirement** — even when X is in the job title. They
are hiring to train. I once built a specialist CV for a role like that and got called delusional,
correctly.

### `ship-safe` — two gates before something becomes irreversible

| Skill | What it does |
|---|---|
| **`security-vet`** | Recon → static scan → written verdict (SAFE / SAFE WITH CAVEATS / HOLD) → **your approval** → an install method proportionate to the risk. Reputable gets version-pinned. Anonymous gets vendored and commit-pinned |
| **`github-publish`** | Sweeps for secrets **including the full git history**, hardens what the sweep exposes, strips copyrighted and bloat files, then verifies the pushed remote instead of assuming |

An agent skill is instructions a model will follow **with your tools and your credentials**. A
bad one needs no exploit — it needs you to install it and then ask for something. That is a
different risk shape from a normal dependency and deserves its own gate.

Yes, you should run `security-vet` on this repository. That is the intended use.

### `deep-work` — discipline for tasks too big to hold in your head

| Skill | What it does |
|---|---|
| **`heavy-build-protocol`** | Seven phases: **frame · recon · plan · gate · build · verify · report · log**. Phase 0 requires you to name the observable check *before* the first tool call. If you cannot name it, you do not understand the task yet |
| **`council`** | Five advisors — strategy, execution, finance, risk, contrarian — run in parallel, get **anonymised and shuffled**, then a judge synthesises. "Divided" is a valid verdict and is never manufactured away |
| **`wiki-sync`** | Session close. Writes the *lesson*, not the changelog, into a markdown knowledge base so the next session starts informed |

**Two rules from `heavy-build-protocol` that earn their keep everywhere:**

> **Two failed attempts on the same approach means stop.** Change approach or go read how a
> project you respect solved it. Research beats thrashing.

> **Compiling is not working.** Run the real flow and watch it. Name the check in your report.

---

## Install

### Any tool that supports the Agent Skills standard

The standard location is `~/.agents/skills/`. Tools discover skills there automatically.

```bash
git clone https://github.com/Jawahars07/ballast.git
mkdir -p ~/.agents/skills
cp -R ballast/skills/* ~/.agents/skills/
```

To scope it to one project instead of your whole machine, use the repo-level location:

```bash
mkdir -p .agents/skills && cp -R /path/to/ballast/skills/* .agents/skills/
```

Prefer `cp` over a symlink. Some tools do not follow symlinks, and copying means you can read
exactly what you installed.

### Claude Code

As a plugin marketplace, which keeps the three plugins separate and updatable:

```
/plugin marketplace add Jawahars07/ballast
/plugin install career-forge@ballast
/plugin install ship-safe@ballast
/plugin install deep-work@ballast
```

Or drop the skills in directly:

```bash
cp -R ballast/skills/* ~/.claude/skills/
```

### Per-tool notes

| Tool | Reads |
|---|---|
| **Claude Code** | the marketplace above, or `~/.claude/skills/` · [docs](https://code.claude.com/docs/en/skills) |
| **Codex CLI** | `.agents/skills/` (repo) then `~/.agents/skills/` (user) · [docs](https://developers.openai.com/codex/skills/) |
| **Cursor** | [docs](https://cursor.com/docs/context/skills) |
| **Gemini CLI** | [docs](https://geminicli.com/docs/cli/skills/) |
| **GitHub Copilot / VS Code** | [docs](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| **OpenCode** | [docs](https://opencode.ai/docs/skills/) |
| **Goose** | [docs](https://block.github.io/goose/docs/guides/context-engineering/using-skills/) |
| **Factory Droid** | [docs](https://docs.factory.ai/cli/configuration/skills) |
| **OpenClaw** | [docs](https://docs.openclaw.ai/tools/skills) |
| **Kiro, Amp, Roo Code, Letta, Trae, and ~30 more** | [full client list](https://agentskills.io/clients) |

I have verified the `~/.agents/skills/` path and the Claude Code paths myself. For the rest, the
linked docs are authoritative — if your tool puts skills somewhere else, follow its
documentation rather than this table, and please open an issue so I can correct it.

### Take only what you want

Each skill is a self-contained folder. There is no shared code, so copying one directory works:

```bash
cp -R ballast/skills/jd-analyser ~/.agents/skills/
```

### Then, for `career-forge` only

The career skills read two files you own. Nothing works without them, and that is deliberate —
it is what makes fabrication structurally impossible rather than merely discouraged.

```bash
cp examples/facts.md examples/profile.yml examples/applications.tsv .
```

Fill `facts.md` with things that are true. Its **Evidence Index** is binding: a skill marked
*never claim* never appears in a document, no matter what a posting asks for.

`cv-tailor` additionally needs two binaries:

```bash
brew install tectonic poppler        # macOS
```

`ship-safe` and `deep-work` need nothing at all.

### Updating

```bash
cd ballast && git pull && cp -R skills/* ~/.agents/skills/
```

Pin to a release tag rather than tracking `main` if you would rather audit once and freeze:
`git checkout v1.0.0`.

## Provenance

**All eleven skills are mine.** No forks, no renames, no lightly-edited copies.

[PROVENANCE.md](PROVENANCE.md) also lists the third-party skills I use daily and **deliberately
did not republish**, with credit to their authors. While writing it I found one skill recorded in
my own lock file as self-authored that was actually [Graphify-Labs'](https://github.com/Graphify-Labs/graphify)
work. I corrected the record and left it out.

I would rather ship eleven skills that are genuinely mine than thirty that are mostly other
people's.

---

## Honest limits

- **`human-voice` does not prove human authorship.** Commercial AI detectors are unreliable in
  both directions and should not be used as a gate. This removes known lexical and structural
  markers at near-zero cost. Whether an AI-sounding CV is *why* a given application was rejected
  is unproven — rejection reasons are almost never disclosed.
- **`council` gives you five framings from one model, not five independent minds.** Correlated
  blind spots stay blind.
- **`jd-analyser`'s scoring formula is a heuristic**, not a calibrated model. The value is the
  BAR/WORK separation and the hard gates. The number is a conversation-starter.
- **`apply-prefill` will be blocked** by anti-bot protection on major boards. Fall back to
  guiding a human click by click.

---

## The name

Ballast is dead weight you carry on purpose. It adds nothing to top speed and it costs you
cargo space, and without it the first serious crosswind puts the mast in the water.

That is what these skills are. None of them make an agent more capable. Every one of them makes
it harder to do the confident, fast, wrong thing.

MIT licensed. Built in Paris. Issues and PRs welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

If you use these and something in them is wrong, I would genuinely like to know.
