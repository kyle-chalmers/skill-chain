---
name: skill-chain
description: "Design and scaffold a skill chain: a set of small skills that run in order and hand their work to each other, so a repeatable multi-step workflow becomes a pipeline instead of one giant prompt. Use when you keep doing the same multi-step process by hand and want to wire it together. Triggers: 'design a skill chain for', 'turn this workflow into a pipeline', 'break this big process into skills', 'chain my skills together', 'scaffold a skill pipeline for X'."
---

# Skill Chain

A skill is a small instruction file your AI agent loads on demand. In practice it is a folder with a `SKILL.md` inside: a `name`, a `description` full of the phrases you would actually say to trigger it, and a workflow the agent follows. You install one by dropping the folder where your agent looks for skills (for Claude Code that is `~/.claude/skills/`).

A skill chain is several of those small skills, run in order, where each step's output feeds the next. The chain is the unit of automation. The skills are the steps. One giant prompt that tries to do everything is hard to repeat, hard to fix, and gives you nowhere to step in. A chain lets you run a step on its own, swap one out, review at the points that matter, and restart from the middle.

This skill walks you through the design decisions for a new chain, then scaffolds the `SKILL.md` stubs and a short pipeline guide so a later session can pick it up and fill in the real work.

## When this fits

This is a recognition test, not a gate. A workflow is a good candidate when most of these are true:

1. **You repeat it.** It is part of how the work actually runs, not a one-off.
2. **It has steps in an order, and each feeds the next.** One step produces something the next step uses.
3. **It varies each time.** This is the important one. If the process is identical every single time, a plain script already does it. Chains earn their keep when each step needs a little judgment.
4. **You would want to review along the way.** There are points where you would check the work before moving on.

If it passes, it is worth chaining. A few honest notes so you do not over-build:

- **Short chains count.** Even two solid steps can be a chain if each is a meaty chunk of work. You do not need many stages.
- **A shared label helps, but is not required.** Giving one run of the chain a single identifier (a number, a client name, a slug) keeps files from getting mismatched across steps. Use it when it helps. It is a habit, not a rule.
- **Handoffs are usually files, sometimes just the key facts.** The point is that a step passes something concrete to the next, not that it must always be a saved document.
- **This is an art, not a precise science.** These are guidelines. The shape that helps you is the right one.

## Example chains

Three shapes most people recognize. Use them to sanity-check your own workflow.

| Chain | Steps | What passes between steps |
|---|---|---|
| Content production | idea -> research -> draft -> edit -> publish -> promote | research notes feed the draft; the published link feeds the promo posts |
| Client onboarding | intake -> proposal -> contract -> kickoff -> invoice | intake answers populate the proposal; the signed contract triggers the invoice |
| Hiring | job spec -> sourcing -> screening -> interview -> offer | the job spec scopes sourcing; screening notes feed the interview panel |

The thing to notice: each arrow carries something real. The draft step reads the research the earlier step produced. That handoff is what makes it a chain instead of a pile of related tasks.

Chains come in two valid styles. **Numbered and strictly sequenced** (content production: one canonical order) suits a workflow with a single path. **Loosely ordered** (client onboarding: steps fire on their own triggers but accumulate against the same client) suits a workflow where stages start on their own schedule. Pick the one that matches how the work actually arrives.

---

## Workflow

Run these in order. Each phase's output feeds the next.

### 1. Capture intent

Get these in plain prose before touching structure:

1. **What does the chain produce end to end?** One sentence. ("A published article." "A signed and invoiced client." "A hired candidate.")
2. **What starts it?** The trigger. ("Someone approves a topic." "A lead signs." "A role opens.")
3. **Roughly how many steps?** A guess is fine. It gets refined in step 2.
4. **Where should it live?** Which project or skills folder, so related chains sit together.

If any of these is vague, ask before continuing. Vague intent produces a vague chain.

### 2. Decompose steps

For each step, capture five fields. Use a table.

| # | Name | What it produces | What it consumes | Review here? |
|---|---|---|---|---|

Rules for splitting:

