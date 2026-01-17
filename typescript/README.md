# TypeScript Boilerplate

A minimal, framework-agnostic TypeScript boilerplate configured with:

- **Bun** - Fast package manager & runtime
- **TypeScript** - Type safety
- **ESLint** - Code quality linting
- **Prettier** - Code formatting
- **Vitest** - Unit testing framework
- **Husky + lint-staged** - Git pre-commit hooks
- **Source control ready** - Optimized `.gitignore`

Perfect for scaffolding **Node.js backends**, **React apps**, **NestJS projects**, **CLI tools**, or **vanilla TypeScript projects**.

---

## Quick Start

### 1. Copy the Boilerplate

```bash
cp -r boilerplate-ts my-new-project
cd my-new-project
```

### 2. Install Dependencies

```bash
bun install
```

### 3. Initialize Git (if needed)

```bash
git init
```

### 4. Create your first file

```bash
mkdir src
touch src/index.ts
```

### 5. Run your project

```bash
bun dev
```

---

## Project Structure

```
my-new-project/
├── src/                 # Your source code
│   └── index.ts         # Entry point (customize per project)
├── tests/               # Test files (optional)
├── .eslintrc.config.mts # ESLint configuration (flat config)
├── .lintstagedrc        # lint-staged hooks
├── .prettierrc           # Prettier formatting rules
├── .gitignore           # Git ignore patterns
├── eslint.config.mts    # ESLint configuration
├── tsconfig.json        # TypeScript configuration
├── vitest.config.ts     # Vitest testing configuration
├── package.json         # Dependencies & scripts
└── .husky/
    └── pre-commit       # Git pre-commit hook
```

---

## Available Scripts

### Development

```bash
# Watch files and run your code (customize the entry point!)
bun dev

# Type check your code
bun typecheck

# Format code with Prettier
bun format

# Lint code with ESLint
bun lint

# Auto-fix linting errors
bun lint:fix
```

### Testing

```bash
# Run tests (watch mode by default)
bun test

# Run tests with visual UI dashboard
bun test:ui

# Generate coverage report
bun test:coverage
```

### Pre-Commit (Automatic)

On every `git commit`, the pre-commit hook runs automatically:

1. **lint-staged** - Runs ESLint and Prettier on staged files
2. **typecheck** - Type checks with TypeScript
3. **test** - Runs test suite

If any step fails, your commit is blocked (desired behavior).

---

## Framework-Specific Setup

### Plain TypeScript / Node.js Project

**No additional setup needed!** Your boilerplate is ready to use.

Update the `dev` script in `package.json` to point to your entry file:

```json
"dev": "bun --watch src/index.ts"
```

Create `src/index.ts`:

```typescript
console.log("Hello, TypeScript!");
```

Run:

```bash
bun dev
```

---

### React + Vite

React requires additional tooling. Here's the complete setup:

#### 1. Install React and Vite

```bash
bun add react react-dom
bun add -d @vitejs/plugin-react vite
```

#### 2. Create `vite.config.ts`

```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
});
```

#### 3. Update `tsconfig.json`

Add to `compilerOptions`:

```json
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "jsxImportSource": "react",
    "lib": ["ES2020", "DOM", "DOM.Iterable"]
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
```

#### 4. Create `src/main.tsx`

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = document.getElementById("root");
if (root) {
  ReactDOM.createRoot(root).render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
  );
}
```

#### 5. Create `src/App.tsx`

```typescript
export default function App() {
  return <h1>Hello, React!</h1>;
}
```

#### 6. Create `index.html` (in project root)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

#### 7. Update `package.json` scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

#### 8. Update Vitest for React (jsdom environment)

Modify `vitest.config.ts`:

```typescript
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: "jsdom",
    setupFiles: ["./src/vitest.setup.ts"],
  },
});
```

#### 9. Create `src/vitest.setup.ts`

```typescript
import "@testing-library/jest-dom";
import { expect, afterEach, vi } from "vitest";
import { cleanup } from "@testing-library/react";

expect.extend({});

afterEach(() => {
  cleanup();
});
```

#### 10. Install React Testing Library

```bash
bun add -d @testing-library/react @testing-library/jest-dom jsdom
```

#### 11. Update `tsconfig.json` types

Add to `compilerOptions.types`:

```json
{
  "compilerOptions": {
    "types": ["vitest/globals", "@testing-library/jest-dom"]
  }
}
```

#### 12. Create your first component test

`src/App.test.tsx`:

```typescript
import { render, screen } from "@testing-library/react";
import App from "./App";

