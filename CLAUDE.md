# CLAUDE.md

Small Express REST API (users + health check) used as the starter project for the Claude Code course.

## Commands

- `npm run dev` — start the API with auto-reload on http://localhost:3000
- `npm test` — run the tests (Node's built-in `node:test` runner + supertest)
- `npm run lint` — ESLint (`eslint:recommended`); CI runs lint and tests on every push

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules (`import` / `export`).
- Use Node's built-in `node:test` and `node:assert` for tests, not Jest or Mocha; put them in `tests/` as `<resource>.test.js` and call the app through supertest.
- Route handlers never touch the `users` array directly; all data access goes through functions exported by `db/store.js`.
- Errors are returned as JSON `{ "error": "<message>" }` with the matching status code (400 for bad input, 404 for not found).
- Do not change `server.js` so that it listens on import: it only calls `app.listen` when run directly, so tests can `require` the app.

## Architecture

- `server.js` — entry point: creates the Express app, mounts the routers, exports `app`.
- `routes/` — one router file per resource (`users.js` at `/users`, `health.js` at `/health`). A new resource gets its own file here and is mounted in `server.js`.
- `db/store.js` — in-memory data helper standing in for a database; data resets on every restart.
- Config comes from environment variables (see `.env.example`); real values live in `.env`, which is never committed.
