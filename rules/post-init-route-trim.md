# post-init-route-trim: Trim route tree after scaffold

## Priority: HIGH

## Explanation

After `bunx @tanstack/cli create`, templates may ship extra file-based routes (`about.tsx`, demos, nested segments). This project’s convention is a **minimal route tree**: only the root layout and home route remain. That avoids dead links, unused demo pages, and conflicting route modules before feature work.

Apply this rule **once**, immediately after scaffold succeeds. When [post-init-shadcn-init.md](post-init-shadcn-init.md) is part of the workflow, run **route trim after** that shadcn init step. Next in the full workflow: [post-init-shell-cleanup.md](post-init-shell-cleanup.md), then dev verification.

## Required steps

1. Under **`src/routes/`**, **delete every `*.tsx` file** that is **not** one of:
   - **`__root.tsx`** — keep (TanStack Router root layout; required).
   - **`index.tsx`** at `src/routes/index.tsx` — keep (home route `/`).
2. Apply **recursively** for any `*.tsx` under subfolders of `src/routes/` (e.g. `about.tsx`, nested route files).
3. Remove **empty directories** left under `src/routes/` after deletions.
4. **Do not** delete generated artifacts such as **`routeTree.gen.ts`**. Run **`bun run dev`** (or the repo’s route/codegen script) so the route tree regenerates after file changes.
5. Run the project’s **ESLint / lint** script if present (e.g. `bun run lint` — verify the script name in `package.json`).

## Bad Example

Leaving template routes in place:

```
src/routes/
├── __root.tsx
├── index.tsx
├── about.tsx          ← should be removed
└── demo/
    └── index.tsx      ← should be removed
```

## Good Example

Minimal tree after post-init:

```
src/routes/
├── __root.tsx
├── index.tsx
└── routeTree.gen.ts   ← generated; do not delete manually
```

## Context

- If the template places **`index.tsx` only in a nested folder** and not at `src/routes/index.tsx`, adjust only if the user’s intent is still “single entry route”—default assumption here is **keep `src/routes/index.tsx`** as documented.
- If **`__root.tsx` is missing** (unusual), do not delete it—report; the scaffold may have changed layout.
