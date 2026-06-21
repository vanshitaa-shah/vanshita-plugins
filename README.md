# vanshita-plugins

A Claude Code plugin marketplace.

## Plugins

### react-toolkit
Scaffold React components and hooks with project conventions, plus specialist
agents to build and review them.

- **Commands:** `/gen-component <Name>`, `/gen-hook <useName>`
- **Skills:** `react-patterns`, `testing-conventions` (auto-loaded when relevant)
- **Agents:** `component-builder`, `component-reviewer`

## Install

```
/plugin marketplace add vanshitaa-shah/vanshita-plugins
/plugin install react-toolkit@vanshita-plugins
```

## Usage

```
/gen-component UserCard      # scaffold a component
/gen-hook useToggle          # scaffold a custom hook
```

Ask Claude to "build" or "review" a React component and it will delegate to the
`component-builder` / `component-reviewer` agents automatically.
