## What this changes

<!-- One or two sentences. -->

## The failure behind it

<!-- If this adds or changes a rule: what went wrong that made it necessary?
     A rule without a real failure behind it will usually be declined. See CONTRIBUTING.md. -->

## What you tested it against

<!-- "Ran jd-analyser on three postings, two of which previously scored wrong" beats
     "looks right". Say which tool and version you ran it in. -->

## Checklist

- [ ] No executable file, symlink, or dependency manifest added under `skills/`
- [ ] No personal data, and no real company names in worked examples
- [ ] Any new skill has a `SKILL.md` with kebab-case `name` matching its directory, and a
      `description` that names the trigger situations
- [ ] Any new skill is listed in a plugin's `skills` array in `.claude-plugin/marketplace.json`
- [ ] `CHANGELOG.md` updated under Unreleased
