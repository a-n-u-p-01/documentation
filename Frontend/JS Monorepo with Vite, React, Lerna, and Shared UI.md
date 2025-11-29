## **1. Monorepo Folder Structure**

```
monorepo2/
├─ packages/
│  ├─ a/              # React + Vite app A
│  │  ├─ src/
│  │  │  └─ App.jsx
│  │  └─ package.json
│  ├─ b/              # React + Vite app B
│  │  ├─ src/
│  │  │  └─ App.jsx
│  │  └─ package.json
│  └─ shared-ui/      # Shared JS components
│     ├─ src/
│     │  ├─ index.js
│     │  └─ Button.jsx
│     └─ package.json
├─ package.json       # Monorepo root
└─ vite.config.js     # Optional root config
```

**Explanation:**

- `packages/*` — This is where all your “projects” or “packages” live.
    
- `a/` and `b/` — These are your React applications.
    
- `shared-ui/` — A package containing shared React components (like Buttons).
    
- Root `package.json` manages **workspaces** and scripts for running all apps.
    

---

## **2. Root `package.json`**

```json
{
  "private": true,
  "workspaces": ["packages/*"],
  "scripts": {
    "dev:a": "cd packages/a && npm run dev",
    "dev:b": "cd packages/b && npm run dev",
    "dev:all": "npm-run-all --parallel dev:a dev:b"
  },
  "devDependencies": {
    "npm-run-all": "^4.1.5"
  }
}
```

**Explanation:**

- `"private": true` — Prevents accidental publishing of the root package.
    
- `"workspaces": ["packages/*"]` — Enables **npm Workspaces**, automatically linking local packages.
    
- `"dev:a"` / `"dev:b"` — Scripts to run individual apps.
    
- `"dev:all"` — Runs all apps **in parallel** using `npm-run-all`.
    
- `npm-run-all` — A utility to run multiple npm scripts simultaneously.
    

**Commands to install dependencies:**

```bash
npm install
```

This will automatically link the `shared-ui` package to apps `a` and `b`.

---

## **3. Lerna Setup (Optional but Useful)**

Install Lerna globally:

```bash
npm install -g lerna
```

Initialize:

```bash
lerna init
```

This creates a `lerna.json`:

```json
{
  "packages": ["packages/*"],
  "version": "independent"
}
```

**Explanation:**

- Lerna helps manage multiple packages, versions, and dependencies.
    
- `"version": "independent"` — Each package can have its own version.
    
- Lerna v9 automatically works with npm Workspaces; no need for `useWorkspaces`.
    

**Useful Lerna commands:**

- `lerna bootstrap` – Automatically links dependencies between packages (deprecated in v9, use npm workspaces).
    
- `lerna run <script>` – Runs a script in all packages.
    

---

## **4. Shared UI Package**

### `packages/shared-ui/package.json`

```json
{
  "name": "@my-org/shared-ui",
  "version": "1.0.0",
  "main": "src/index.js",
  "license": "MIT",
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}
```

**Explanation:**

- `name` — This is how other apps will import your package (`@my-org/shared-ui`).
    
- `main` — Entry point for the package (important for Vite and Node).
    

---

### `packages/shared-ui/src/Button.jsx`

```jsx
import React from 'react';

export function Button({ children }) {
  return (
    <button style={{ padding: '10px 20px', backgroundColor: 'blue', color: 'white', borderRadius: 5 }}>
      {children}
    </button>
  );
}
```

### `packages/shared-ui/src/index.js`

```js
export * from './Button';
```

**Explanation:**

- `index.js` is the “public API” of the package.
    
- Any component exported here can be imported in other apps.
    

---

## **5. React Apps A and B**

### `packages/a/src/App.jsx` (same for B)

```jsx
import React from 'react';
import { Button } from '@my-org/shared-ui';

export default function App() {
  return (
    <div>
      <h1>App A</h1>
      <Button>Click Me</Button>
    </div>
  );
}
```

**Explanation:**

- `@my-org/shared-ui` is the shared package.
    
- You can use all exported components from `shared-ui` in both apps.
    

---

### `packages/a/package.json` (similarly for B)

```json
{
  "name": "app-a",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite"
  },
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "@my-org/shared-ui": "1.0.0"
  }
}
```

**Explanation:**

- `private: true` prevents publishing.
    
- `@my-org/shared-ui` is added as a dependency to link the shared package.
    

---

## **6. Vite Configuration for Monorepo**

### `packages/a/vite.config.js` (similar for B)

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@my-org/shared-ui': path.resolve(__dirname, '../shared-ui/src')
    }
  },
  server: {
    fs: {
      // Allow Vite to serve files outside the app folder
      allow: [
        path.resolve(__dirname, '../shared-ui/src'),
        path.resolve(__dirname)
      ]
    }
  }
});
```

**Explanation:**

- `alias` — Allows importing from `@my-org/shared-ui` without writing relative paths.
    
- `fs.allow` — Fixes Vite’s error about serving files outside the root.
    
- `server.fs.allow` is **essential in monorepos**.
    

---

## **7. Running the Apps**

From root:

```bash
# Run App A
npm run dev:a

# Run App B
npm run dev:b

# Run both apps in parallel
npm run dev:all
```

- Vite dev servers will run independently but can share the same `shared-ui`.
    

---

## **8. Notes & Tips**

1. **No TypeScript required** — pure JS.
    
2. Add new shared packages in `packages/*` and reference in apps using workspace names.
    
3. Node version >= 20.19 is required for Vite.
    
4. Always run `npm install` in root to link workspaces.
    
5. You can add more apps or shared libraries without changing the root structure.
    

---

✅ **With this setup:**

- Both React apps share components from `shared-ui`.
    
- You can run them individually or in parallel.
    
- Monorepo structure is clean and scalable.
    
---