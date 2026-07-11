# Recipes

Recipe skill pack for FlowLeap Patent AI. Each skill is an end-to-end,
multi-step patent-research workflow that drives the [FlowLeap CLI](https://github.com/abdullahatrash/flowleap-cli)
(and, through it, the FlowLeap backend facade) to do real patent work.

| Skill | Workflow |
|-------|----------|
| `recipe-prior-art-search` | Comprehensive prior-art search — query generation, dual EPO/USPTO search, academic sweep, deep dives on the closest hits. |
| `recipe-freedom-to-operate` | FTO / clearance search — per-feature queries, dual-database blocking-patent search, legal-status, family, and claims checks. |
| `recipe-patent-landscape` | Landscape analysis — scoped searches, key-player identification, recent-activity checks, full-corpus filing analytics. |
| `recipe-claim-analysis` | Claim extraction and analysis with full context — claims, abstract, bibliography, description, family, element decomposition. |
| `recipe-patent-to-report` | Complete single-patent dossier — bibliography, claims, description, family, legal status, prosecution timeline, figures, related art. |
| `recipe-academic-literature-review` | State-of-the-art review combining scholarly search (Semantic Scholar, arXiv, OpenAlex) with a matching patent sweep. |

## How these skills reach data

These skills call the `flowleap` CLI (`flowleap patent search`, `flowleap ops
claims`, `flowleap academic search`, …). They reference **no in-app typed tool
names**, which is what makes them multi-harness: they work identically inside
the FlowLeap IDE's Claude sessions, in Claude Code, and in any agent that has
the FlowLeap CLI available.

### Requires the FlowLeap CLI (and the CLI tool skills)

This pack has a **soft dependency on the [FlowLeap CLI](https://github.com/abdullahatrash/flowleap-cli)** —
install it separately; it is a runtime dependency. Each recipe's frontmatter
declares `metadata.requires.skills` naming the CLI tool skills it builds on
(`flowleap-shared`, `flowleap-patent`, `flowleap-ops`, `flowleap-academic`).
Those skills ship in this marketplace's **[flowleap-tools](../flowleap-tools)**
pack — install that pack alongside these recipes so the referenced skills
resolve. The recipes orchestrate the individual tool skills into full workflows;
without the CLI (and its skills) installed, the recipes load but their commands
will not run.

## Format

This plugin uses the Claude plugin manifest format
(`.claude-plugin/plugin.json`), so it loads in both the FlowLeap app's plugin
marketplace and the Claude Code CLI. Skills live in `skills/<name>/SKILL.md`.
