You are a senior software engineer at a FANG company. Your task is to scaffold a **modern TypeScript library boilerplate** targeting both Node.js and ESM consumers. The boilerplate must adhere to industry best practices used in large-scale FANG engineering teams.

✅ KEY REQUIREMENTS:

1. **Project Goal**:
   - Create a reusable, versioned TypeScript library intended to be published on npm.
   - The codebase should support strict type safety, testability, modularity, and long-term maintainability.
   - Compatible with Node.js (LTS), supports both CommonJS and ESM builds.
2. **Project Structure**:
```txt
/src/  
--index.ts  
/test/  
--example.test.ts  
dist/  
tsconfig.json  
package.json  
README.md  
.gitignore
```
3. **tsconfig.json (Best Practices)**:
- Output both ESM and CJS (consider separate configs or build step).
- Follow strictest type safety.
- Ensure compatibility with Node.js and modern editors like VSCode.
- Enable incremental builds for performance.
- Prevent accidental publishing of untyped code.
- Recommended config:
```json
{
	"compilerOptions": {
	  "target": "ES2020",
	  "module": "ESNext",
	  "lib": ["ES2020"],
	  "declaration": true,
	  "declarationMap": true,
	  "sourceMap": true,
	  "moduleResolution": "node",
	  "resolveJsonModule": true,
	  "esModuleInterop": true,
	  "allowSyntheticDefaultImports": true,
	  "strict": true,
	  "noImplicitAny": true,
	  "noUnusedLocals": true,
	  "noUnusedParameters": true,
	  "noFallthroughCasesInSwitch": true,
	  "forceConsistentCasingInFileNames": true,
	  "outDir": "dist",
	  "rootDir": "src",
	  "composite": true,
	  "incremental": true,
	  "types": [],
	  "skipLibCheck": true
	},
	"include": ["src"],
	"exclude": ["node_modules", "dist", "test"]
}
```
4. **package.json (Include the following)**:
- Fields:
  - `"type": "module"`
  - `"main"`: CJS entry point (e.g., `dist/index.cjs.js`)
  - `"module"`: ESM entry point (e.g., `dist/index.esm.js`)
  - `"types"`: Type declaration file
  - `"exports"` map for dual build
  - `"files"`: Include only `dist/` and relevant files
  - Repository metadata, author, license

- Scripts:
  ```json
  {
    "scripts": {
      "clean": "rimraf dist",
      "build": "tsc --build",
      "lint": "eslint src --ext .ts",
      "test": "vitest",
      "dev": "tsc --watch"
    }
  }
  ```
- DevDependencies:
  - TypeScript (`^5.x`)
  - ESLint with `@typescript-eslint`
  - `rimraf` for cleaning
  - Testing framework like `vitest` or `jest`
  - Optionally: Prettier for formatting

5. **Git Ignore**:
```sh
/dist/  
node_modules/  
.DS_Store  
*.log
```
6. **README.md**:
- Title, usage example, install instructions, contribution guide.

7. **Optional Enhancements**:
- Add `tsconfig.build.json` for production builds
- Lint + format Git hooks via Husky and Lint-Staged
- Setup ESLint + Prettier config files

➡️ Generate all the code and files necessary for this project as if you're initializing a real-world professional TypeScript library that would pass code review at Google, Facebook, or Netflix.