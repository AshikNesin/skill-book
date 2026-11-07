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

Do every tracker step with the tracker's own CLI, never by hand in a browser
and never through raw API calls. The commands below were checked against each
CLI's `--help`; if a flag is rejected, run `<cli> <command> --help` and adapt.

## 1. Fix first, then file the rest

Fix obvious, low-risk friction in the current change when it is in scope, and
mention the fix in your summary. File only what is left over.

## 2. Pick the tracker and its CLI

Use the tracker the project already uses. Work it out from the git remote,
the README or CONTRIBUTING file, or ask the user once. Then confirm its CLI is
installed and logged in:

| Tracker | CLI | Check |
| --- | --- | --- |
| GitHub Issues | `gh` | `gh auth status` |
| GitLab Issues | `glab` | `glab auth status` |
| Linear | `linear` ([schpet/linear-cli](https://github.com/schpet/linear-cli)) | `linear auth whoami` |
| Jira | `acli` (Atlassian CLI) | `acli jira auth status` |

If the CLI is missing or not logged in, tell the user the one command to fix
it (for example `gh auth login`) and stop; don't work around it. If the
project has no tracker at all, use the local log (section 7).

If the friction lives in a dependency or tool the project does not own, file
it on that project's tracker instead, and only if it clearly meets the bar.
Never file to a public tracker without the user's go-ahead when the content
could reveal private code, internal names or customer data.

## 3. Write the issue body to a file

Write the body to a temporary Markdown file (for example `/tmp/friction.md`)
and pass the file to the CLI, so long Markdown survives shell quoting. Give the
issue a short, specific title naming the gap, not the symptom you felt. The
body has these sections:

- **What happened:** the observed behavior.
- **What you wanted:** the expected behavior or missing capability.
- **How to reproduce:** exact steps or commands.
- **Cost:** time lost, workarounds needed, how often it bites.
- **What you already looked at:** what you checked and ruled out.
- **Relevant files:** paths, line numbers, links.

If the project has a friction issue template, follow it instead. Keep
secrets, tokens and private data out.

## 4. Pick a priority

- `P0`: blocks work, no workaround.
- `P1`: costly workaround, hits often.
- `P2`: annoying, has a cheap workaround.
- `P3`: minor polish.

Add `needs-decision` when fixing it needs a human call (a design choice, an
ownership question). If the project already has its own priority or triage
labels, use those instead. `friction-skipped` marks an issue that friction
sweeps should leave alone until someone removes the label; don't change it.

## 5. Search, then comment or file

Search open friction issues first. If one covers the same gap, comment with
your new evidence instead of opening a duplicate.

### GitHub (`gh`)

```bash
# search
gh issue list --label friction --state open --search "<keywords>"
# comment on a match
gh issue comment <number> --body-file /tmp/friction.md
# create missing labels once (errors if they exist; that's fine)
gh label create friction --color FBCA04 --description "Durable papercut worth fixing" 2>/dev/null || true
gh label create P2 --color C5DEF5 2>/dev/null || true
# file
gh issue create --title "<title>" --label friction --label P2 --body-file /tmp/friction.md
```

Add `--repo owner/repo` to any command to target another repository. To use a
repo issue template, add `--template "<template name>"`.

### GitLab (`glab`)

```bash
# search
glab issue list --label friction --search "<keywords>"
# comment on a match (GitLab calls comments "notes")
glab issue note <number> --message "$(cat /tmp/friction.md)"
# create missing labels once
glab label create --name friction --color "#FBCA04" --description "Durable papercut worth fixing" 2>/dev/null || true
glab label create --name P2 --color "#C5DEF5" 2>/dev/null || true
# file
glab issue create --title "<title>" --label friction,P2 --description-file /tmp/friction.md --yes
```

Add `--repo group/project` to target another project. To use a template from
`.gitlab/issue_templates/`, add `--template <name>`.

### Linear (`linear`)

Linear has a built-in priority, so map P0–P3 to it instead of labels:
`P0 → urgent`, `P1 → high`, `P2 → medium`, `P3 → low`.

```bash
# search: list open friction issues from everyone, then match titles
linear issue list --label friction --all-assignees \
  --state triage --state backlog --state unstarted --state started --json --no-pager
# comment on a match
linear issue comment add ENG-123 --body-file /tmp/friction.md --yes
# create the label once
linear label create --name friction --color "#FBCA04" --description "Durable papercut worth fixing" --yes
# file
linear issue create --title "<title>" --team <TEAM> --label friction \
  --priority medium --description-file /tmp/friction.md --no-input
```

Leave out `--team` when the project has a default team configured.

### Jira (`acli`)

Jira labels need no setup; they are created on first use.

```bash
# search
acli jira workitem search \
  --jql 'project = <KEY> AND labels = friction AND statusCategory != Done AND text ~ "<keywords>"' \
  --fields "key,summary,status" --json
# comment on a match
acli jira workitem comment create --key <KEY-123> --body-file /tmp/friction.md
# file
acli jira workitem create --project <KEY> --type Task --summary "<title>" \
  --label "friction,P2" --description-file /tmp/friction.md --json
```

## 6. Report back

List what you fixed, what you filed (with links or keys from the CLI output),
what you commented on, and what you judged below the bar and left unfiled.

## 7. Local log (no tracker)

When the project has no tracker, append one entry per item to `FRICTION.md`
in the repo root (create it if missing), newest first, and tell the user:

```markdown
## <title> (P2, <YYYY-MM-DD>)

- What happened: …
- What you wanted: …
- How to reproduce: …
- Cost: …
- What you already looked at: …
- Relevant files: …
```
