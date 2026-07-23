# Personas

Persona skill pack for FlowLeap Patent AI. Each skill primes the agent to work
as a particular kind of IP professional, driving the [FlowLeap CLI](https://github.com/flowleap-ai/flowleap-cli)
(and, through it, the FlowLeap backend facade) to do real patent work.

| Skill | Persona |
|-------|---------|
| `persona-patent-attorney` | Prior-art search, claim reading, legal-status checks, FTO assessment. |
| `persona-ip-analyst` | Landscape mapping, portfolio assessment, filing-trend analytics. |
| `persona-researcher` | Parallel literature + patent exploration for R&D. |
| `persona-startup-founder` | Novelty checks, FTO scans, competitor monitoring, patent strategy. |

## How these skills reach data

These skills call the `flowleap` CLI (`flowleap patent search`, `flowleap ops
claims`, `flowleap academic search`, …). They reference **no in-app typed tool
names**, which is what makes them multi-harness: they work identically inside
the FlowLeap IDE's Claude sessions, in Claude Code, and in any agent that has
the FlowLeap CLI available.

### Requires the FlowLeap CLI

This pack has a **soft dependency on the [FlowLeap CLI](https://github.com/flowleap-ai/flowleap-cli)** —
install it separately; it is a runtime dependency. Each persona's frontmatter
declares `metadata.requires.skills` naming the CLI's own skills
(`flowleap-shared`, `flowleap-patent`, `flowleap-ops`, `flowleap-academic`).
Those skills live in the flowleap-cli repository, **not** in this pack, and they
are what supply the `flowleap` commands the personas invoke. Without the CLI (and
its skills) installed, the personas load but their commands will not run.

## Format

This plugin uses the Claude plugin manifest format
(`.claude-plugin/plugin.json`), so it loads in both the FlowLeap app's plugin
marketplace and the Claude Code CLI. Skills live in `skills/<name>/SKILL.md`.
