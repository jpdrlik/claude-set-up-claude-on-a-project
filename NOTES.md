# NOTES

## What went into CLAUDE.md, and what was left out

`CLAUDE.md` keeps only what Claude can't cheaply work out by itself or would likely get wrong: the three commands (including that tests use `node:test`, not Jest), the conventions that are easy to break without noticing (CommonJS, data access only through `db/store.js`, the JSON error shape, keeping `app.listen` out of the import path so tests work), and a short map of where things live and where a new resource goes.

I deliberately left out a file-by-file description of the code, the course instructions, setup steps for installing Node, and anything from `.env`. Claude can read the code when it needs details, the course text is a one-off task rather than a lasting rule, and secrets don't belong in a file that is committed and loaded into every session. Keeping the file short means every line gets followed.

## Permission rules

- **allow:** `npm test`, `npm run lint`, `git status` and `git diff`. They are safe, read-only or local, and I run them constantly, so approving them every time is just friction.
- **ask:** `git push` and `npm install`. Both reach outside my machine (the remote repo, the npm registry), so I want to see them before they happen.
- **deny:** reading `.env`, `git push --force` / `-f`, and `rm -rf`.

Without the deny rules, Claude could read `.env` while "looking around" the project and copy a real secret into its context, a log or a commit; a force-push could overwrite history on the shared branch and lose other people's work; and `rm -rf` could delete files that are not in git. Deny rules win over allow rules, so these stay blocked even if I approve something broader later.

## Fix found along the way

The lines in `.gitignore` had leading spaces, and Git treats leading spaces as part of the pattern, so `.env` and `.claude/settings.local.json` were **not** actually being ignored (`git check-ignore .env` returned nothing). I removed the indentation; now both files are ignored as the README intends.

## Verification

- `claude --version` runs, and I'm signed in.
- In a fresh session, `/memory` lists the project `CLAUDE.md` as loaded.
- `/permissions` shows the allow, ask and deny rules above.
- Asking "How do I run the tests here?" gets `npm test` with the `node:test` runner, answered from `CLAUDE.md`.
