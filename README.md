# FlowLeap Plugin Marketplace

The curated, free-to-install plugin marketplace for **FlowLeap Patent AI**. This
public monorepo is the FlowLeap app's default, pre-trusted plugin marketplace:
installing anything published here skips the "plugins can run code" trust prompt,
because FlowLeap curates every entry through pull-request review.

Each plugin is a **Skill Pack** — a bundle of `SKILL.md` files that extend a
patent professional's agent with new workflows. The launch packs are:

- [**Personas**](plugins/personas) — patent attorney, IP analyst, researcher,
  and startup-founder skills that prime the agent for a role.
- [**Recipes**](plugins/recipes) — end-to-end workflows: prior-art search,
  freedom-to-operate, patent landscape, claim analysis, patent-to-report, and
  academic literature review.
- [**FlowLeap Tools**](plugins/flowleap-tools) — the `flowleap-*` skills that
  teach an agent to drive the FlowLeap CLI and backend facade (auth, provider
  keys, EPO/USPTO/OPS search, academic & non-patent literature, legal
  references, citations, and the unified `/v1/tools` facade).

Every pack drives the FlowLeap CLI / backend facade — no in-app typed tool
names — so the same skills work in the FlowLeap IDE, Claude Code, and any agent
with the CLI available.

## Free forever

Per [ADR 0006](https://github.com/abdullahatrash/flowleap-agent-v2/blob/main/docs/adr/0006-curated-free-marketplace-v1.md),
everything in this repository is **free and public, permanently**. Nothing here
can later become paid content. Premium/Pro packs, if they ever exist, must ship
over a separate authenticated channel — never by locking down anything in this
repo. Do not add content here that is intended to become paid.

## Repository layout

```
marketplace.json                     # marketplace index the FlowLeap app reads (root, checked first)
.claude-plugin/marketplace.json      # same index in Claude Code CLI format (kept in sync by CI)
plugins/
  personas/                          # one directory per plugin (the plugin root)
    .claude-plugin/plugin.json       #   Claude plugin manifest → loads in FlowLeap app + Claude Code
    skills/
      persona-patent-attorney/SKILL.md
      persona-ip-analyst/SKILL.md
      persona-researcher/SKILL.md
      persona-startup-founder/SKILL.md
  recipes/                           # end-to-end patent-research workflows
    .claude-plugin/plugin.json
    skills/
      recipe-prior-art-search/SKILL.md
      recipe-freedom-to-operate/SKILL.md
      recipe-patent-landscape/SKILL.md
      recipe-claim-analysis/SKILL.md
      recipe-patent-to-report/SKILL.md
      recipe-academic-literature-review/SKILL.md
  flowleap-tools/                    # the flowleap-* CLI / backend-facade tool skills
    .claude-plugin/plugin.json
    skills/
      flowleap/SKILL.md
      flowleap-shared/SKILL.md
      flowleap-auth/SKILL.md
      flowleap-keys/SKILL.md
      flowleap-patent/SKILL.md
      flowleap-uspto/SKILL.md
      flowleap-ops/SKILL.md
      flowleap-academic/SKILL.md
      flowleap-npl/SKILL.md
      flowleap-legal/SKILL.md
      flowleap-citation/SKILL.md
      flowleap-tools/SKILL.md
skills/                              # aggregation for the `npx skills` CLI (symlinks — see below)
scripts/validate.mjs                 # zero-dependency CI validator
test/fixtures/                       # deliberately broken marketplaces the validator must reject
```

## Three consumers, one source of truth

The skill content lives **once**, under `plugins/<plugin>/skills/<name>/SKILL.md`.
Three different tools consume it:

1. **FlowLeap app.** Reads the root `marketplace.json` (Open Plugin format),
   resolves each plugin's `source` directory, and loads its Claude-format
   manifest and skills. This repo is wired as the app's sole default
   marketplace, pre-seeded as trusted.
2. **Claude Code CLI.** `claude plugin marketplace add abdullahatrash/flowleap-plugins`
   reads `.claude-plugin/marketplace.json` and each plugin's
   `.claude-plugin/plugin.json`. Authoring plugins in the Claude manifest format
   is what makes the same packs usable outside the IDE.
3. **`npx skills` CLI** ([vercel-labs/skills](https://github.com/vercel-labs/skills)),
   which installs `SKILL.md` folders into 70+ agents (Claude Code, Cursor,
   Codex, …). It discovers skills in a repo's root `skills/` directory at shallow
   depth (`skills/<name>/SKILL.md`) — it does **not** descend into per-plugin
   `plugins/*/skills/` trees. So the root `skills/` directory here is an
   aggregation layer: each entry is a **relative symlink** to the canonical skill
   under `plugins/personas/skills/`. Symlinks (not copies) keep a single source
   of truth with no drift; git preserves them and the `npx skills` walker follows
   them.

   ```
   npx skills add abdullahatrash/flowleap-plugins --list   # lists the persona skills
   npx skills add abdullahatrash/flowleap-plugins          # installs them into your agent
   ```

> These persona skills call the **FlowLeap CLI** (`flowleap patent search`,
> `flowleap ops claims`, …) and reference no in-app typed tool names. That is
> what makes them multi-harness. Install the [FlowLeap CLI](https://github.com/abdullahatrash/flowleap-cli)
> to use them outside the FlowLeap app.

## Publishing = opening a pull request

There is no separate publish step. **Merging to `main` publishes.** To add or
change a pack:

1. Add or edit a plugin under `plugins/<plugin>/` (Claude manifest +
   `skills/<name>/SKILL.md`).
2. Register it in `marketplace.json` **and** `.claude-plugin/marketplace.json`
   (CI checks the two stay consistent).
3. If it ships new skills, add matching relative symlinks under `skills/`.
4. Open a PR. CI validates every manifest and every `SKILL.md`. Review is the
   curation gate; history and rollback are ordinary git operations.

Installed apps pick up merged changes on the marketplace's normal refresh
cadence (fetches cached ~8h; plugins auto-update ~24h).

## Validation

`scripts/validate.mjs` (no dependencies) checks:

- `marketplace.json`: every plugin has a non-empty `name`, `description`,
  `version`, and a `source` directory that exists and carries a plugin manifest.
- `.claude-plugin/marketplace.json`: present, well-formed, and lists exactly the
  same plugins as the root index.
- Each plugin manifest: `name` matches its folder; `description` present.
- Each `SKILL.md`: frontmatter `name` matches its folder and `description` is
  present.
- The root `skills/` aggregation: every entry resolves to a real `SKILL.md`.

```
npm run validate            # validate this repo (must pass)
npm run validate:fixtures   # every test/fixtures/invalid-* repo must FAIL
npm test                    # both of the above
```

CI runs both on every PR and push to `main`.
