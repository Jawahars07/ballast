# Security

## What is in this repository

Markdown, and two LaTeX templates.

There is no JavaScript, no Python, no shell script, no binary, and no dependency manifest inside
any plugin. There is nothing to `npm install`. There is no postinstall step, because there is no
install step at all beyond copying files.

This is enforced in CI, not just promised. `.github/workflows/validate.yml` fails the build if
any executable file, any executable bit, any symlink, any unexpected file type, or any
dependency manifest appears under `skills/`. GitHub Actions are pinned to full commit SHAs
rather than moving tags, and the workflow runs with `contents: read` and nothing more.

The only scripts in this repository are the GitHub Actions workflows themselves. Those run on
GitHub's infrastructure when the repository is built. They are never installed on your machine
and are not part of any plugin.

## Portability

These skills follow the [Agent Skills open standard](https://agentskills.io) and are plain
folders containing a `SKILL.md`. They carry no tool-specific code, so the same files behave the
same way in Claude Code, Codex, Cursor, Gemini CLI and every other conforming tool.

That also means the audit below is complete for every platform. There is no per-tool variant
with different behaviour.

## What each plugin can and cannot do

Skills are instructions your agent reads. They cannot do anything your agent could not already
do — but they can *tell it to*, which is exactly why you should read them. Here is the honest
accounting.

| Plugin | Reads | Writes | Network | Runs commands | Spawns subagents |
|---|---|---|---|---|---|
| **career-forge** | your `facts.md`, `profile.yml`, job postings you supply | CV/cover-letter `.tex` and `.pdf`, reports, tracker rows | opens job postings you point it at; `apply-prefill` drives your browser | `tectonic`, `pdftotext`, `pdfinfo`, `grep` | no |
| **ship-safe** | the project or package you ask it to vet | `SECURITY.md`, `.gitignore`, lock-file entries | fetches repo and registry pages for the component being vetted | `git`, `grep`, `find` | no |
| **deep-work** | your project files and knowledge base | your knowledge base | no | as the task requires | **yes** — `council` runs five parallel subagents |

Things no skill here does, anywhere: transmit your data to a third-party service, read
credentials, read your keychain, read your shell history, install anything, or update itself.

## Two behaviours worth knowing about

**`apply-prefill` drives a real browser** using your existing logged-in sessions. It fills job
application forms. It has one hard rule, stated at the top of the skill: it never clicks Submit,
Apply, or Send. It halts rather than guessing on any field it cannot answer from your verified
facts — including language level, salary, and any ambiguous dropdown. Read the skill before
using it.

**`council` spawns five parallel subagents.** That costs tokens. Nothing leaves your machine.

## Verify it yourself

Do not take the table above on faith. It is a small repository and you can check it in a minute:

```bash
git clone https://github.com/Jawahars07/ballast
cd ballast

# every file that ships in a plugin, and its type
find plugins -type f | xargs file

# anything executable at all
find plugins -type f -perm -u+x

# every URL any skill could reach
grep -rInE 'https?://' skills/

# every command any skill tells your agent to run
grep -rInE '^\s*(tectonic|pdftotext|pdfinfo|grep|git|find|awk|curl|wget|npm|pip)' skills/
```

Then read the SKILL.md files. They are prose. That is the whole point — you can audit an agent
skill by reading it, and you should.

## Installing this safely

Pin to a tag rather than tracking `main`, so a future commit cannot change what you already
audited:

```
/plugin marketplace add Jawahars07/ballast
```

Then check what you have against a release tag. If you want a stronger guarantee, fork the
repository and install from your fork. That is a reasonable thing to do with anyone's skills,
including these.

The `security-vet` skill in this repository describes this practice in full. Applying it to this
repository is encouraged, not taken personally.

## Reporting a vulnerability

Open a private security advisory through GitHub's **Security → Report a vulnerability** tab on
this repository. Please do not open a public issue for anything exploitable.

Expect an acknowledgement within 72 hours.

## Scope

In scope: anything in a SKILL.md that could cause an agent to leak data, run something
destructive, or make an irreversible change without consent. Prompt-injection surfaces in these
skills. Anything in the CI workflows.

Out of scope: what your agent does with instructions you gave it yourself, and the behaviour of
third-party tools these skills call (`tectonic`, `pdftotext`, your browser).
