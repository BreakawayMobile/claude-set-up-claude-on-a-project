# CLAUDE.md

Starter Express REST API with users and health endpoints, used as the working repo for the Claude Code course projects.

## Commands

```bash
npm run dev    # Start on http://localhost:3000 and restart on file changes
npm test       # Run all tests
npm run lint   # Run ESLint

# Run a single test by name
node --test --test-name-pattern="GET /health" tests/users.test.js
```

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules. ESLint parses files as scripts, so `import` fails lint.
- Read and write data through functions exported by `db/store.js`, not with data kept in route files.
- Return errors as `{ error: "message" }` with the matching status code, not as plain text.
- Convert `req.params.id` with `Number()` before lookups, because store IDs are numbers.
- Write tests with `node:test`, `node:assert`, and `supertest`, passing the `app` exported by `server.js`.
- Assert on specific users in tests, not on an exact user count, because tests share store state.
- Stage files by name, not with `git add .`, and never commit `.env` or `.claude/settings.local.json`. Neither is git-ignored.
- For the current course project, change only `CLAUDE.md`, `.claude/settings.json`, and `NOTES.md`, not application code.

## Architecture

- `server.js` is the entry point. It builds the Express app, mounts each router, and exports `app`. It only calls `listen` when run directly, so tests can import it without opening a port.
- `routes/` holds one router file per resource. Mount each new router in `server.js`.
- `db/store.js` is the only data layer. It keeps users in memory, so data resets on restart and is shared by every test in a process.
