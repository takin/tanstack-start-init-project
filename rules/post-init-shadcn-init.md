# post-init-shadcn-init: shadcn/ui init with TanStack Start preset

## Priority: HIGH

## Explanation

The TanStack CLI scaffold may include a baseline shadcn add-on. This step **re-runs** the official shadcn CLI with a **fixed registry preset** and the **Start** template so `components.json`, style tokens, and paths match the owner’s chosen preset (`b6Z8GIMhE`).

Run from the **project root** (inside `<project_name>` after `cd`), on the filesystem that already contains `package.json` from the scaffold.

## Ordering

Apply **before** [post-init-route-trim.md](post-init-route-trim.md). Any demo or extra route files touched or added by tooling should then be removed by the route-trim rule.

## Required command

```bash
bunx --bun shadcn@latest init --preset b6Z8GIMhE --template start
```

- **`--bun`** — run the shadcn CLI with the Bun runtime via `bunx`.
- **`--preset b6Z8GIMhE`** — owner registry preset (do not substitute unless the user asks).
- **`--template start`** — TanStack Start–aligned init.

## After the command

1. If the CLI reports errors, read the message; retry after fixing (e.g. missing peer deps). For flag drift, run `bunx --bun shadcn@latest init --help`.
2. Run the project’s **ESLint / lint** script if present (verify in `package.json`).
3. Continue with **route trim** per [post-init-route-trim.md](post-init-route-trim.md), then **shell cleanup** per [post-init-shell-cleanup.md](post-init-shell-cleanup.md).

## Bad Example

Skipping init and hand-editing `components.json` to “match” the preset — drifts from the registry and breaks `bunx shadcn add` expectations.

## Good Example

One non-interactive pass from repo root:

```bash
cd my-app
bunx --bun shadcn@latest init --preset b6Z8GIMhE --template start
```

## Context

- For **adding components** after init, use the **`shadcn`** skill in the workspace.
- If the user requests a **different preset code**, replace only `b6Z8GIMhE` in this command and document the change in the conversation.
