# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

Single-package Next.js 16 community platform (not a monorepo). All npm scripts are in the root `package.json`. See `docs/DEVELOPMENT.md` for the full development guide.

### Running the app (demo mode)

The app works out of the box in demo mode with no external services:

```bash
cp .env.local.demo .env.local   # only needed once
npm run dev                      # http://localhost:3000
```

Firebase-dependent features (auth, Firestore, storage) are gracefully disabled in demo mode. All pages, layouts, navigation, and static content render correctly.

### Key commands

| Task | Command |
|------|---------|
| Dev server | `npm run dev` (binds to `localhost:3000`) |
| Lint | `npm run lint` |
| Type check | `npm run type-check` |
| Unit tests | `npm test` |
| Build | `npm run build` (runs `validate-env` first via `prebuild`) |

### Gotchas

- The dev script uses `--webpack` flag and `-H localhost`: `NODE_OPTIONS='--no-deprecation' next dev --webpack -H localhost`.
- `npm run lint` produces 1 expected warning in `postcss.config.mjs` (`import/no-anonymous-default-export`); this is not a blocker.
- `npm run build` runs `validate-env` as a prebuild step and will reject placeholder env values. The `.env.local.demo` values pass validation.
- Firestore rules tests (`npm run test:rules`) require the Firebase emulator and are not expected to work without it.
- Husky pre-commit hooks run `gitleaks` (skips if not installed), GPL header insertion, and `lint-staged` (tsc + eslint). The commit-msg hook enforces Conventional Commits via `commitlint`.
- All commits require `-s` (sign-off) for DCO compliance and Conventional Commit format: `type(scope): description`.
