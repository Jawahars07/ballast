# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

For skills, semantic versioning is read as: **major** = a rule changed such that the skill now
produces materially different output; **minor** = a new skill or a new rule; **patch** = wording,
examples, and corrections.

## [Unreleased]

### Fixed

- `heavy-build-protocol` described its loop as seven phases while listing eight. The loop is
  numbered from zero because phase 0 happens before the first tool call; it is now described
  as eight phases numbered from zero, consistently across the skill, the README, the changelog
  and the plugin description
- `PROVENANCE.md` and the README said "no renames", which was ambiguous: three skills were
  renamed from their private names when published (`fable-protocol` to `heavy-build-protocol`,
  `career-latex-documents` to `cv-tailor`, `auto-apply` to `apply-prefill`). The claim being
  made is that none is a re-badged copy of someone else's work, and both files now say that
  and list the renames

### Security

- Branch protection active on `main` via a repository ruleset: pull request required, `validate`
  must pass and be up to date, force-push and deletion blocked

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
