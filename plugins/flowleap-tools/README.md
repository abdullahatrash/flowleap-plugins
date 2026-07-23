# FlowLeap Tools

The FlowLeap CLI tool pack. Each skill teaches an agent to drive one command
family of the [FlowLeap CLI](https://github.com/flowleap-ai/flowleap-cli)
and, through it, the FlowLeap backend facade — patent search, prosecution
metadata, literature, legal references, citations, and the unified tools facade.

| Skill | Command family |
|-------|----------------|
| `flowleap` | Umbrella / start-here map of every command family, routing to the specialist skills. |
| `flowleap-shared` | Authentication, credential storage, config precedence, global flags, output formats. |
| `flowleap-auth` | OAuth 2.0 device-flow login, `fl_pat_` personal API tokens, status, logout. |
| `flowleap-keys` | BYOK provider credentials — EPO OPS consumer key/secret, USPTO ODP API key. |
| `flowleap-patent` | EPO patent search with CQL and natural-language → CQL query building. |
| `flowleap-uspto` | USPTO Open Data Portal Lucene search, grants, applications, continuity chains. |
| `flowleap-ops` | Direct EPO Open Patent Services — bibliography, claims, description, family, legal status, abstract. |
| `flowleap-academic` | Academic literature (Semantic Scholar + arXiv) with source and year filters. |
| `flowleap-npl` | Non-patent literature (OpenAlex) with year, open-access, and publication-type filters. |
| `flowleap-legal` | Hybrid semantic/keyword search over patent-law reference documents (EPC, MPEP, EPO Guidelines, …). |
| `flowleap-citation` | USPTO enriched citation data — citations by application, forward citations, statistics, X-category references. |
| `flowleap-tools` | The agent-first `/v1/tools` facade — one stable contract (search_patents, get_claims, get_patent_summary, compare_patents, …). |

## How these skills reach data

Every skill calls the `flowleap` CLI (`flowleap patent search`, `flowleap ops
claims`, `flowleap tools run …`, …) or the backend `/v1/tools` facade. They
reference **no in-app typed tool names**, which is what makes them
multi-harness: they work identically inside the FlowLeap IDE's Claude sessions,
in Claude Code, and in any agent that has the FlowLeap CLI available.

### Requires the FlowLeap CLI

This pack has a **soft dependency on the [FlowLeap CLI](https://github.com/flowleap-ai/flowleap-cli)** —
install it separately; it is a runtime dependency. These skills document and
drive the CLI; without the `flowleap` binary on PATH (and valid credentials via
`flowleap-auth` / `flowleap-keys`) the skills load but their commands will not
run.

### Relationship to the other packs

This pack **ships** the `flowleap-*` skills that the
[recipes](../recipes) and [personas](../personas) packs declare as
`metadata.requires.skills`. Install this pack alongside those so their required
skills resolve to real content in the same marketplace.

## Format

This plugin uses the Claude plugin manifest format
(`.claude-plugin/plugin.json`), so it loads in both the FlowLeap app's plugin
marketplace and the Claude Code CLI. Skills live in `skills/<name>/SKILL.md`.
