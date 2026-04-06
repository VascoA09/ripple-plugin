# ripple-plugin

A [Claude Code](https://claude.ai/code) plugin that sets up [@ripple/ui](https://github.com/VascoA09/Ripple) — Unit4's Ripple design system — in any project.

## Requirements

- [Node.js](https://nodejs.org) + npm
- [Claude Code](https://claude.ai/code) (terminal CLI — not the Claude desktop app)

## Install

The plugin is installed once per machine. Open a terminal in your project folder, start Claude Code, and run:

```
/plugin install github:VascoA09/ripple-plugin
```

## Usage

```
/ripple-plugin:setup-ripple
```

Claude will detect your project state and walk you through the appropriate setup flow.

## What it does

The plugin handles three scenarios automatically:

**New project** — Scaffolds a Vite + React + TypeScript project, installs `@ripple/ui`, and configures all required files with a working starter UI.

**Existing project** — Installs `@ripple/ui` into your project and integrates it without breaking your existing setup.

**Broken install** — Detects and repairs a failed or incomplete Ripple installation.

In all cases it ensures:
- `@ripple/ui/style.css` is imported before any other styles
- `data-theme="light"` is set on the root wrapper
- Conflicting Vite `:root` CSS variables are removed
- `resolveJsonModule` is enabled in `tsconfig.app.json`

## More

Full Ripple documentation → [github.com/VascoA09/Ripple](https://github.com/VascoA09/Ripple/blob/main/SETUP.md)
