# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project context

Starter project for a Claude Code course: a small expense tracker built with React. Per the README, it started with an intentional bug, poor UI, and messy code, all meant to be fixed over the course. The incorrect totals have been fixed and the single large component has been split up, but the rest may still need work, so don't treat the existing code as a pattern to preserve.

## Commands

```bash
npm install        # install dependencies
npm run dev        # Vite dev server at http://localhost:5173
npm run build      # production build to dist/
npm run preview    # serve the production build
npm run lint       # ESLint across the repo
```

No test runner is configured, so there are no test commands. Verify changes by running the app.

## Architecture

JavaScript (JSX, not TypeScript) on React 19 and Vite 7, with no other runtime dependencies.

`src/App.jsx` is the parent. It owns the `transactions` array and the seed data, defines the `categories` list, and renders three child components that sit beside it in `src/`:

| Component | Props | Responsibility |
|---|---|---|
| `Summary` | `transactions` | Calculates and displays income, expenses, and balance |
| `TransactionForm` | `categories`, `onAdd` | Holds the input state, validates, and passes new entries up |
| `TransactionList` | `transactions`, `categories` | Holds the filter state and renders the filters and table |

Data flows one way. `App` is the only place that changes `transactions`: the form calls `onAdd` with `{ description, amount, type, category }`, and `App`'s `handleAdd` adds the `id` and `date`. State that only one component uses stays in that component.

Filtering happens inside `TransactionList`, and `Summary` receives the full list, so filters change the table but never the totals.

State is in memory only: there is no backend or persistence, so a page reload resets to the hardcoded seed transactions.

A transaction has the shape `{ id, description, amount, type, category, date }`, where `type` is `"income"` or `"expense"`. `amount` is currently a string, both in the seed data and from the form's input, so convert it to a number before doing arithmetic, as `Summary` does.

Styles are plain, global CSS. `src/App.css` is imported only by `App`, and its class names (such as `summary`, `add-transaction`, and `transactions`) apply to the child components too.
