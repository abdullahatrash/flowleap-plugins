# Negative fixtures

Each `invalid-*` directory is a deliberately broken mini-marketplace that the
validator **must reject**. CI runs `node scripts/validate.mjs --test-fixtures`,
which validates every fixture and fails the build if any fixture unexpectedly
passes. This guards the validator itself against silently going lax.

| Fixture | Broken thing |
|---------|--------------|
| `invalid-marketplace-missing-name` | A `marketplace.json` plugin entry missing the required `name`. |
| `invalid-skill-missing-description` | A `SKILL.md` whose frontmatter omits `description`. |
| `invalid-skill-name-mismatch` | A `SKILL.md` whose frontmatter `name` does not match its folder. |

When you add a new validation rule, add a fixture that trips it.
