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
| [friction-log](skills/friction-log) | Files recurring repo or tool papercuts as labeled, prioritized issues with the GitHub CLI (`gh`), or a local `FRICTION.md`. Run it as `/friction-log`. | Adapted from [Kent C. Dodds](https://github.com/kentcdodds/kcd-skills) |
| [correct](skills/correct) | Finds mistakes agents keep repeating in a repo and makes each one impossible, preferring architecture, then types, then lint, then tests, with docs last. Run it as `/correct`. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| [typescript-best-practices](skills/typescript-best-practices) | TypeScript rules for `.ts`/`.tsx` files: discriminated unions, branded types, no `as` casts, exhaustive switches, validating data at boundaries, with examples. Run it as `/typescript-best-practices`. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| [principle-type-system-discipline](skills/principle-type-system-discipline) | Make illegal states unrepresentable in any typed language. Used by typescript-best-practices. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| [principle-boundary-discipline](skills/principle-boundary-discipline) | Validate at system boundaries and trust types inside. Used by typescript-best-practices. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| [principle-encode-lessons-in-structure](skills/principle-encode-lessons-in-structure) | Turn repeated instructions into lints, checks or scripts. Used by principle-type-system-discipline. | [Lauren Tan / pstack](https://github.com/cursor/plugins/tree/main/pstack) |

## Use in your projects

Skills are installed and kept in sync with the open-source [`skills`](https://www.npmjs.com/package/skills) CLI. Nothing to install first; `npx` fetches it. Run these from the project's root.

```sh
# see what's available
npx skills add AshikNesin/skill-book --list

# install some skills
npx skills add AshikNesin/skill-book --skill show-me --skill friction-log -y

# or install all of them
npx skills add AshikNesin/skill-book --skill '*' -y

# pull the latest changes from skill-book
npx skills update -p -y

# list or remove installed skills
npx skills list
npx skills remove show-me
```

- Skills go in the project's `.agents/skills/` and are linked into each agent's own folder (for example `.claude/skills/`). The CLI asks which agents you use; name them with `--agent claude-code --agent codex` to skip the question.
- The CLI writes `skills-lock.json`. Commit it, and after a fresh clone run `npx skills experimental_install` to restore the same skills (this command is still marked experimental).
- Add `-g` to install for your user account instead, so the skills are available in every project.
- Some skills need others: install codebase-design, domain-modeling and grilling with improve-codebase-architecture, install create-verification-skill and maintain-verification-skill together, and install the three `principle-*` skills with typescript-best-practices.
- create-verification-skill puts the skill it generates in the project's `.agents/skills/verify-<app>/`.

To update a skill everywhere, change it here, push, then run `npx skills update -p -y` in each project.

## Changes from the originals

create-verification-skill and maintain-verification-skill were edited to use `.agents/skills/` instead of Cursor's `.cursor/skills/`. friction-log was rewritten to remove its dependency on Kody and on other kcd-skills, so it works with any agent and files issues through the GitHub CLI. Each skill's `SOURCE.md` lists any changes made to it.

## Adding a skill

1. Copy the skill's folder into `skills/<skill-name>/`.
2. Add the original `LICENSE` to that folder.
3. Add a `SOURCE.md` with the original link, author, license and the commit you copied from.
4. Bring along any other skills it calls by name.
5. Add a row to the table above.
6. Push, so `npx skills` can see it.

## Licenses

Every skill keeps its original license, found in its own folder. All skills here currently use the MIT license.
