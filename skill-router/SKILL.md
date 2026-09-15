---
name: skill-router
description: Routes a user's request to the best-fitting skill(s) already installed on this machine/project — nothing external. Use this whenever the user explicitly asks "which skill should I use," "what skill fits this," "find the best skill for this task," "route this task," or "/skill-router," and also proactively whenever a request is ambiguous enough that more than one installed skill could plausibly apply and picking wrong would waste a turn. This is strictly a router over what's ALREADY installed — never use it to search the web or suggest new skills to install (that's find-skills' job); if nothing installed fits, say so plainly instead of stretching a bad match.
metadata:
  version: 1.0.0
---

# Skill Router

You are a router, not a generalist: your job is to look at the user's actual request, compare it against the real list of skills installed right now, and name the ones that genuinely fit — not to guess from memory what skills probably exist.

## Why this exists

A machine can accumulate dozens or hundreds of installed skills across personal (`~/.claude/skills/`), project (`.claude/skills/`, `.agents/skills/`), and plugin sources. When a request is ambiguous, it's easy to either miss a skill that fits perfectly or invoke the wrong one out of habit. This skill forces an explicit comparison against the live, current skill list before answering, so the choice is grounded in what's real right now — not what was installed last month, or what a similar-sounding skill used to be called.

## Step 1 — Get the real, current list

Never answer from memory or from a table you wrote earlier — installed skills change over time (installs, uninstalls, updates). Get the live list:

- If the available-skills listing is already present in this conversation's context (it usually is, via a system-reminder), use that — it's already current.
- If it's missing, stale, or you suspect skills were added/removed since, call `ListSkills` to refresh it.

Read each candidate's name and description closely. The description is the only signal you have about what a skill actually does and when it's meant to trigger — don't infer behavior from the name alone (e.g. `finder` is a Spanish-language marketing orchestrator, not a file-finder).

## Step 2 — Match against the request

Identify what the user is actually trying to accomplish — the task, the domain, the expected output — and compare it against the descriptions from Step 1. A few things that trip this up:

- **Specificity wins.** A narrow skill whose description matches closely beats a broad one that could technically apply to almost anything.
- **Overlapping skills are common.** Several skills often cover adjacent ground (e.g. cold outreach vs. lifecycle email vs. page copy). Pick based on the precise task, and say why you didn't pick the neighbors if it's close.
- **Language and project scope matter.** Some installed skills are scoped to a specific project, brand, or language (Spanish-only orchestrators, project-specific design systems). Don't recommend one whose scope doesn't match the current project or request unless the user is clearly in that context.
- **A router skill is not itself an answer.** Don't recommend `skill-router` or `find-skills` as the fit for a task — route past them to the real skill underneath. `find-skills` is the exception only when the true answer is "nothing installed fits" and the user might want to search for something new.

## Step 3 — Report and offer

Output a short ranked list — 1 to 3 skills, best fit first. For each: name, and one line on why it fits *this specific request* (not a generic description restatement). Then offer to invoke the top pick.

If genuinely nothing installed fits, say that plainly — don't force a stretch match. In that case, mention that `find-skills` can search for something new to install, but don't invoke it yourself unless asked.

**Example output shape:**

```
Best fit: cold-email — this is outbound prospecting copy, not a lifecycle sequence, so cold-email's framework (subject line, one ask, follow-up cadence) is the closer match.
Also relevant: copywriting — if the ask expands to landing-page copy for the same offer.

Want me to invoke cold-email now?
```

## Notes

- Never name a skill that isn't in the actual current list. If you're unsure whether something exists, check — don't guess and don't hedge by inventing a plausible-sounding name.
- This is about routing, not doing the work yourself. Once the user confirms (or if the fit is unambiguous and the request is clearly asking you to just proceed), invoke the chosen skill with the Skill tool rather than answering from general knowledge.
- If the request is trivial enough that no skill would meaningfully help (a one-line factual question, a simple file edit), say so instead of forcing a recommendation.
