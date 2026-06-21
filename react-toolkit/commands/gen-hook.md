---
description: Generate a custom React hook
argument-hint: <useHookName>
---

Create a new custom React hook named `$ARGUMENTS`.

Requirements:
- The name must start with `use` (e.g. `useToggle`). If it doesn't, prefix it.
- TypeScript, with typed arguments and return value.
- Place it at `src/hooks/$ARGUMENTS.ts`.
- Add a colocated test file `src/hooks/$ARGUMENTS.test.ts` following the `testing-conventions` skill if available.
- Keep the hook pure and side-effect-aware (cleanup in `useEffect` where relevant).

If no hook name was provided in `$ARGUMENTS`, ask the user what the hook should do.
