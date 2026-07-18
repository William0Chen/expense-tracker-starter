# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install      # install dependencies
npm run dev      # start Vite dev server at http://localhost:5173
npm run build    # production build to dist/
npm run preview  # serve the production build locally
npm run lint     # run ESLint over all .js/.jsx files
```

There is no test runner configured in this project.

## Architecture

Single-page React 19 app scaffolded with Vite. Entry is `src/main.jsx`, which mounts `src/App.jsx` into `#root` (see `index.html`).

The **entire application is `src/App.jsx`** — one `App` component holding all state via `useState`. There is no router, no backend, and no persistence: the transaction list is seeded from a hardcoded array and lives only in memory (a reload resets it). Adding a transaction appends to that in-memory array; there is no delete or edit.

State is grouped by purpose within the component: the `transactions` array, the add-form fields (`description`, `amount`, `type`, `category`), and the two list filters (`filterType`, `filterCategory`). Derived values (`totalIncome`, `totalExpenses`, `balance`, `filteredTransactions`) are recomputed on each render from that state. Styling is plain CSS in `src/App.css` and `src/index.css`, referenced by className.

## Context: this is a course starter

Per the README, this is the intentionally-flawed starting point for a Claude Code course — it "intentionally has a bug, poor UI, and messy code" that get fixed during the course. Treat existing rough edges as the subject of work, not as conventions to preserve.

One concrete known issue: `amount` is stored as a **string** in the transaction objects, so `totalIncome`/`totalExpenses` compute `sum + t.amount` as string concatenation rather than numeric addition. Convert to `Number` when doing arithmetic.
