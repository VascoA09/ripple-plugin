---
name: ripple
description: Sets up @ripple/ui (Unit4's Ripple design system) in the current project, or generates a self-contained HTML prototype using Ripple tokens. Use when the user wants to install Ripple, scaffold a new Ripple project, add Ripple to an existing project, fix a broken Ripple installation, or create an HTML prototype with the Ripple design system.
---

Set up the Ripple design system (@ripple/ui) in the current project, or create a standalone HTML prototype.

## Step 0 — Ask intent first

Before doing anything, ask:

> "Are you a **developer** setting up Ripple in a React project, or do you want to create a **quick HTML prototype** using Ripple's look and feel — no coding required?"

- **Developer** → continue to Step 1 (React flows)
- **HTML prototype** → skip to Flow D

---

## Step 1 — Detect context (developer path only)

Check the current directory:

1. **No `package.json`** → New project. Ask: "Are you already inside your project folder, or should I create one?" Then follow Flow A.
2. **`package.json` exists, `@ripple/ui` already in `node_modules`** → Already installed. Follow Flow C.
3. **`package.json` exists, no `@ripple/ui`** → Existing project. Follow Flow B.

---

## Flow A — New project

**If the user is already inside their project folder**, scaffold into the current directory:

```bash
npm create vite@latest . -- --template react-ts
npm install --legacy-peer-deps
npm install https://github.com/VascoA09/Ripple --legacy-peer-deps
```

**If the user wants a new folder created**, ask for a project name then run:

```bash
npm create vite@latest [project-name] -- --template react-ts
cd [project-name]
npm install --legacy-peer-deps
npm install https://github.com/VascoA09/Ripple --legacy-peer-deps
```

Then make these file changes:

### src/main.tsx — replace entirely
```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import '@ripple/ui/style.css'
import './index.css'
import App from './App'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <div data-theme="light">
      <App />
    </div>
  </StrictMode>
)
```

### src/index.css — replace entirely
```css
/* Ripple reset — do not add :root variables here */
*, *::before, *::after { box-sizing: border-box; }
body { margin: 0; font-family: var(--font-family-base); color: var(--text); background: var(--bg-canvas); -webkit-font-smoothing: antialiased; }
#root { min-height: 100svh; }
```

### src/App.css — clear the file
```css
/* Add prototype-specific styles here — do not redefine Ripple tokens */
```

### src/App.tsx — replace with working starter
```tsx
import { StandardNavigation, Unit4Logo, Button, Input, Tag, Badge, Card, CardHeader, CardTitle, CardContent } from '@ripple/ui'
import { LayoutDashboard } from 'lucide-react'
import ripplePkg from '../node_modules/@ripple/ui/package.json'

export default function App() {
  return (
    <StandardNavigation
      nav={{
        logo: <Unit4Logo />,
        productName: 'My App',
        globalNavItems: [
          { id: 'home', label: 'Home', icon: <LayoutDashboard size={20} />, selected: true, onClick: () => {} },
        ],
      }}
    >
      <div style={{
        display:        'flex',
        flexDirection:  'column',
        alignItems:     'center',
        justifyContent: 'center',
        minHeight:      '100%',
        gap:            'var(--spacing-100)',
        padding:        'var(--spacing-200)',
      }}>
        <div style={{ display: 'flex', flexDirection: 'column', alignItems: 'center', gap: 'var(--spacing-75)', textAlign: 'center' }}>
          <Unit4Logo width={74} height={42} />
          <h1 className="typography-heading-l" style={{ margin: 0 }}>Welcome to Ripple</h1>
          <p className="typography-body" style={{ margin: 0, color: 'var(--text-soft)' }}>Unit4's design system for product UI</p>
        </div>

        <Card style={{ width: '100%', maxWidth: '400px' }}>
          <CardHeader>
            <CardTitle as="h2">Get started</CardTitle>
          </CardHeader>
          <CardContent>
            <div style={{ display: 'flex', flexDirection: 'column', gap: 'var(--spacing-150)' }}>
              <Input label="Email" placeholder="you@example.com" />
              <div style={{ display: 'flex', gap: 'var(--spacing-50)' }}>
                <Button variant="fill">Primary</Button>
                <Button variant="outline">Secondary</Button>
              </div>
              <div style={{ display: 'flex', gap: 'var(--spacing-50)', alignItems: 'center' }}>
                <Tag color="blue">Design</Tag>
                <Tag color="green">System</Tag>
                <Badge color="primary">3</Badge>
              </div>
            </div>
          </CardContent>
        </Card>

        <p className="typography-caption" style={{ margin: 0 }}>
          @ripple/ui v{ripplePkg.version}
        </p>
      </div>
    </StandardNavigation>
  )
}
```

---

## Flow B — Existing project

```bash
npm install https://github.com/VascoA09/Ripple --legacy-peer-deps
```

Then:

1. **Find the CSS entry files** — check `src/index.css` and `src/App.css` for `:root` variable blocks. Remove any that define `--text`, `--bg`, `--border` or similar — they clash with Ripple tokens.

2. **Find the main entry file** — check `src/main.tsx` or `src/main.jsx`. Add this import before any other CSS:
   ```tsx
   import '@ripple/ui/style.css'
   ```

3. **Find the root wrapper** — in `main.tsx`, add `data-theme="light"` to the outermost div wrapping the app. If there's no wrapper div, add one.

4. **Report every file changed.**

---

## Flow C — Already installed

Check that `node_modules/@ripple/ui/dist/style.css` exists.

- If yes: report the version from `node_modules/@ripple/ui/package.json` and confirm ready.
- If no: the install is broken. Run `npm cache clean --force && npm install https://github.com/VascoA09/Ripple --legacy-peer-deps`.

---

## Flow D — HTML prototype (no coding required)

This flow is for non-developers who want a ready-to-open HTML file that looks like a real Ripple UI.

### Step D1 — Understand what they want to prototype

Ask ONE question:

> "What would you like to prototype? For example: a login page, a dashboard, a form, a list view, a settings screen — or describe what you have in mind."

Wait for the answer before proceeding.

### Step D2 — Fetch the Ripple CSS

Fetch the compiled Ripple stylesheet and embed it inline in the HTML so the file is fully self-contained and works without internet access.

Fetch from:
```
https://raw.githubusercontent.com/VascoA09/Ripple/main/dist/style.css
```

If that fails, try:
```
https://cdn.jsdelivr.net/gh/VascoA09/Ripple@main/dist/style.css
```

If both fail, embed a minimal set of Ripple-compatible CSS variables as a fallback (see Fallback tokens below).

### Step D3 — Generate the HTML file

Create a self-contained `.html` file named after the prototype (e.g. `login.html`, `dashboard.html`).

**Required structure:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[Prototype name]</title>
  <style>
    /* === RIPPLE CSS (embedded) === */
    [paste fetched CSS here]

    /* === Page reset === */
    *, *::before, *::after { box-sizing: border-box; }
    body { margin: 0; font-family: var(--font-family-base); color: var(--text); background: var(--bg-canvas); -webkit-font-smoothing: antialiased; }
  </style>
</head>
<body data-theme="light">
  <!-- prototype content here -->
</body>
</html>
```

**Key rules for HTML prototypes:**
- `data-theme="light"` must always be on `<body>`
- Never define `:root` CSS variables — use only `var(--token-name)` from Ripple
- Use Ripple spacing tokens for all gaps and padding: `var(--spacing-50)` through `var(--spacing-400)`
- Use Ripple colour tokens: `var(--text)`, `var(--text-soft)`, `var(--bg-surface)`, `var(--bg-canvas)`, `var(--border)`
- Use Ripple typography classes where available: `typography-heading-l`, `typography-heading-m`, `typography-body`, `typography-caption`
- Build UI blocks (buttons, inputs, cards, nav) as plain HTML styled with Ripple tokens — no JavaScript frameworks needed

**Reference HTML patterns for common Ripple components:**

#### Button (fill)
```html
<button style="background: var(--color-primary); color: var(--text-on-primary); border: none; border-radius: var(--radius-m); padding: var(--spacing-75) var(--spacing-150); font-family: var(--font-family-base); font-size: var(--font-size-m); cursor: pointer;">
  Button label
</button>
```

#### Button (outline)
```html
<button style="background: transparent; color: var(--color-primary); border: 1px solid var(--color-primary); border-radius: var(--radius-m); padding: var(--spacing-75) var(--spacing-150); font-family: var(--font-family-base); font-size: var(--font-size-m); cursor: pointer;">
  Button label
</button>
```

#### Input with label
```html
<div style="display: flex; flex-direction: column; gap: var(--spacing-25);">
  <label style="font-size: var(--font-size-s); color: var(--text-soft);">Label</label>
  <input type="text" placeholder="Placeholder" style="border: 1px solid var(--border); border-radius: var(--radius-m); padding: var(--spacing-75) var(--spacing-100); font-family: var(--font-family-base); font-size: var(--font-size-m); color: var(--text); background: var(--bg-surface); outline: none;" />
</div>
```

#### Card
```html
<div style="background: var(--bg-surface); border: 1px solid var(--border); border-radius: var(--radius-l); padding: var(--spacing-200);">
  <h2 class="typography-heading-m" style="margin: 0 0 var(--spacing-100);">Card title</h2>
  <!-- card content -->
</div>
```

#### Top navigation bar
```html
<nav style="background: var(--bg-surface); border-bottom: 1px solid var(--border); padding: 0 var(--spacing-200); height: 56px; display: flex; align-items: center; gap: var(--spacing-150);">
  <span class="typography-heading-m" style="margin: 0;">App name</span>
  <a href="#" style="color: var(--text-soft); text-decoration: none; font-size: var(--font-size-m);">Home</a>
  <a href="#" style="color: var(--text-soft); text-decoration: none; font-size: var(--font-size-m);">Settings</a>
</nav>
```

#### Tag / badge
```html
<span style="background: var(--color-blue-subtle); color: var(--color-blue); border-radius: var(--radius-full); padding: var(--spacing-25) var(--spacing-75); font-size: var(--font-size-s);">Label</span>
```

### Step D4 — Save and present the file

Save the file to the workspace folder. Then tell the user:

> "Your prototype is ready — just double-click the file to open it in your browser. No installs needed."

---

### Fallback tokens (use only if CSS fetch fails)

```css
:root {
  --font-family-base: 'Inter', system-ui, sans-serif;
  --font-size-s: 12px; --font-size-m: 14px; --font-size-l: 16px;
  --text: #1a1a2e; --text-soft: #6b7280; --text-on-primary: #ffffff;
  --bg-canvas: #f4f5f7; --bg-surface: #ffffff;
  --border: #e5e7eb;
  --color-primary: #0f62fe;
  --color-blue: #0f62fe; --color-blue-subtle: #dbeafe;
  --color-green: #16a34a; --color-green-subtle: #dcfce7;
  --radius-m: 6px; --radius-l: 10px; --radius-full: 999px;
  --spacing-25: 2px; --spacing-50: 4px; --spacing-75: 6px;
  --spacing-100: 8px; --spacing-150: 12px; --spacing-200: 16px;
  --spacing-300: 24px; --spacing-400: 32px;
}
```

---

## Key rules (all flows)

- Always use `--legacy-peer-deps` (React flows)
- CSS import is `@ripple/ui/style.css` — never `@ripple/ui/dist/style.css` (React flows)
- `data-theme="light"` is required on the root wrapper (all flows)
- `lucide-react` installs automatically with Ripple — no separate install needed (React flows)
- Clear Vite template `:root` variables — they override Ripple tokens (React flows)
- Never define `:root` variables in HTML prototypes — use Ripple tokens only (Flow D)

---

## Always end with a summary

**React flows:** bullet list of what was done + next step (`npm run dev`) + reminder to use Ripple tokens. Full docs: https://github.com/VascoA09/Ripple/blob/main/SETUP.md

**HTML prototype flow:** confirm the file was saved, tell the user to double-click to open it, and offer to adjust any section or add more screens.
