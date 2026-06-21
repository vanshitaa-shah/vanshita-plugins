---
description: Generate a React component
argument-hint: <ComponentName>
---

Create a new React functional component named `$ARGUMENTS`.

Requirements:
- TypeScript functional component with a typed `${ARGUMENTS}Props` interface.
- Place it at `src/components/$ARGUMENTS/$ARGUMENTS.tsx`.
- Add a barrel `src/components/$ARGUMENTS/index.ts` re-exporting the component.
- Follow the project's existing styling approach (detect CSS Modules, styled-components, Tailwind, etc.).
- Reuse the conventions from the `react-patterns` skill if available.

If no component name was provided in `$ARGUMENTS`, ask the user for one first.
