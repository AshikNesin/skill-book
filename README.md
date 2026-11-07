# skill-book

A personal collection of agent skills gathered from different authors and kept in one place. Each skill sits in its own folder under `skills/` with its original license and a `SOURCE.md` saying where it came from.

## Skills

| Skill | What it does | Author |
| --- | --- | --- |
| [show-me](skills/show-me) | Explains the current topic visually: pseudocode, call trees, file trees, Mermaid diagrams, diffs or a focused HTML page. Run it as `/show-me`. | [HumanLayer](https://github.com/humanlayer/skills) |
| [visual-recap](skills/visual-recap) | Turns a PR, branch, commit or diff into an interactive visual recap for reviewers. Needs Builder.io's Plan connector, or its local-files mode. | [Builder.io](https://github.com/BuilderIO/skills) |
| [improve-codebase-architecture](skills/improve-codebase-architecture) | Scans a codebase for chances to turn shallow modules into deep ones, shows them as an HTML report, then works through the one you pick. Run it as `/improve-codebase-architecture`. | [Matt Pocock](https://github.com/mattpocock/skills) |
| [codebase-design](skills/codebase-design) | Shared architecture vocabulary and principles. Used by improve-codebase-architecture. | [Matt Pocock](https://github.com/mattpocock/skills) |
| [domain-modeling](skills/domain-modeling) | How to keep a project glossary and decision records (ADRs). Used by improve-codebase-architecture. | [Matt Pocock](https://github.com/mattpocock/skills) |
| [grilling](skills/grilling) | Questions you closely about a plan or design until it holds up. Used by improve-codebase-architecture. | [Matt Pocock](https://github.com/mattpocock/skills) |
| [create-verification-skill](skills/create-verification-skill) | Generates a project-local `verify-<app>` skill that launches your app, drives it the way a user would and captures evidence. Run it as `/create-verification-skill`. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| [maintain-verification-skill](skills/maintain-verification-skill) | Keeps a generated verification skill and its feature map up to date as the app changes. Run it as `/maintain-verification-skill`. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |

## Using a skill

Copy a skill's folder into your skills directory:

```sh
# Claude Code, available in every project
cp -R skills/show-me ~/.claude/skills/

# or for one project only
cp -R skills/show-me your-project/.claude/skills/
```

For improve-codebase-architecture, copy codebase-design, domain-modeling and grilling as well. create-verification-skill and maintain-verification-skill go together.

create-verification-skill was written for Cursor and puts the skill it generates in `.cursor/skills/verify-<app>/`. In Claude Code, move it to `.claude/skills/verify-<app>/` (or ask Claude to write it there) so it gets picked up.

## Adding a skill

1. Copy the skill's folder into `skills/<skill-name>/`.
2. Add the original `LICENSE` to that folder.
3. Add a `SOURCE.md` with the original link, author, license and the commit you copied from.
4. Bring along any other skills it calls by name.
5. Add a row to the table above.

## Licenses

Every skill keeps its original license, found in its own folder. All skills here currently use the MIT license.
