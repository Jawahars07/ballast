# Contributing

Issues and pull requests are welcome. A few things that will make yours land faster.

## The bar for a new rule

Every rule in these skills traces to something that actually went wrong. If you want to add one,
say what it prevents. A rule with a real failure behind it is worth ten rules that sound
sensible.

"This would be better practice" is not enough. "This produced a wrong output, here is the case"
is.

## What will be rejected

- **Any executable file inside `plugins/`.** Scripts, binaries, dependency manifests. CI blocks
  these, and the block is deliberate — the security story of this repository is that there is
  nothing to run. Solve it with inline shell inside the skill markdown instead.
- **Anything that sends user data anywhere.** These skills read local files. That is the deal.
- **A copy of someone else's skill.** See `PROVENANCE.md`. If you have written something good,
  publish it under your own name and I will link to it.
- **Real company names in worked examples.** Anonymise them. CI checks this.
- **Personal data of any kind.** CI checks this too, across the full git history.

## Style

Skills are prose, written for a model that will follow them literally.

- **Be specific.** "Filter the results" is useless. "Drop anything already in `applications.tsv`
  at any status" is a rule.
- **State the failure mode**, not just the rule.
- **Give tables to things that are lists of cases**, and prose to things that are reasoning.
- **Say what the skill does not do.** The honest-limits sections are load-bearing. A skill that
  claims more than it delivers is worse than one that claims less.
- Descriptions in frontmatter decide whether a skill triggers at the right moment. Write them
  for retrieval: name the trigger situations, not just the capability.

## Before you open a PR

```bash
# the same checks CI runs
find plugins -type f -perm -u+x                  # must be empty
grep -rInE 'https?://' plugins/                  # every URL must be justified
python3 -c "import json;json.load(open('.claude-plugin/marketplace.json'))"
```

If you touched a skill, say in the PR what you tested it against. "Ran it on three postings"
beats "looks right".