- **A step is a meaty chunk.** If it splits into two things that each produce something useful on their own, split it. If two steps are tiny, combine them. Even a two-step chain is fine.
- **A step passes something concrete to the next.** A file, a document, a row, or just the key facts. If it only changes something in memory and hands nothing on, fold it into a neighbor.
- **Mark where you would review.** For each step, note whether you want to check the work before it moves on. Automate the safe ones for speed, stay in the loop where quality matters.
- **Do not number steps until the order is locked.** Re-numbering later is annoying.

### 3. Give the run a label (optional but handy)

If it helps, pick one identifier that travels with a single run of the chain through filenames and folders, so nothing gets mismatched. A number, a slug, or a composite like `{client}/{project}`. Decide who assigns it and whether it stays stable. Skip this for a short chain where mismatching is not a risk.

### 4. Define the handoffs

For each pass between steps, specify:

- **The upstream step produces:** what, and where it lands (filename, location, format), or the key facts it hands on.
- **The downstream step reads:** the same thing, and what it uses from it.
- **What if it is missing or stale?** Hard error, graceful fallback, or re-run the upstream step.

The strongest pattern is one clear artifact at a known place, produced once and read by several later steps. Aim for that when it fits.

### 5. Choose folder layout

If the chain writes files, the filesystem can encode the pipeline. Two common styles, pick one:

- **Per-step folders, run-named files:** `2 Drafts/{slug}.docx`, `3 Published/{slug}.json`. Good for many steps and one shared archive.
- **Per-run folders, step-named files:** `clients/{client}/proposal.docx`, `clients/{client}/invoice.pdf`. Good when each run is long-lived and collects artifacts over time.

Do not mix styles within one chain.

### 6. Identify cross-cutting skills

Some skills run at more than one step, taking a step argument. A review skill that can check the proposal, the contract, or the kickoff plan is one skill invoked at different points, not three skills. Call these out separately in the guide, not in the step list.

### 7. Write the pipeline guide

Write a short `{chain-name}-pipeline-guide.md` in the project. Required sections:

- **What the chain produces and when to run it** (from step 1)
- **Step table** (from step 2): name, trigger phrase, what it produces, what it consumes, where you review
- **The run label**, if you use one (from step 3)
- **Folder layout**, if it writes files (from step 5)
- **Cross-cutting skills** (from step 6)
- **Key handoffs:** the non-obvious passes, for example "step 2 to step 5: the research file is the source of truth for every cited claim"

This is the one place the chain's shape is written down. Everything downstream trusts it.

### 8. Scaffold the SKILL.md stubs

Create one folder per step, named for the step. **Before creating each folder, check whether it already exists** in the target skills directory. If it does, ask the owner whether to overwrite, rename the chain prefix, or stop, so you never clobber an existing skill. Then in each new folder write a `SKILL.md` with:

- **Frontmatter `name`** matching the folder name.
- **Frontmatter `description`** covering what the step produces, its position ("Step N of {chain-name}, after X, before Y"), and the trigger phrases someone would actually say.
- **A stub `## Workflow` section** with placeholder steps. Do not write the real implementation here; the point is correct metadata the agent can find.
- **A `## Handoff contracts` section** listing what it reads upstream and writes downstream, with exact paths.

Keep frontmatter to just `name` and `description` so it stays valid across different agents.

### 9. Wire the chain into the agent's context file

Most agents load a project context file at startup (for Claude Code it is `CLAUDE.md`; other agents use their own). Propose the line that points that file at the new pipeline guide, so future sessions know the chain exists. Do not add the line yourself. Show the exact line and where it goes; the owner decides where it belongs.

### 10. Output summary

Print: files created with paths, the stubs to fill in next in chain order, the context-file line to add, and any open design questions you could not resolve.

---

## Operational notes

- **Do not write implementation for the scaffolded step skills.** The output of this skill is structure and metadata. Filling in each step is a separate task that deserves full attention.
- **The pipeline guide is the contract.** If a later session changes a step's output format, the guide changes first. Treat it as the source of truth, not documentation written after the fact.
- **Chains are iterative.** The one you sketch first is rarely the one you end up with. The more you run a chain, the more steps reveal themselves. Expect to revise it.

## Anti-patterns to avoid

- **Do not chain things that pass nothing between them.** Steps that share a topic but hand nothing to each other are a tag, not a chain.
- **Do not over-decompose.** Eight tiny steps is usually four real ones. Combine.
- **Do not skip the guide.** "I will remember the order" is how a workflow ends up with three competing versions of itself.
