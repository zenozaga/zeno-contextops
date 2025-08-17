# Rules

This file defines **coding conventions** and **error handling** the AI must always respect.

- Language/TS: Enable TypeScript `strict`. Avoid `any` except at boundaries.
- Errors: Extend `CustomError` with fields `code` and `additionalInfo`.
- Style: Keep functions small, single responsibility, documented when non-trivial.
- Tests: Provide unit tests for new logic; E2E when adding adapters.
- Commits: Use Conventional Commits (feat, fix, chore, docs, test, refactor).
