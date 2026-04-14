---
name: setup-ripple
description: Sets up @ripple/ui (Unit4's Ripple design system) in the current project. Use when the user wants to install Ripple, scaffold a new Ripple project, add Ripple to an existing project, or fix a broken Ripple installation.
---

Set up the Ripple design system (@ripple/ui) in the current project.

## Step 1 — Detect context

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

### tsconfig.app.json — ensure resolveJsonModule is enabled
After scaffolding, check `tsconfig.app.json`. If `resolveJsonModule` is not present under `compilerOptions`, add it:
```json
{
  "compilerOptions": {
    "resolveJsonModule": true
  }
}
```
This is required for importing `package.json` to display the Ripple version.

---

## Key rules

- Always use `--legacy-peer-deps`
- CSS import is `@ripple/ui/style.css` — never `@ripple/ui/dist/style.css`
- `data-theme="light"` is required on the root wrapper
- `lucide-react` installs automatically with Ripple — no separate install needed
- Clear Vite template `:root` variables — they override Ripple tokens

---

## Always end with a summary

- What was done (bullet list)
- Next step: `npm run dev` or `cd [project] && npm run dev`
- Reminder: "Use Ripple tokens for all styles: `var(--text)`, `var(--bg-surface)`, `var(--spacing-100)`. Full docs: https://github.com/VascoA09/Ripple/blob/main/SETUP.md"
