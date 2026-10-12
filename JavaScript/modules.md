# Modules in Javascript

Modules are fundamental to maintain clean code. A module is a **file** that has related functions, variables, or classes.
Modules are then imported into other files instead of having all your code in one file. **This helps with overall readability of code.**

There are two different types of modules in JS: CommonJS (legacy) & ES Modules

## CommonJS

CommonJS was developed in 2009 by Kevin Dangoor. It is a set of standards to implement modules.

CommonJS was designed for server-side JavaScript, like Node, so browsers don't support it.

Here is an example of a single function being a module:

```js
// test.js
const test = () => console.log("Test");
module.exports = test

// index.js
const test = require("./test.js")
test()
```

If you wanted more to be included in a module, then this is how you do it:

```js
// test.js
const test = () => console.log("Test");
const test2 = () => console.log("Test2");
module.exports = { test, test2 }

// index.js
const { test, test2 } = require("./test.js")
test()
```

## ES Modules

ECMAScript Modules (also known as ESModules) is a standard introduced in 2015 with ES6. It standardized how JS Modules work. This allows browsers to use it as well

Lets look at the updated code:

```js
// test.js
const test = () => console.log("Test");
export { test }

// index.js
import { test } from './test.js'
test()
```

```js
// test.js
const test = () => console.log("Test");
const test2 = () => console.log("Test2");
export { test, test2 }

// index.js
import { test, test2 } from './test.js'
test()
```

### Named vs default exports

Everything above uses **named exports** — the importer has to use the exact same name, in braces.

You also usually don't write the `export { ... }` block at the bottom. Just tag the declaration itself:

```js
// test.js
export const test = () => console.log("Test");
export function test2() { console.log("Test2"); }
```

Default exports are very common. A default export is the "main thing" a module provides. There's at most one per file, and the importer picks whatever name it likes:

```js
// test.js
export default function test() { console.log("Test"); }

// index.js
import whateverIWant from './test.js'   // no braces
```

CommonJS's `module.exports = test` is really the equivalent of `export default`, not of `export { test }`.

### Other import forms

```js
import * as utils from './utils.js'      // namespace: everything on one object
utils.test()

const mod = await import('./heavy.js')   // dynamic import, returns a Promise
```

Static `import` can't go inside an `if` or a function — it's top level only. `import()` is the escape hatch, and it's how bundlers do code splitting (only download a module when it's actually needed).

## Turning ESM on in Node

Node assumes CommonJS by default. To use ESM, either:

- add `"type": "module"` to `package.json` (makes every `.js` file in the package ESM), or
- name the file `.mjs`. Going the other way, `.cjs` forces CommonJS even inside a `"type": "module"` package.

Gotcha: in ESM, relative imports need the **file extension**. `import x from './test'` fails; it has to be `'./test.js'`. CommonJS lets you leave it off because `require` tries a list of extensions for you.

## CommonJS vs ES Modules

The syntax is the surface difference. The ones that actually matter:

| | CommonJS | ES Modules |
| --- | --- | --- |
| Resolution | at runtime, when `require()` runs | statically, at parse time, before any code runs |
| Placement | anywhere (inside `if`, inside a function) | top level only |
| Loading | synchronous | asynchronous |
| Bindings | copies the value out | *live bindings* — importer sees later reassignments |
| Top-level `await` | no | yes |
| Tree-shaking | hard | possible |

**Live bindings** are the surprising one. An ESM import is a live view of the exporter's variable, not a snapshot:

```js
// counter.js
export let count = 0;
export const inc = () => count++;

// index.js
import { count, inc } from './counter.js'
inc()
console.log(count)   // 1
```

The CommonJS version prints `0`, because `const { count } = require(...)` copied the number at import time.

### Reassignment vs mutation

- **Mutation** — changing a property *inside* an object: `config.port = 3000`
- **Reassignment** — pointing the variable at a whole new value: `count = 1`, `config = {}`

**Mutation always propagates, in both systems.** An object export is a reference. Everyone who imported it holds the same reference, so a property change is visible to all of them:

```js
// config.js (CommonJS)
module.exports = { port: 8080 };

// a.js
const config = require('./config.js');
config.port = 3000;

// b.js
const config = require('./config.js');
console.log(config.port);   // 3000 — same object, both systems agree
```

**Reassignment is where they diverge.** CommonJS copies the *value* of the binding at import time; ESM keeps a live link back to the exporter's variable.

```js
// counter.js (CommonJS)
let count = 0;
module.exports = { count, inc: () => count++ };

// index.js
const { count, inc } = require('./counter.js');
inc();
console.log(count);   // 0  ❌
```

Trace what happened: at the moment `module.exports = { count, ... }` ran, `count` was `0`, so the number `0` was copied into the object. `inc()` then increments the module's *own* `count` variable to 1 — but nothing goes back and updates that copy. You're looking at a stale snapshot.

The ESM version prints `1`, because `import { count }` doesn't copy anything. It creates a read-only alias pointing at `counter.js`'s actual `count` variable. Read it later, get the current value.

```js
// counter.js (ESM)
export let count = 0;
export const inc = () => count++;

// index.js
import { count, inc } from './counter.js';
inc();
console.log(count);   // 1  ✅
```

Note "read-only alias": the importer can't write to it. `count = 5` in `index.js` is a `TypeError` — only the module that owns the variable may reassign it. This is a feature; it means you can always find every write to an export by searching one file.

#### Why this matters in practice

The classic bug is a module that reassigns state during initialization:

```js
// db.js
let connection = null;
export function connect() { connection = createConnection(); }
export { connection };
```

With ESM this works — importers see `null` before `connect()` and the real connection after. Port the same file to CommonJS and every importer is permanently stuck with `null`, because they copied it before `connect()` ever ran.

The CommonJS workaround is to never export the value directly. Export a getter, or export the container and read through it:

```js
// db.js (CommonJS)
let connection = null;
module.exports = {
  connect() { connection = createConnection(); },
  get connection() { return connection; }   // read at access time, not import time
};
```

This is also why so much CommonJS code looks like `const db = require('./db'); db.connection` instead of destructuring — reaching through the object each time dodges the stale-copy problem, since property lookup happens when you access it. The moment you destructure, you've taken a snapshot.

**Tree-shaking** means a bundler dropping exports nobody imports. It works for ESM because the whole import graph is known just by parsing, without running anything. `require()` can be called with a computed string inside a branch, so a bundler can't prove what's unused.

## Module caching

A module's body runs **once per process**. Every later `require`/`import` of the same file gets the cached exports back. That's why this acts like a singleton:

```js
// db.js — connect() runs once, no matter how many files import this
export const db = connect();
```

## Interop

- `import x from 'some-cjs-package'` works — you get `module.exports` as the default export.
- `require()` of an ESM file used to throw outright, which is why older Stack Overflow answers call it impossible. Node 22+ allows it for ESM that doesn't use top-level await.

Mixing the two in one project is the source of most `ERR_REQUIRE_ESM` and "Cannot use import statement outside a module" errors.

## Modules in the browser

```html
<script type="module" src="./index.js"></script>
```

- Deferred by default (runs after the HTML is parsed).
- Always strict mode, with their own top-level scope — no accidental globals.
- Fetched over HTTP with CORS rules, so opening the page as `file://` won't work; you need a local server.

## Historical context

Before ESM existed, the community invented its own formats: **AMD** (browser, async, `define(...)`), **UMD** (a wrapper that works in both AMD and CommonJS), and CommonJS on the server. Bundlers like webpack, Rollup, and Vite exist partly to paper over this split — they take whatever module format you wrote and output something the target environment can run.
