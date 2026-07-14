# skill-chain

A free agent skill that turns a repeated, multi-step workflow into a **skill chain**: a set of small skills that run in order, where each step's output feeds the next. Instead of asking AI to do a whole job in one giant prompt, you break it into steps you can review, swap, and rerun.

Built by [Kyle Chalmers](https://kclabs.ai) (KC AI Labs). I teach practical AI for people who run businesses on YouTube: **[@kylechalmersdataai](https://www.youtube.com/@kylechalmersdataai)**.

## What it does

Point it at a workflow you repeat and it walks you through the design, then scaffolds the skill files for you:

- Checks whether the workflow is even worth chaining (you repeat it, its steps feed each other, it varies each time, you want to review along the way).
- Breaks it into steps, and notes what each one produces and where you would review.
- Writes a one-page pipeline guide and stubs out a `SKILL.md` for each step.

You end with a map and a folder of skill stubs ready to fill in.

## What a "skill" is

A skill is a folder with a `SKILL.md` inside: a name, a description full of the phrases you would say to trigger it, and a workflow the agent follows. Your agent loads it on demand. That is all this repo is: one well-written `SKILL.md`.

## Install

### Claude Code, copy the folder (simplest)

```bash
git clone https://github.com/kyle-chalmers/skill-chain.git
cp -r skill-chain/skills/skill-chain ~/.claude/skills/
```

Then start Claude Code and say: `design a skill chain for <your workflow>`.

### Claude Code, as a plugin (one command)

This repo is also a Claude Code plugin marketplace. From inside Claude Code:

```
/plugin marketplace add kyle-chalmers/skill-chain
/plugin install skill-chain@skill-chain
```

Updates then come with `/plugin update`. The manifests are at [`.claude-plugin/`](.claude-plugin/); the skill lives at [`skills/skill-chain/`](skills/skill-chain/).

### Other AI agents

The skill is a plain `SKILL.md` with only `name` and `description` in the frontmatter, so it works with any agent that loads skills from a folder. Drop [`skills/skill-chain/`](skills/skill-chain/) wherever your agent looks for skills.

## Use it

Once installed, describe a workflow you repeat:

```
design a skill chain for how I handle a new inbound lead:
intake the lead, research the company, decide if they're a fit,
draft a tailored intro email, and queue them in my CRM
```

It will interview you, lay out the steps and handoffs, mark where you would review, and scaffold the files.

## License

MIT. Use it, fork it, ship it. See [LICENSE](LICENSE).
