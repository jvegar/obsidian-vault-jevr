#JavaScript #modules


JavaScript has a long history of different ways to handle modularizing code. Having been around since **2012**, TypeScript has implemented support for a lot of these formats, but over time the community and the JavaScript specification has **==converged==** on a format called **ES Modules** (or ES6 modules). You might know it as the `import`/`export` syntax.

ES Modules was added to the JavaScript spec in **2015**, and by 2020 had broad support in most web browsers and JavaScript runtimes.

For focus, the handbook will cover both ES Modules and its popular pre-cursor _CommonJS_ `module.exports =` syntax, and you can find information about the other module patterns in the reference section under [Modules](https://www.typescriptlang.org/docs/handbook/modules.html).
## 📋 Table of Contents

- [[#How JavaScript Modules are Defined|How JavaScript Modules are Defined]]
- [[#Non-modules|Non-modules]]
- [[#Modules in TypeScript|Modules in TypeScript]]
  - [[#ES Module Syntax|ES Module Syntax]]
  - [[#Additional Import Syntax|Additional Import Syntax]]
    - [[#TypeScript Specific ES Module Syntax|TypeScript Specific ES Module Syntax]]
    - [[#ES Module Syntax with CommonJS Behavior|ES Module Syntax with CommonJS Behavior]]
- [[#CommonJS Syntax|CommonJS Syntax]]
    - [[#Exporting|Exporting]]
  - [[#CommonJS and ES Modules interop|CommonJS and ES Modules interop]]
- [[#TypeScript’s Module Resolution Options|TypeScript’s Module Resolution Options]]
- [[#TypeScript’s Module Output Options|TypeScript’s Module Output Options]]
    - [[#ES2020|ES2020]]
    - [[#CommonJS|CommonJS]]
    - [[#UMD|UMD]]
- [[#TypeScript namespaces|TypeScript namespaces]]

---

## How JavaScript Modules are Defined

In TypeScript, just as in ECMAScript 2015, ==_any file containing a top-level `import` or `export` is considered a module_==.

Conversely, _a file without any top-level import or export declarations is treated as a script_ whose contents are available in the global scope (and therefore to modules as well).

_Modules_ are **executed within their own scope**, not in the global scope. This means that variables, functions, classes, etc. declared in a module are not visible outside the module unless they are explicitly exported using one of the export forms. Conversely, to consume a variable, function, class, interface, etc. exported from a different module, it has to be imported using one of the import forms.

## Non-modules

Before we start, it’s important to understand what TypeScript considers a module. The JavaScript specification declares that _==any JavaScript files without an `import` declaration, `export`, or top-level `await` should be considered a **script** and not a module==_.

Inside a _script file variables and types are declared to be in the **shared global scope**_, and it’s assumed that you’ll either _use the [`outFile`](https://www.typescriptlang.org/tsconfig#outFile) compiler option to join **multiple input files** into one output file_, or use multiple `<script>` tags in your HTML to load these files (in the correct order!).

If you have a file that doesn’t currently have any `import`s or `export`s, but you want to be treated as a module, add the line:

`   export {};   `[Try](https://www.typescriptlang.org/play/#code/KYDwDg9gTgLgBAbwL4G4g)

which will change the file to be a module exporting nothing. This syntax works regardless of your module target.

## Modules in TypeScript

> Additional Reading:  
> [Impatient JS (Modules)](https://exploringjs.com/impatient-js/ch_modules.html#overview-syntax-of-ecmascript-modules)  
> [MDN: JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)  

There are three main things to consider when writing module-based code in TypeScript:

- **Syntax**: What syntax do I want to use to import and export things?
- **Module Resolution**: What is the relationship between module names (or paths) and files on disk?
- **Module Output Target**: What should my emitted JavaScript module look like?

### ES Module Syntax

A file can declare a _main export_ via **`export default`**:

```TS
// @filename: hello.ts
export default function helloWorld() {    
	console.log("Hello, world!");  
}
```   
This is then imported via:
```TS
import helloWorld from "./hello.js";
helloWorld();
```

In addition to the default export, you can have more than one export of variables and functions via the `export` by omitting `default`:
```TS
// @filename: maths.ts
export var pi = 3.14;
export let squareTwo = 1.41;
export const phi = 1.61;
 
export class RandomNumberGenerator {}
 
export function absolute(num: number) {
  if (num < 0) return num * -1;
  return num;
}
```
These can be used in another file via the `import` syntax:
```TS
import { pi, phi, absolute } from "./maths.js";
 
console.log(pi);
const absPhi = absolute(phi);
```
### Additional Import Syntax

An import can be renamed using a format like `import {old as new}`:
```TS
import { pi as π } from "./maths.js";
 
console.log(π);
```
You can mix and match the above syntax into a single `import`:
```TS
// @filename: maths.ts
export const pi = 3.14;
export default class RandomNumberGenerator {}
 
// @filename: app.ts
import RandomNumberGenerator, { pi as π } from "./maths.js";
 
RandomNumberGenerator; 
console.log(π);
```

You can take all of the exported objects and put them into a single namespace using `* as name`:
```TS
// @filename: app.ts
import * as math from "./maths.js";
 
console.log(math.pi);
const positivePhi = math.absolute(math.phi);
```

You can import a file and _not_ include any variables into your current module via `import "./file"`:
```TS
// @filename: app.ts
import "./maths.js";
 
console.log("3.14");
```
In this case, the `import` does nothing. However, all of the code in `maths.ts` was evaluated, which could trigger side-effects which affect other objects.

#### TypeScript Specific ES Module Syntax

Types can be exported and imported using the same syntax as JavaScript values:
```TS
// @filename: animal.ts
export type Cat = { breed: string; yearOfBirth: number };
 
export interface Dog {
  breeds: string[];
  yearOfBirth: number;
}
 
// @filename: app.ts
import { Cat, Dog } from "./animal.js";
type Animals = Cat | Dog;
```
TypeScript has extended the `import` syntax with two concepts for declaring an import of a type:
```TS
import type
```
Which is an import statement which can _only_ import types:
```TS
// @filename: animal.ts
export type Cat = { breed: string; yearOfBirth: number };
export type Dog = { breeds: string[]; yearOfBirth: number };
export const createCatName = () => "fluffy"; 
// @filename: valid.ts
import type { Cat, Dog } from "./animal.js";
export type Animals = Cat | Dog; 
// @filename: app.ts
import type { createCatName } from "./animal.js";
const name = createCatName();
/*
'createCatName' cannot be used as a value because it was imported using 'import type'
*/.
```

```TS
inline type imports
```

TypeScript 4.5 also allows for individual imports to be prefixed with `type` to indicate that the imported reference is a type:
```TS
// @filename: app.ts
import { createCatName, type Cat, type Dog } from "./animal.js";
 
export type Animals = Cat | Dog;
const name = createCatName();
```
Together these allow a non-TypeScript transpiler like _Babel_, _swc_ or _esbuild_ to know what imports can be safely removed.

#### ES Module Syntax with CommonJS Behavior

TypeScript has ES Module syntax which _directly_ correlates to a CommonJS and AMD `require`. Imports using ES Module are _for most cases_ the same as the `require` from those environments, but this syntax ensures you have a 1 to 1 match in your TypeScript file with the CommonJS output:
```TS
import fs = require("fs");
const code = fs.readFileSync("hello.ts", "utf8");
```
You can learn more about this syntax in the [modules reference page](https://www.typescriptlang.org/docs/handbook/modules.html#export--and-import--require).

## CommonJS Syntax

CommonJS is the format which most modules on npm are delivered in. Even if you are writing using the ES Modules syntax above, having a brief understanding of how CommonJS syntax works will help you debug easier.
#### Exporting

Identifiers are exported via setting the `exports` property on a global called `module`.
```TS
function absolute(num: number) {
  if (num < 0) return num * -1;
  return num;
}
 
module.exports = {
  pi: 3.14,
  squareTwo: 1.41,
  phi: 1.61,
  absolute,
};
```
Then these files can be imported via a `require` statement:
```TS
const maths = require("./maths");
maths.pi;
```
Or you can simplify a bit using the destructuring feature in JavaScript:
```TS
const { squareTwo } = require("./maths");
squareTwo;
```
### CommonJS and ES Modules interop

There is a mis-match in features between CommonJS and ES Modules regarding the distinction between a default import and a module namespace object import. TypeScript has a compiler flag to reduce the friction between the two different sets of constraints with [`esModuleInterop`](https://www.typescriptlang.org/tsconfig#esModuleInterop).
## TypeScript’s Module Resolution Options

==_Module resolution_== is the process of ==taking a string from the `import` or `require` statement, and determining what file that string refers to==.

TypeScript includes two resolution strategies: Classic and Node. Classic, the default when the compiler option [`module`](https://www.typescriptlang.org/tsconfig#module) is not `commonjs`, is included for backwards compatibility. The Node strategy replicates how Node.js works in CommonJS mode, with additional checks for `.ts` and `.d.ts`.

There are many TSConfig flags which influence the module strategy within TypeScript: [`moduleResolution`](https://www.typescriptlang.org/tsconfig#moduleResolution), [`baseUrl`](https://www.typescriptlang.org/tsconfig#baseUrl), [`paths`](https://www.typescriptlang.org/tsconfig#paths), [`rootDirs`](https://www.typescriptlang.org/tsconfig#rootDirs).

For the full details on how these strategies work, you can consult the [Module Resolution](https://www.typescriptlang.org/docs/handbook/modules/reference.html#the-moduleresolution-compiler-option) reference page.
## TypeScript’s Module Output Options

There are two options which affect the emitted JavaScript output:

- [`target`](https://www.typescriptlang.org/tsconfig#target) which determines which JS features are downleveled (converted to run in older JavaScript runtimes) and which are left intact
- [`module`](https://www.typescriptlang.org/tsconfig#module) which determines what code is used for modules to interact with each other

Which [`target`](https://www.typescriptlang.org/tsconfig#target) you use is determined by the features available in the JavaScript runtime you expect to run the TypeScript code in. That could be: the oldest web browser you support, the lowest version of Node.js you expect to run on or could come from unique constraints from your runtime - like Electron for example.

All communication between modules happens via a module loader, the compiler option [`module`](https://www.typescriptlang.org/tsconfig#module) determines which one is used. At runtime the module loader is responsible for locating and executing all dependencies of a module before executing it.

For example, here is a TypeScript file using ES Modules syntax, showcasing a few different options for [`module`](https://www.typescriptlang.org/tsconfig#module):
```TS
import { valueOfPi } from "./constants.js";
 
export const twoPi = valueOfPi * 2;
```
#### `ES2020`
```TS
import { valueOfPi } from "./constants.js";
export const twoPi = valueOfPi * 2;
```
#### `CommonJS`
```JS
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.twoPi = void 0;
const constants_js_1 = require("./constants.js");
exports.twoPi = constants_js_1.valueOfPi * 2;
```
#### `UMD`
```JS
(function (factory) {
    if (typeof module === "object" && typeof module.exports === "object") {
        var v = factory(require, exports);
        if (v !== undefined) module.exports = v;
    }
    else if (typeof define === "function" && define.amd) {
        define(["require", "exports", "./constants.js"], factory);
    }
})(function (require, exports) {
    "use strict";
    Object.defineProperty(exports, "__esModule", { value: true });
    exports.twoPi = void 0;
    const constants_js_1 = require("./constants.js");
    exports.twoPi = constants_js_1.valueOfPi * 2;
});
```

> Note that ES2020 is effectively the same as the original `index.ts`.

You can see all of the available options and what their emitted JavaScript code looks like in the [TSConfig Reference for `module`](https://www.typescriptlang.org/tsconfig#module).

## TypeScript namespaces

TypeScript has its own module format called `namespaces` which pre-dates the ES Modules standard. This syntax has a lot of useful features for creating complex definition files, and still sees active use [in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped). While not deprecated, the majority of the features in namespaces exist in ES Modules and we recommend you use that to align with JavaScript’s direction. You can learn more about namespaces in [the namespaces reference page](https://www.typescriptlang.org/docs/handbook/namespaces.html).