describe("App", () => {
  it("should render heading", () => {
    render(<App />);
    expect(screen.getByRole("heading")).toHaveTextContent("Hello, React!");
  });
});
```

#### 13. Run your app

```bash
bun dev
```

✅ **That's it!** React + Vite + Vitest is now configured.

---

### NestJS Backend

NestJS is opinionated and handles most setup automatically.

#### 1. Install NestJS CLI

```bash
bun add -g @nestjs/cli
```

#### 2. Create NestJS app in current directory

```bash
nest new . --package-manager bun --strict
```

This will overwrite some files, but that's okay—your boilerplate config will still work.

#### 3. Update `dev` script

```json
"dev": "nest start --watch"
```

#### 4. Run

```bash
bun dev
```

✅ **NestJS is ready!** Your boilerplate's ESLint, Prettier, Vitest configs work seamlessly with NestJS.

---

### Next.js App Router

Next.js provides its own setup and opinionates many decisions.

#### 1. Use Next.js create-next-app instead

```bash
bunx create-next-app@latest my-app --typescript --bun
```

**Note:** Next.js scaffolds its own linting and testing setup. Your boilerplate provides a good foundation, but Next.js has stronger opinions.

---

### Express.js Backend

Lightweight and minimal setup.

#### 1. Install Express

```bash
bun add express
bun add -d @types/express @types/node
```

#### 2. Create `src/index.ts`

```typescript
import express from "express";

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello, Express!");
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

#### 3. Update `dev` script

```json
"dev": "bun --watch src/index.ts"
```

#### 4. Run

```bash
bun dev
```

✅ **Express is ready!** Your boilerplate's ESLint, Prettier, Vitest configs work out of the box.

---

### Fastify Backend

Similar to Express but faster.

#### 1. Install Fastify

```bash
bun add fastify
bun add -d @types/node
```

#### 2. Create `src/index.ts`

```typescript
import Fastify from "fastify";

const fastify = Fastify({ logger: true });

fastify.get("/", async (request, reply) => {
  return { hello: "world" };
});

fastify.listen({ port: 3000 }, (err, address) => {
  if (err) throw err;
});
```

#### 3. Update `dev` script

```json
"dev": "bun --watch src/index.ts"
```

#### 4. Run

```bash
bun dev
```

✅ **Fastify is ready!**

---

## What Happens on Git Commit

Your pre-commit hook runs automatically:

```bash
git commit -m "Your message"
```

Behind the scenes:

1. **lint-staged** lints and formats only changed files
2. **TypeScript** checks for type errors
3. **Vitest** runs your tests

If any step fails, commit is blocked:

```
✖ Linting failed
✖ Type errors found
✖ Tests failed
```

Fix the issues and try again!

---

## Configuration Details

### ESLint (`eslint.config.mts`)

Lints JavaScript, TypeScript, JSON, Markdown, and CSS.

- Framework-agnostic (no React, Vue, etc. presets)
- Both browser and Node.js globals included
- Automatically disables conflicting Prettier rules

**Run manually:**

```bash
bun lint          # Check for issues
bun lint:fix      # Auto-fix issues
```

### Prettier (`.prettierrc`)

Opinionated but minimal formatting:

- Double quotes (your preference)
- Trailing commas (your preference)
- 100-char line width
- 2-space indentation
- Unix line endings (LF)

**Run manually:**

```bash
bun format
```

### Vitest (`vitest.config.ts`)

Unit testing framework with:

- Global test helpers (`describe`, `it`, `expect`)
- Node.js test environment (customize for React with `jsdom`)
- Coverage reporting
- Watch mode by default

**Run tests:**

```bash
bun test          # Watch mode
bun test:ui       # Visual dashboard
bun test:coverage # Coverage report
```

### TypeScript (`tsconfig.json`)

Strict type checking enabled. Customize `lib`, `target`, and `jsx` based on your framework.

---

## Customization Checklist

After scaffolding a new project, consider:

- [ ] Update `package.json` name and description
- [ ] Adjust `dev` script for your framework (Express, NestJS, React, etc.)
- [ ] Modify `.gitignore` for framework-specific files (commented examples included)
- [ ] Customize Prettier rules in `.prettierrc` if needed
- [ ] Add ESLint rules in `eslint.config.mts` for your framework (e.g., React plugin)
- [ ] Create `src/` folder structure
- [ ] Initialize Git: `git init`
- [ ] Install dependencies: `bun install`

---

## Troubleshooting

### `bun run format` fails

Make sure to include the path:

```bash
bun format  # ❌ Missing path
bun run format .  # ✅ Correct
```

### ESLint doesn't catch React errors

React requires ESLint plugin. Install it:

```bash
bun add -d eslint-plugin-react eslint-plugin-react-hooks
```

Update `eslint.config.mts` to add React plugin.

### Vitest tests don't run

Ensure test files match the pattern:

```
src/**/*.test.ts
src/**/*.spec.ts
```

### Pre-commit hook doesn't run

Ensure Husky is initialized:

```bash
bun run prepare
```

---

## Next Steps

1. **Choose your framework** and follow the guide above
2. **Create your first file** in `src/`
3. **Write a test** in `src/` with `.test.ts` suffix
4. **Run your project:** `bun dev`
5. **Commit code:** `git commit -m "Initial commit"`

Happy coding! 🚀
