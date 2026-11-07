---
name: friction-log
description: >
  File durable repo or tool papercuts as friction issues so the next person or
  agent can fix them. Use when a session hits recurring friction (a missing
  setup step, a lying type, a broken script or test harness, docs that disagree
  with the code, a brittle test), when wrapping up work that left friction
  behind, or for /friction-log.
---

# Friction log

Friction is durable, recurring pain with a clear owner and a reproducible gap:
a missing step, a lying type, a broken harness, docs that disagree with the
code, a brittle test. Log it so the next agent can act on it without your
session. One-off confusion and nits that only mattered in this session stay in
the session.

## 1. Fix first, then file the rest

Fix obvious, low-risk friction in the current change when it is in scope, and
mention the fix in your summary. File only what is left over.

## 2. Decide where it goes

Use the first that applies:

1. **The project's issue tracker.** For GitHub, use the `gh` CLI. For another
   tracker (GitLab, Linear, Jira, …), use its CLI or connected tool, following
   the same steps.
2. **A different owner.** If the friction lives in a dependency or tool the
   project does not own, file it on that project's tracker instead, and only if
   it clearly meets the bar.
3. **A local log.** If there is no tracker you can reach, append the entry to
   `FRICTION.md` in the repo root (create it if missing) and tell the user.

Never file to a public tracker without the user's go-ahead when the content
could reveal private code, internal names or customer data.

## 3. Search before filing

Look for an open friction issue covering the same gap. If one exists, comment
with your new evidence instead of opening a duplicate.

```bash
gh issue list --label friction --state open --search "<keywords>"
gh issue comment <number> --body "<new evidence>"
```

## 4. Write the issue

Give it a short, specific title naming the gap, not the symptom you felt. The
body has these sections:

- **What happened:** the observed behavior.
- **What you wanted:** the expected behavior or missing capability.
- **How to reproduce:** exact steps or commands.
- **Cost:** time lost, workarounds needed, how often it bites.
- **What you already looked at:** what you checked and ruled out.
- **Relevant files:** paths, line numbers, links.

If the project has a friction issue template, use it instead. Keep secrets,
tokens and private data out.

## 5. Label and prioritize

Add the `friction` label and one priority:

- `P0`: blocks work, no workaround.
- `P1`: costly workaround, hits often.
- `P2`: annoying, has a cheap workaround.
- `P3`: minor polish.

Add `needs-decision` when fixing it needs a human call (a design choice, an
ownership question). If the project already has its own priority or triage
labels, use those instead.

Create missing labels once, then file:

```bash
gh label create friction --color FBCA04 --description "Durable papercut worth fixing" 2>/dev/null
gh label create P2 --color C5DEF5 2>/dev/null
gh issue create --title "<title>" --label friction,P2 --body-file friction.md
```

`friction-skipped` marks an issue that friction sweeps should leave alone until
someone removes the label. Don't change it.

## 6. Report back

List what you fixed, what you filed (with links), what you commented on, and
what you judged below the bar and left unfiled.

## Local log format

When filing to `FRICTION.md`, add one entry per item, newest first:

```markdown
## <title> (P2, <YYYY-MM-DD>)

- What happened: …
- What you wanted: …
- How to reproduce: …
- Cost: …
- What you already looked at: …
- Relevant files: …
```
