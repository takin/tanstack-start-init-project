# tanstack-start-project-init

**English:** An [Agent Skill](https://cursor.com/docs/context/rules#agent-skills) for scaffolding **new** [TanStack Start](https://tanstack.com/start) apps with **Bun**, **ESLint**, **Nitro** deployment, and CLI add-ons (nitro, table, store, shadcn, form, compiler, drizzle). It encodes a repeatable workflow: `bunx @tanstack/cli create`, then ordered post-init steps (shadcn preset, route trim, shell cleanup).

**Bahasa Indonesia:** Skill agen untuk membuat proyek **baru** TanStack Start dengan Bun, ESLint, deploy Nitro, dan add-on CLI di atas. Alurnya: perintah `create` resmi, lalu langkah pasca-init berurutan (preset shadcn, rapikan route, bersihkan template).

## Repository layout

| Path | Purpose |
|------|---------|
| [`SKILL.md`](SKILL.md) | Skill definition: frontmatter, default `create` command, agent checklist, related skills, troubleshooting. |
| [`rules/post-init-shadcn-init.md`](rules/post-init-shadcn-init.md) | Re-run `shadcn` init with preset `b6Z8GIMhE` and template `start`. |
| [`rules/post-init-route-trim.md`](rules/post-init-route-trim.md) | Keep only `src/routes/__root.tsx` and `src/routes/index.tsx`; regenerate route tree. |
| [`rules/post-init-shell-cleanup.md`](rules/post-init-shell-cleanup.md) | Empty `<main>` on the index route; remove Header/Footer/demo form chrome from `__root` and components. |

## Who should use this

- **AI agents / Cursor:** Load this skill when the user asks to bootstrap a TanStack Start app with this stack (Bun + ESLint + Nitro + Drizzle + shadcn, minimal routes after init).
- **Humans:** Use [`SKILL.md`](SKILL.md) as the canonical reference; the `rules/` files are step-by-step playbooks you can run manually.

## Prerequisites

- [Bun](https://bun.sh) installed.
- Network access for `bunx` to fetch `@tanstack/cli` (and later `shadcn`).

## Default scaffold command

Run from the **parent** directory of the new project folder. Replace `<project_name>` with the target directory name.

```bash
bunx @tanstack/cli@latest create --no-examples \
  --add-ons=nitro,table,store,shadcn,form,compiler,drizzle \
  --package-manager=bun \
  --deployment=nitro \
  --toolchain=eslint \
  <project_name>
```

Then `cd <project_name>` and apply the post-init rules **in order**: shadcn init → route trim → shell cleanup. Exact commands and edge cases are in each rule file and in [`SKILL.md`](SKILL.md).

## Related skills

- **`tanstack-start-best-practices`** — Patterns after the repo exists (server functions, auth, SSR, deployment).
- **`shadcn`** — Adding and styling components once `components.json` exists.

## CLI drift

TanStack CLI flags and add-on names can change between releases. If `create` fails, run `bunx @tanstack/cli create --help` and align flags while preserving intent (Bun + ESLint + Nitro + the same add-ons).

## Scope

This skill is for **new project scaffolding** (or explicit greenfield recreation), not for refactors or feature work inside an existing app.
