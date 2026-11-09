# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project context

Starter project for a Claude Code course: a small expense tracker built with React. Per the README, it intentionally has a bug, poor UI, and messy code, all meant to be fixed over the course. Don't treat the existing code as a pattern to preserve.

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

The whole app is one component, `App` in `src/App.jsx`, which holds the seed data, all `useState` state, the derived values (totals, balance, filtered list), the add-transaction form, and the table. Styles are plain CSS in `src/App.css` and `src/index.css`.

State is in memory only: there is no backend or persistence, so a page reload resets to the hardcoded seed transactions.

A transaction has the shape `{ id, description, amount, type, category, date }`, where `type` is `"income"` or `"expense"`. `amount` is currently a string, both in the seed data and from the form's input, so convert it to a number before doing arithmetic. The category list is defined inside `App` and shared by the form and the filter dropdown.
