# talk2me-ASD-STE100

Skill: every reply in ASD-STE100 Simplified Technical English. Result first, short sentences, one meaning per word, fixed layout.

The skill body is host-neutral. Only the install path and the always-on switch differ.

## Install

### Claude Code

```bash
mkdir -p ~/.claude/skills/talk2me-asd-ste100
cp SKILL.md ~/.claude/skills/talk2me-asd-ste100/
```

Always-on: copy `hooks/*.mjs` to `~/.claude/hooks/`, add it as a `SessionStart` hook, then `touch ~/.claude/.talk2me-asd-ste100-always`. The hook reads `SKILL.md` from `~/.claude/skills/talk2me-asd-ste100/`. Delete the flag file to turn always-on off.

In a session, invoke with `/talk2me-asd-ste100`. It stays on until `stop ste mode`.

### Hermes

```bash
mkdir -p ~/.hermes/skills/talk2me-asd-ste100
cp SKILL.md ~/.hermes/skills/talk2me-asd-ste100/
```

New sessions pick it up as `/talk2me-asd-ste100`. In an open session, `/reload-skills` refreshes the slash command.

Always-on (every new session, all surfaces):

```bash
hermes config set skills.auto_load '["talk2me-asd-ste100"]'
```

Takes effect in the next session. `hermes --ignore-rules` skips it. An explicit `hermes -s talk2me-asd-ste100` loads it for one launch without the config change.

### Codex

Codex reads skills from `~/.agents/skills/` (user) and `.agents/skills/` (repo). `~/.codex/skills` is the old path; Codex still finds it, the documented path is `~/.agents/skills`.

```bash
mkdir -p ~/.agents/skills/talk2me-asd-ste100
cp SKILL.md ~/.agents/skills/talk2me-asd-ste100/
```

Restart Codex. Invoke with `$talk2me-asd-ste100`, or run `/skills` and pick it. Codex can also load it when the request matches the skill description.

Always-on: Codex has no skill auto-load. Put the rules in `~/.codex/AGENTS.md` (all projects) or `AGENTS.md` in the repo (this project). Codex reads that file before every task. A skill stays on demand; `AGENTS.md` is the always-on switch.

## Before and after

Same question, plain console text. First block is a normal reply. Second block is the same reply under this skill.

```
$ how did the deploy go
```

Before:

```
I went ahead and took a look at the deploy, and it seems like it
basically worked, although there might have been a couple of issues
with some of the tests that we should probably look into at some point.
```

After (`/talk2me-asd-ste100`, or `$talk2me-asd-ste100` in Codex):

```
PARTIAL: The deploy completed. 2 of 48 tests failed.

| Test              | Cause                                      |
| auth.spec.ts:42   | Expected 200, got 401. No auth header.     |
| cache.spec.ts:17  | Timeout after 5 s.                         |

Next: Add the Authorization header in src/client.ts:88, then run npm test -- auth.
```

A second one, a status question:

```
$ is the scanner done
```

Before:

```
Yeah so I think it's pretty much finished? I kicked it off earlier and
it looks like it found a bunch of stuff, but I'm not 100% sure we
actually saved the report anywhere. Might be worth double-checking.
```

After:

```
NOT VERIFIED: The scan finished. This session did not confirm a saved report.

- Exit code: 0
- Findings: 14
- Expected path: /tmp/scan-report.json
- That path is not on disk.

Next: Run ls /tmp/scan-report.json. If it is missing, run the scan again with --out /tmp/scan-report.json.
```
