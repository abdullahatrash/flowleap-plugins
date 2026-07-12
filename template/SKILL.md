---
# Copy this whole folder to start a new skill:
#   cp -r template plugins/<pack>/skills/<your-skill-name>
# Then rename the copied folder to your skill name and edit the two fields below.
#
# name: MUST be identical to the folder this file lives in. If the folder is
#       skills/patent-term-check/, this must read exactly `name: patent-term-check`.
name: your-skill-name
# description: one or two sentences. Say WHAT the skill does, then end with a
# "Trigger when…" clause naming the situations that should invoke it. This is the
# only text the agent reads to decide whether to use your skill, so make it
# concrete. Avoid version numbers, dates, or anything that will go out of date.
description: What this skill does in one sentence. Trigger when the user asks the agent to do X, review Y, or check Z.
---

# Your Skill Name

One or two sentences of context: who is using this and what they are trying to
achieve. Write the body as instructions addressed to the agent.

## How to do it

Break the task into clear steps. Every command must call the **FlowLeap CLI**
(`flowleap …`) — never the FlowLeap app's internal typed tool names like
`search_patents(...)`. Run `flowleap --help` to see the real commands, and only
put in commands you have actually run.

```bash
# Example — replace with the real commands your workflow needs:
flowleap patent build-query "wireless charging for electric vehicles"
flowleap patent search --query "ti=wireless AND ti=charging" --limit 20
flowleap ops claims EP3456789
```

Done when <a clear, checkable finish line — e.g. "every live hit has its claims
pulled and a legal-status verdict">. Write a finish line the agent can verify,
not a fuzzy one like "when you have enough results."

## Optional: pointing at other skills

If a deeper workflow lives in another skill, mention it as a suggestion that still
makes sense when that skill is NOT installed — for example: "If the full recipe
pack is installed, `recipe-prior-art-search` runs this end to end." Never write a
step that dead-ends when the other skill is missing.
