# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

For skills, semantic versioning is read as: **major** = a rule changed such that the skill now
produces materially different output; **minor** = a new skill or a new rule; **patch** = wording,
examples, and corrections.

## [Unreleased]

### Added

- **`job-triage` gains a mission-block screen.** Two categories that no stated requirement
  catches, and that no CV can work around: **authorship as the deliverable** (editorial copy,
  newsletters, white papers, press relations) and **design tooling as the deliverable**
  (infographics, high-fidelity mockups, design systems, video editing). These are read from the
  *mission* block rather than the requirements block, which is a deliberate and deliberately
  narrow exception to the rule that responsibilities are never requirements: the question is not
  "can this candidate be hired" but "could they do the day job at all". One posting that stated
  no language requirement whatsoever was still unbuildable because the deliverable was editorial
  copy in a language the candidate does not write.
- **`job-triage` gains a portfolio gate** — a required portfolio, book, or prior placement in a
  craft the candidate has never practised.

### Changed

- **`jd-analyser` returns one score again: `MATCH`.** The two-score FIT/EDGE model shipped three
  days earlier is withdrawn. It was over-engineered and it answered the wrong question.
  - **MATCH scores the candidate against the job description and nothing else**, computed from
    **TESTABLE requirements only** — things an employer can screen on with a fact or a document.
    Unfalsifiable traits (*dynamic, proactive, team player, high potential*) are listed, marked
    `S`, and excluded from the arithmetic. **This part was the real fix and it stays.**
  - When fewer than three requirements are testable, the skill says the score reflects very
    little rather than presenting it as meaningful.
  - **EDGE is gone.** It folded four things into a number: employer history, target-role fit, bar
    selectivity, and warm contacts. **Three of those four are not properties of the job
    description at all.** Averaging them into a score made the output hard to read and stopped
    the number answering the one question it was asked.
  - Those factors now appear in **Step 5 as plain flags, in words** — bar softness, employer
    history (weighted by how far the candidate got, since repeated CV-screen rejections are a
    negative), target-role fit, warm path, unknowns, differentiated asset. The recommendation is
    one sentence of judgement naming the flag that drove it, not a lookup in a matrix.
  - Worked examples rewritten: five roles, all with a high MATCH, each decided by a different
    flag. The table makes the point that the score was never the interesting column.

## [1.0.0] - 2026-09-11

First public release. Eleven skills, three plugins.

### Added

- **`career-forge`** (6 skills)
  - `jd-analyser` — sorts every line of a posting into BAR / WORK / BONUS / NOISE, scores from
    the requirements block only, and applies six hard gates that cap the score and stop the
    process
  - `human-voice` — removes AI tells from anything sent under a real person's name. Part 1 for
    prose, Part 2 for CVs, where the tell is rhythm rather than vocabulary
  - `cv-tailor` — one-page LaTeX CV and cover letter, compiled with `tectonic`, verified the way
    an ATS parser reads a PDF. Ships two templates
  - `application-package` — orchestrates the above into a tracked package, and stops before
    submission
  - `job-triage` — screens a list of postings on the requirements block before anything reaches
    a short-list
  - `apply-prefill` — fills application forms in a browser and never clicks Submit. Ten named
    halts that fire rather than guess
- **`ship-safe`** (2 skills)
  - `security-vet` — recon, static scan, written verdict, user approval, then an install method
    proportionate to the assessed risk
  - `github-publish` — secret sweep including full git history, hardening, then verification of
    the pushed remote
- **`deep-work`** (3 skills)
  - `heavy-build-protocol` — eight phases numbered from zero, each with an observable check
  - `council` — five perspectives, anonymised and shuffled, then synthesised
  - `wiki-sync` — session close that records the lesson rather than the changelog
- `examples/` — `facts.md`, `profile.yml` and `applications.tsv` scaffolding
- CI that fails the build on any executable file, executable bit, symlink, unexpected file type,
  dependency manifest, secret in tree or history, personal data, un-anonymised company name,
  non-conforming `SKILL.md`, orphaned skill, missing community document, or broken README link

### Security

- Zero executable surface. `skills/` holds only `.md`, `.tex`, and `.json`, and this is enforced
  in CI rather than asserted in prose
- GitHub Actions pinned to full commit SHAs rather than moving tags
- Workflow permissions restricted to `contents: read`
