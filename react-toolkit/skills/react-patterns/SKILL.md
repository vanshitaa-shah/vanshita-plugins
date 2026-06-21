---
name: react-patterns
description: Conventions for building React components — folder structure, prop
  typing, hooks rules, and styling. Use whenever creating or editing React
  components or reviewing component structure.
---

# React component patterns

Apply these conventions when creating or editing React components.

## Structure
- One folder per component: `src/components/<Name>/`.
- `<Name>.tsx` holds the component; `index.ts` is a barrel that re-exports it.
- Colocate tests as `<Name>.test.tsx` and stories as `<Name>.stories.tsx`.

## Components
- Functional components only; no class components.
- Props interface named `<Name>Props`, exported alongside the component.
- Destructure props in the signature; provide sensible defaults.
- Keep components presentational — push logic into custom hooks.

## Hooks
- Follow the rules of hooks (only call at the top level).
- Extract reusable stateful logic into `use*` hooks under `src/hooks/`.
- Always clean up subscriptions/timers in `useEffect` return functions.

## Styling
- Detect and match the project's existing approach (CSS Modules,
  styled-components, Tailwind, etc.) — do not introduce a new one.

## Accessibility
- Use semantic elements; add `aria-*` only when semantics are insufficient.
- Ensure interactive elements are keyboard reachable.
