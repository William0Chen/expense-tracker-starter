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

`src/App.jsx` is the root component. It owns the canonical `transactions` array (the single source of truth) and the shared `categories` list, and passes them down to three child components in `src/components/`:

- **`Summary.jsx`** — receives `transactions` and computes `totalIncome`, `totalExpenses`, and `balance` internally for display.
- **`TransactionForm.jsx`** — owns its own add-form field state (`description`, `amount`, `type`, `category`) and reports a completed transaction up via the `onAddTransaction` callback. App appends it to `transactions`.
- **`TransactionList.jsx`** — owns its own filter state (`filterType`, `filterCategory`), renders the filtered table, and has a per-row delete button that calls the `onDeleteTransaction` callback with the row's `id`.

App holds two callbacks over the `transactions` array: `handleAddTransaction` (appends a new transaction) and `handleDeleteTransaction` (removes by `id` via `filter`). State lives as close to where it's used as possible: only `transactions` is lifted into `App`; form and filter state are local to their respective child components. There is no router, no backend, and no persistence — the transaction list is seeded from a hardcoded array and lives only in memory (a reload resets it). Transactions can be added and deleted, but there is no edit. Styling is plain CSS in `src/App.css` and `src/index.css`, referenced by className (the delete button uses `.delete-btn`).

## Context: this is a course starter

Per the README, this is the intentionally-flawed starting point for a Claude Code course — it "intentionally has a bug, poor UI, and messy code" that get fixed during the course. Treat existing rough edges as the subject of work, not as conventions to preserve.

The original string-vs-number `amount` bug has been fixed: `amount` is now stored as a **number** in the transaction objects (both the seed data and newly-added transactions, via `Number(amount)` in `TransactionForm`). Keep it numeric so `totalIncome`/`totalExpenses` add rather than concatenate.
