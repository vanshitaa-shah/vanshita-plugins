---
name: testing-conventions
description: Conventions for testing React components and hooks with React
  Testing Library and Jest/Vitest. Use whenever writing or updating tests for
  React code.
---

# React testing conventions

Apply these when writing tests for components or hooks.

## Tooling
- Use React Testing Library with the project's runner (Jest or Vitest — detect
  which from the repo config).
- Colocate test files next to the source: `<Name>.test.tsx` / `<name>.test.ts`.

## Component tests
- Query by accessible role/label, not by test IDs or class names.
- Test behavior the user observes, not implementation details.
- Cover: default render, prop variations, and key interactions.

## Hook tests
- Use `renderHook` to exercise hooks in isolation.
- Assert on returned values and on state transitions after `act(...)`.

## Style
- One top-level `describe` per unit; clear `it('does X when Y')` names.
- Avoid snapshot tests except for stable, presentational output.
