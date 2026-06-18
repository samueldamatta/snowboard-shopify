# SPEC — Task 5 (Optional): GitHub + Shopify CLI

## Goal
Optional assessment points: **connect GitHub** and **use the Shopify CLI** for the theme workflow.

## Current state
- `samu-test` is a theme folder but **not a git repo** and has no GitHub remote.
- A `.shopify/` directory exists (CLI has been used locally); no `shopify.theme.toml` committed.

## Steps
1. **Git init**
   - `git init` in `/Users/samueldamatta/ProjetosWeb/samu-test`.
   - Add `.gitignore`: ignore `.shopify/`, `node_modules/`, `.DS_Store`, editor files.
   - Initial commit of the theme.
2. **GitHub**
   - `gh repo create <name> --private --source . --push` (or create + `git remote add` + push).
3. **Shopify CLI workflow** (document in README):
   - `shopify theme dev --store <store-handle>` → local preview at `http://127.0.0.1:9292`.
   - `shopify theme push` / `shopify theme pull` for deploys/sync.
   - Optionally connect the repo in Admin → Online Store → Themes → **Add from GitHub** for
     branch-based deploys.

## Deliverables
- `.gitignore` (new)
- `README.md` (new) — setup + CLI commands + how to run the dev server.
- Git history pushed to GitHub.

## Open items
- Confirm the **store handle** for `--store` and the desired **repo name / visibility**.

## Acceptance criteria
- `git status` clean after commit; repo visible on GitHub.
- `shopify theme dev` serves the theme locally.
- README documents the dev/push/pull workflow.
