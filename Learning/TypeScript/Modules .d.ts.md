## Comparing JavaScript to an example DTS

### Common CommonJS Patterns
A module using CommonJS patterns uses `module.exports` to describe the exported values. For example, here is a module which exports a function and a numerical constant:
```js
const maxInterval = 12;
function getArrayLength(arr) {
  return arr.length;
}
module.exports = {
  getArrayLength,
  maxInterval,
};
```
This can be described by the following `.d.ts`:
```ts
export function getArrayLength(arr: any[]): number;
export const maxInterval: 12;
```

The TypeScript playground can show you the `.d.ts` equivalent for JavaScript code. You can [try it yourself here](https://www.typescriptlang.org/play?useJavaScript=true#code/GYVwdgxgLglg9mABAcwKZQIICcsEMCeAMqmMlABYAUuOAlIgN6IBQiiW6IWSNWAdABsSZcswC+zCAgDOURAFtcADwAq5GKUQBeRAEYATM2by4AExBC+qJQAc4WKNO2NWKdNjxFhFADSvFquqk4sxAA).

The `.d.ts` syntax intentionally looks like [ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) syntax. ES Modules was ratified by TC39 in 2015 as part of ES2015 (ES6), while it has been available via transpilers for a long time, however if you have a JavaScript codebase using ES Modules:
```ts
export function getArrayLength(arr) {
  return arr.length;
}
```
This would have the following `.d.ts` equivalent:
```ts
export function getArrayLength(arr: any[]): number;
```

### Default Exports
In CommonJS you can export any value as the default export, for example here is a regular expression module:
```js
module.exports = /hello( world)?/;
```
Which can be described by the following .d.ts:
```ts
declare const helloWorld: RegExp;
export = helloWorld;
```
Or a number:
```js
module.exports = 3.142;
```
```ts
declare const pi: number;
export = pi;
```
One style of exporting in CommonJS is to export a function. Because a function is also an object, then extra fields can be added and are included in the export.
```js
function getArrayLength(arr) {
  return arr.length;
}
getArrayLength.maxInterval = 12;
module.exports = getArrayLength;
```
Which can be described with:
```ts
declare function getArrayLength(arr: any[]): number;
declare namespace getArrayLength {
  declare const maxInterval: 12;
}
export = getArrayLength;
```
See [Module: Functions](https://www.typescriptlang.org/docs/handbook/declaration-files/templates/module-function-d-ts.html) for details of how that works, and the [Modules reference](https://www.typescriptlang.org/docs/handbook/modules.html) page.

### Handling Many Consuming Import
There are many ways to import a module in modern consuming code:
```ts
const fastify = require("fastify");
const { fastify } = require("fastify");
import fastify = require("fastify");
import * as Fastify from "fastify";
import { fastify, FastifyInstance } from "fastify";
import fastify from "fastify";
import fastify, { FastifyInstance } from "fastify";
```
Covering all of these cases requires the JavaScript code to actually support all of these patterns. To support many of these patterns, a CommonJS module would need to look something like:
```js
class FastifyInstance {}
function fastify() {
  return new FastifyInstance();
}
fastify.FastifyInstance = FastifyInstance;
// Allows for { fastify }
fastify.fastify = fastify;
// Allows for strict ES Module support
fastify.default = fastify;
// Sets the default export
module.exports = fastify;
```

### Types in Modules
You may want to provide a type for JavaScript code which does not exist
```js
function getArrayMetadata(arr) {
  return {
    length: getArrayLength(arr),
    firstObject: arr[0],
  };
}
module.exports = {
  getArrayMetadata,
};
```
This can be described with:
```ts
export type ArrayMetadata = {
  length: number;
  firstObject: any | undefined;
};
export function getArrayMetadata(arr: any[]): ArrayMetadata;
```
This example is a good case for [using generics](https://www.typescriptlang.org/docs/handbook/generics.html#generic-types) to provide richer type information:
```ts
export type ArrayMetadata<ArrType> = {
  length: number;
  firstObject: ArrType | undefined;
};
export function getArrayMetadata<ArrType>(
  arr: ArrType[]
): ArrayMetadata<ArrType>;
```
Now the type of the array propagates into the `ArrayMetadata` type.

The types which are exported can then be re-used by consumers of the modules using either `import` or `import type` in TypeScript code or [JSDoc imports](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html#import-types).

### Namespaces in Modules
Trying to describe the runtime relationship of JavaScript code can be tricky. When the ES Module-like syntax doesn’t provide enough tools to describe the exports then you can use `namespaces`.

For example, you may have complex enough types to describe that you choose to namespace them inside your `.d.ts`:
```ts
// This represents the JavaScript class which would be available at runtime
export class API {
  constructor(baseURL: string);
  getInfo(opts: API.InfoRequest): API.InfoResponse;
}
// This namespace is merged with the API class and allows for consumers, and this file
// to have types which are nested away in their own sections.
declare namespace API {
  export interface InfoRequest {
    id: string;
  }
  export interface InfoResponse {
    width: number;
    height: number;
  }
}
```
To understand how namespaces work in `.d.ts` files read the [`.d.ts` deep dive](https://www.typescriptlang.org/docs/handbook/declaration-files/deep-dive.html).
#### Optional Global Usage
You can use `export as namespace` to declare that your module will be available in the global scope in UMD contexts:
```ts
export as namespace moduleName;
```
