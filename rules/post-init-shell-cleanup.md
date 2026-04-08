# post-init-shell-cleanup: Empty home main and remove template chrome

## Priority: HIGH

## Explanation

After scaffold (and after shadcn init + route trim), strip the default marketing/layout content from the home route and remove the template **Header** / **Footer** components so the app starts from a minimal shell.

Apply **once** per new project, from the repo root, **after** [post-init-route-trim.md](post-init-route-trim.md).

## Required steps

### 1. Add "Hello Agent" in `<main>` for the index route

Open **`src/routes/index.tsx`**.

- Ensure **`<main>...</main>`** includes a child div with the exact text: **`<div>Hello Agent</div>`**.
- **Keep** the `<main>` element and any **attributes** on it (e.g. `className`, `id`).

### 2. Delete template chrome and demo files

If these files exist, **delete** them:

- **`src/components/Header.tsx`**
- **`src/components/Footer.tsx`**
- **`src/components/demo.FormComponent.tsx`**
- **`src/data/demo-table-data.ts`**
- **`src/hooks/demo.form-context.ts`**
- **`src/hooks/demo.form.ts`**
- **`src/lib/demo-store-devtools.tsx`**
- **`src/lib/demo-store.ts`**
- **`src/lib/utils.ts`** — delete only when it is not the shadcn **`cn`** helper (`@/lib/utils` imported by UI components). If it is the standard `cn` file, keep it and remove demo-only imports/usages instead.

### 3. Clean up `__root.tsx` (Header / Footer)

Open **`src/routes/__root.tsx`** and:

- Remove **imports** of `Header` and `Footer` (e.g. `@/components/Header`, `@/components/Footer`).
- Remove **all JSX usages**: **`<Header />`**, **`<Footer />`**, and any wrapper that only existed for the template chrome.
- Ensure the root layout still renders **`Outlet`** (or the route outlet pattern the template uses) so child routes work.

Then search the rest of the codebase (**`src/routes/index.tsx`**, **`src/components/`**, other routes) for any remaining imports or references to those deleted files and remove them.

### 4. Remove references to `demo.FormComponent`

Search for **`demo.FormComponent`**, **`FormComponent`**, or imports of **`@/components/demo.FormComponent`** (or relative paths to `demo.FormComponent.tsx`). Remove **imports** and **JSX usages** after the file is deleted.

### 5. Remove default template CSS (keep shadcn styles)

Remove template styling that ships with TanStack Start and keep only shadcn-compatible styles:

- Open **`src/styles.css`** (the global stylesheet imported by the app root).
- Delete all default TanStack/template CSS blocks (layout/demo styles, gradient/background showcase, template utility classes).
- Keep only shadcn/base design-token layers and utilities (`@tailwind base/components/utilities`, `@layer base`, CSS variables used by shadcn, dark-mode tokens, and minimal global resets required by shadcn).
- If both template and shadcn define overlapping root variables, keep the shadcn token set as the source of truth.

### 6. Verify

1. Run the project’s **ESLint / lint** script if present (e.g. `bun run lint` — verify in `package.json`).
2. Run **`bun run dev`** and confirm the app compiles and the home route renders without errors.

## Bad Example

Deleting `Header.tsx` but leaving `import { Header } from '@/components/Header'` or `<Header />` in **`__root.tsx`** — build fails with missing module or unused import errors.

## Good Example

**`__root.tsx`** has no Header/Footer imports or calls—only structural wrappers plus **`<Outlet />`**. **`index.tsx`** has `<main className="..."><div>Hello Agent</div></main>`. **`demo.FormComponent.tsx`** is gone and nothing imports it.

## Context

- If the template uses **different paths** (e.g. `components/layout/Header.tsx`), only remove files that **actually exist**; still remove **all** usages of those removed modules.
- If there is **no** `<main>` in `index.tsx`, add a minimal empty `<main></main>` only if the user wants a semantic main landmark; otherwise strip the template body per the closest equivalent (e.g. outer section) **only when** the user extends this rule.
