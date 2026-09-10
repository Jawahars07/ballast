# Provenance

## Everything here is self-authored

All eleven skills in this repository were written by me, Jawahar Naidu Nettem, for my own daily
use, before any of it was intended for publication. None of it is a fork, a rename, or a
lightly-edited copy of someone else's skill.

Each one exists because something went wrong and needed a rule. Most of the "mistakes that have
actually happened" sections are literal — those are my mistakes, generalised.

| Skill | Origin |
|---|---|
| `jd-analyser` | Written after I built a specialist CV for a role that existed to *train* someone. The requirements block was four lines and did not mention the tool in the job title |
| `human-voice` | Written after a cover letter went out sounding like a model wrote it. Part 2 was added later, when I realised CVs have a different tell — rhythm, not vocabulary |
| `cv-tailor` | The document build, extracted once I had rebuilt the same LaTeX CV by hand too many times |
| `application-package` | The orchestrator that keeps the other three in the right order |
| `job-triage` | Written after three roles in a row matched my profile on responsibilities and were disqualified by a requirement I had not read |
| `apply-prefill` | An auto-submit path was added to my own system once. I removed it the next day. The halt list is what remains |
| `security-vet` | Written the day I nearly installed a plugin from a one-month-old anonymous account |
| `github-publish` | Written after finding a live API key in a project directory that was queued for publishing |
| `heavy-build-protocol` | Distilled from the sessions where large builds went well, and the ones where they did not |
| `council` | My own decision framework. The five advisor names are deliberate |
| `wiki-sync` | Written after a tracker sat frozen for ten weeks because nobody logged the sessions that changed it |

## What is deliberately NOT here, and why

I use a lot of third-party skills. They are good, and they are not mine to redistribute.
Publishing a re-fork of someone else's work under my own name is the thing this file exists to
make impossible.

Excluded, with credit to the people who wrote them:

| Skill(s) | Author |
|---|---|
| `claude-api`, `docx`, `pdf`, `pptx`, `xlsx`, `skill-creator`, `frontend-design`, `brand-guidelines`, `canvas-design`, `mcp-builder`, `webapp-testing` | [anthropics/skills](https://github.com/anthropics/skills) |
| ~50 engineering and business pattern skills | [wshobson/agents](https://github.com/wshobson/agents) |
| `resume-diagnoser`, `resume-recruiter`, `resume-rewriter`, `resume-hiring-manager` | [espinosacodes/dreamJob](https://github.com/espinosacodes/dreamJob) |
| `agent-self-evaluation`, `article-writing`, `context-budget`, `continuous-learning-v2`, `deep-research`, `eval-harness`, `market-research` | [affaan-m/ECC](https://github.com/affaan-m/everything-claude-code) |
| `graphify` | [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) |
| `caveman` | [juliusbrussee/caveman](https://github.com/juliusbrussee/caveman) |
| `skill-vetter` | [useai-pro/openclaw-skills-security](https://github.com/useai-pro/openclaw-skills-security) |
| `web-search` | [brave/brave-search-skills](https://github.com/brave/brave-search-skills) |
| `r3f-best-practices` | [emalorenzo/three-agent-skills](https://github.com/emalorenzo/three-agent-skills) |
| `subagent-driven-development` | [obra/superpowers](https://github.com/obra/superpowers) |

A note on that list. While preparing this repository I found `graphify` recorded in my own lock
file as locally authored. It is not — it is Graphify-Labs' work, installed as a tool, and my
lock file entry was wrong. I corrected it rather than shipping it. That check is the reason this
file exists, and it is why I would rather publish eleven skills that are genuinely mine than
thirty that are mostly other people's.

## Two acknowledgements

The `career-forge` skills grew alongside a job-pipeline system I adapted from someone else's
work. **None of that system is in this repository** — no scripts, no templates, no
configuration. What ships here is the reasoning: how to read a job description, how to score it
honestly, how to keep a document from claiming something you cannot defend. The LaTeX templates
in `cv-tailor/assets/` were written fresh for this repository.

The AI-tell lists in `human-voice` draw on public documentation of signs of AI writing and on
independent word-frequency research. The compilation, the CV-specific list, and the rhythm
analysis in Part 2 are mine.

## Verifying this claim

I would rather you check than trust me:

- Every skill's history is in this repository's git log
- The mistakes described in each skill are specific and consistent with each other, which is
  hard to fake and easy to test — read two skills and see whether they describe the same
  workspace
- If you believe any of this is derived from your work, open an issue. I will credit it or
  remove it, and I would rather be corrected in public than be wrong quietly
