# ripple-plugin

A Claude Code plugin to set up [@ripple/ui](https://github.com/VascoA09/Ripple) — Unit4's Ripple design system — in any project.

## Install

Inside any project folder, open Claude Code and run:

```
/plugin install github:VascoA09/ripple-plugin
```

## Usage

```
/ripple-plugin:setup-ripple
```

Claude will detect your project state and guide you through the full setup — new project, existing project, or broken install.

## What it does

- Scaffolds a new Vite + React + TypeScript project (if needed)
- Installs `@ripple/ui` from GitHub
- Configures `main.tsx`, `index.css`, `App.css`, and `App.tsx`
- Ensures `data-theme="light"` is set on the root wrapper
- Clears conflicting Vite `:root` CSS variables
- Verifies `resolveJsonModule` in `tsconfig.app.json`

## Requirements

- Node.js + npm
- Claude Code
