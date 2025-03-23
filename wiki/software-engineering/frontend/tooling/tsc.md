### tsconfig overview

```json
{
	"compilerOptions": {
		/* Visit https://aka.ms/tsconfig to read more about this file */

		/* Projects */

		"incremental": true /* Save .tsbuildinfo files to allow for incremental compilation of projects. */,
		"composite": true /* Enable constraints that allow a TypeScript project to be used with project references. */,
		"tsBuildInfoFile": "./.tsbuildinfo" /* Specify the path to .tsbuildinfo incremental compilation file. */,
		"disableSourceOfProjectReferenceRedirect": true /* Disable preferring source files instead of declaration files when referencing composite projects. */,
		"disableSolutionSearching": true /* Opt a project out of multi-project reference checking when editing. */,
		"disableReferencedProjectLoad": true /* Reduce the number of projects loaded automatically by TypeScript. */,

		/* Language and Environment */

		"target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
		"lib": [] /* Specify a set of bundled library declaration files that describe the target runtime environment. */,
		"jsx": "preserve" /* Specify what JSX code is generated. */,
		"experimentalDecorators": true /* Enable experimental support for legacy experimental decorators. */,
		"emitDecoratorMetadata": true /* Emit design-type metadata for decorated declarations in source files. */,
		"jsxFactory": "" /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h'. */,
		"jsxFragmentFactory": "" /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */,
		"jsxImportSource": "" /* Specify module specifier used to import the JSX factory functions when using 'jsx: react-jsx*'. */,
		"reactNamespace": "" /* Specify the object invoked for 'createElement'. This only applies when targeting 'react' JSX emit. */,
		"noLib": true /* Disable including any library files, including the default lib.d.ts. */,
		"useDefineForClassFields": true /* Emit ECMAScript-standard-compliant class fields. */,
		"moduleDetection": "auto" /* Control what method is used to detect module-format JS files. */,

		/* Modules */

		"module": "commonjs" /* Specify what module code is generated. */,
		"rootDir": "./" /* Specify the root folder within your source files. */,
		"moduleResolution": "node10" /* Specify how TypeScript looks up a file from a given module specifier. */,
		"baseUrl": "./" /* Specify the base directory to resolve non-relative module names. */,
		"paths": {} /* Specify a set of entries that re-map imports to additional lookup locations. */,
		"rootDirs": [] /* Allow multiple folders to be treated as one when resolving modules. */,
		"typeRoots": [] /* Specify multiple folders that act like './node_modules/@types'. */,
		"types": [] /* Specify type package names to be included without being referenced in a source file. */,
		"allowUmdGlobalAccess": true /* Allow accessing UMD globals from modules. */,
		"moduleSuffixes": [] /* List of file name suffixes to search when resolving a module. */,
		"allowImportingTsExtensions": true /* Allow imports to include TypeScript file extensions. Requires '--moduleResolution bundler' and either '--noEmit' or '--emitDeclarationOnly' to be set. */,
		"rewriteRelativeImportExtensions": true /* Rewrite '.ts', '.tsx', '.mts', and '.cts' file extensions in relative import paths to their JavaScript equivalent in output files. */,
		"resolvePackageJsonExports": true /* Use the package.json 'exports' field when resolving package imports. */,
		"resolvePackageJsonImports": true /* Use the package.json 'imports' field when resolving imports. */,
		"customConditions": [] /* Conditions to set in addition to the resolver-specific defaults when resolving imports. */,
		"noUncheckedSideEffectImports": true /* Check side effect imports. */,
		"resolveJsonModule": true /* Enable importing .json files. */,
		"allowArbitraryExtensions": true /* Enable importing files with any extension, provided a declaration file is present. */,
		"noResolve": true /* Disallow 'import's, 'require's or '<reference>'s from expanding the number of files TypeScript should add to a project. */,

		/* JavaScript Support */

		"allowJs": true /* Allow JavaScript files to be a part of your program. Use the 'checkJS' option to get errors from these files. */,
		"checkJs": true /* Enable error reporting in type-checked JavaScript files. */,
		"maxNodeModuleJsDepth": 1 /* Specify the maximum folder depth used for checking JavaScript files from 'node_modules'. Only applicable with 'allowJs'. */,

		/* Emit */

		"declaration": true /* Generate .d.ts files from TypeScript and JavaScript files in your project. */,
		"declarationMap": true /* Create sourcemaps for d.ts files. */,
		"emitDeclarationOnly": true /* Only output d.ts files and not JavaScript files. */,
		"sourceMap": true /* Create source map files for emitted JavaScript files. */,
		"inlineSourceMap": true /* Include sourcemap files inside the emitted JavaScript. */,
		"noEmit": true /* Disable emitting files from a compilation. */,
		"outFile": "./" /* Specify a file that bundles all outputs into one JavaScript file. If 'declaration' is true, also designates a file that bundles all .d.ts output. */,
		"outDir": "./" /* Specify an output folder for all emitted files. */,
		"removeComments": true /* Disable emitting comments. */,
		"importHelpers": true /* Allow importing helper functions from tslib once per project, instead of including them per-file. */,
		"downlevelIteration": true /* Emit more compliant, but verbose and less performant JavaScript for iteration. */,
		"sourceRoot": "" /* Specify the root path for debuggers to find the reference source code. */,
		"mapRoot": "" /* Specify the location where debugger should locate map files instead of generated locations. */,
		"inlineSources": true /* Include source code in the sourcemaps inside the emitted JavaScript. */,
		"emitBOM": true /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */,
		"newLine": "crlf" /* Set the newline character for emitting files. */,
		"stripInternal": true /* Disable emitting declarations that have '@internal' in their JSDoc comments. */,
		"noEmitHelpers": true /* Disable generating custom helper functions like '__extends' in compiled output. */,
		"noEmitOnError": true /* Disable emitting files if any type checking errors are reported. */,
		"preserveConstEnums": true /* Disable erasing 'const enum' declarations in generated code. */,
		"declarationDir": "./" /* Specify the output directory for generated declaration files. */,

		/* Interop Constraints */

		"isolatedModules": true /* Ensure that each file can be safely transpiled without relying on other imports. */,
		"verbatimModuleSyntax": true /* Do not transform or elide any imports or exports not marked as type-only, ensuring they are written in the output file's format based on the 'module' setting. */,
		"isolatedDeclarations": true /* Require sufficient annotation on exports so other tools can trivially generate declaration files. */,
		"allowSyntheticDefaultImports": true /* Allow 'import x from y' when a module doesn't have a default export. */,
		"esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
		"preserveSymlinks": true /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */,
		"forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,

		/* Type Checking */

		"strict": true /* Enable all strict type-checking options. */,
		"noImplicitAny": true /* Enable error reporting for expressions and declarations with an implied 'any' type. */,
		"strictNullChecks": true /* When type checking, take into account 'null' and 'undefined'. */,
		"strictFunctionTypes": true /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */,
		"strictBindCallApply": true /* Check that the arguments for 'bind', 'call', and 'apply' methods match the original function. */,
		"strictPropertyInitialization": true /* Check for class properties that are declared but not set in the constructor. */,
		"strictBuiltinIteratorReturn": true /* Built-in iterators are instantiated with a 'TReturn' type of 'undefined' instead of 'any'. */,
		"noImplicitThis": true /* Enable error reporting when 'this' is given the type 'any'. */,
		"useUnknownInCatchVariables": true /* Default catch clause variables as 'unknown' instead of 'any'. */,
		"alwaysStrict": true /* Ensure 'use strict' is always emitted. */,
		"noUnusedLocals": true /* Enable error reporting when local variables aren't read. */,
		"noUnusedParameters": true /* Raise an error when a function parameter isn't read. */,
		"exactOptionalPropertyTypes": true /* Interpret optional property types as written, rather than adding 'undefined'. */,
		"noImplicitReturns": true /* Enable error reporting for codepaths that do not explicitly return in a function. */,
		"noFallthroughCasesInSwitch": true /* Enable error reporting for fallthrough cases in switch statements. */,
		"noUncheckedIndexedAccess": true /* Add 'undefined' to a type when accessed using an index. */,
		"noImplicitOverride": true /* Ensure overriding members in derived classes are marked with an override modifier. */,
		"noPropertyAccessFromIndexSignature": true /* Enforces using indexed accessors for keys declared using an indexed type. */,
		"allowUnusedLabels": true /* Disable error reporting for unused labels. */,
		"allowUnreachableCode": true /* Disable error reporting for unreachable code. */,

		/* Completeness */

		"skipDefaultLibCheck": true /* Skip type checking .d.ts files that are included with TypeScript. */,
		"skipLibCheck": true /* Skip type checking all .d.ts files. */
	}
}
```

#### examples

##### baseUrl

Sets the root directory for non-relative imports.

```
// With baseUrl: "./src"

// Project Structure
src/
  ├── components/
  │   └── Button.tsx
  ├── utils/
  │   └── format.ts
  └── App.tsx

// Import styles:
// Without baseUrl
import { Button } from '../../components/Button';
import { format } from '../utils/format';

// With baseUrl
import { Button } from 'components/Button';
import { format } from 'utils/format';
```

Makes imports:

-   Shorter
-   More maintainable
-   Independent of file location

##### disableSourceOfProjectReferenceRedirect

-   Default: TypeScript checks the original source files
-   With disable: TypeScript uses the compiled .d.ts files

```
// Without this option (default behavior)
packages/shared/
├── src/
│   └── button.tsx    // TypeScript looks at source file
└── dist/
    └── button.d.ts   // Ignores declaration file

// With disableSourceOfProjectReferenceRedirect: true
packages/shared/
├── src/
│   └── button.tsx    // Ignored
└── dist/
    └── button.d.ts   // TypeScript uses this instead
```

Benefits:

-   Faster type checking
-   Better encapsulation
-   Cleaner project boundaries Useful in monorepos where you want to:
-   Speed up type checking
-   Treat packages as "black boxes"
-   Rely on the public API (.d.ts) rather than implementation

##### disableSolutionSearching

```
// Without this option
root/
├── packages/
│   ├── ui/        ← TypeScript searches here
│   └── utils/     ← and here
└── apps/
    └── host/      ← When editing here

// With disableSolutionSearching: true
root/
├── packages/
│ ├── ui/ ✗ Not searched
│ └── utils/ ✗ Not searched
└── apps/
└── host/ ← Only looks here
```

Purpose: Speeds up editing by not checking other projects when you make changes.

##### disableReferencedProjectLoad

```
// Without this option
root/
├── packages/
│   ├── ui/        ← Loaded automatically
│   └── utils/     ← Loaded automatically
└── apps/
    └── host/      ← When opening this

// With disableReferencedProjectLoad: true
root/
├── packages/
│   ├── ui/        ← Loaded only when needed
│   └── utils/     ← Loaded only when needed
└── apps/
    └── host/      ← Working here
```

Purpose: Reduces memory usage by loading referenced projects only when actually needed. Both options help with performance in large monorepos by:

-   Reducing unnecessary type checking
-   Minimizing memory usage
-   Speeding up the editor

##### jsx

Controls how TypeScript handles JSX syntax in .tsx files:

```
// Your TSX code
const App = () => <div>Hello</div>;

// "jsx": "preserve" (keeps JSX as-is)
const App = () => <div>Hello</div>;

// "jsx": "react" (transforms to React.createElement)
const App = () => React.createElement("div", null, "Hello");

// "jsx": "react-jsx" (transforms to _jsx, modern React)
import { jsx as _jsx } from "react/jsx-runtime";
const App = () => _jsx("div", { children: "Hello" });
```

##### jsxFactory

Specifies what function to use for creating JSX elements.

```
// Default React (no jsxFactory needed)
const element = <div>Hello</div>;
// Transforms to: React.createElement("div", null, "Hello")

// With jsxFactory: "h" (for frameworks like Preact)
{
  "jsxFactory": "h"
}
const element = <div>Hello</div>;
// Transforms to: h("div", null, "Hello")

// With custom factory
{
  "jsxFactory": "createElement"
}
const element = <div>Hello</div>;
// Transforms to: createElement("div", null, "Hello")
```

For modern React: Don't set jsxFactory.

##### jsxFragmentFactory

Specifies how to handle JSX fragments (<>). For modern react, don't set it.

##### jsxImportSource

Tells TypeScript where to import JSX runtime functions from. Default from React. Set it when using:

-   CSS-in-JS libraries
-   Alternative JSX runtimes
-   Custom JSX implementations

##### lib

Specifies which built-in APIs are available in your code:

```
{
  "compilerOptions": {
    "lib": [
      "DOM",        // Browser APIs (window, document, etc.)
      "DOM.Iterable", // For iterating over DOM collections
      "ES2024"      // Modern JavaScript features
    ]
  }
}
```

Examples:

```
// DOM lib
document.querySelector('.button');  // Browser API

// ES2024 lib
const numbers = [1, 2, 3];
numbers.findLast(n => n > 1);      // Modern JS method

// Without proper lib, these would show errors
```

Common combinations:

-   Browser apps: ["DOM", "ES2024"]
-   Node.js apps: ["ES2024"]
-   React apps: ["DOM", "DOM.Iterable", "ES2024"]

The lib should match your target and runtime environment.

##### module

Determines how TypeScript converts your import/export statements:

```
// Your code
import { Button } from './Button';
export const App = () => <Button />;

// Output with "module": "commonjs"
const { Button } = require('./Button');
exports.App = () => <Button />;

// Output with "module": "ESNext"
import { Button } from './Button';
export const App = () => <Button />;
```

Common choices:

-   "commonjs": For Node.js (uses require/exports)
-   "ESNext": For modern browsers/bundlers (uses import/export)

##### moduleDetection

Controls how TypeScript decides if a file is a module or a script:

```
// With "moduleDetection": "auto" (default)

// This is a module (has import/export)
import React from 'react';
export const App = () => <div>Hello</div>;

// This is a script (no import/export)
const greeting = "Hello";
console.log(greeting);

// This becomes a module (has 'export {}'')
const greeting = "Hello";
export {};
```

Best practice:

-   Use "auto" (default) for most projects
-   Use "force" if you want all files to be modules

##### moduleResolution

Determines how TypeScript finds imported files:

```
// Your import
import { Button } from '@/components/Button';

// How TypeScript looks for files:

// "moduleResolution": "node" (Classic Node.js)
- check: @/components/Button.ts
- check: @/components/Button/index.ts
- check: node_modules/@/components/Button

// "moduleResolution": "bundler" (For Vite/webpack)
- uses package.json "exports"
- better for modern bundlers
- supports subpath imports

// "moduleResolution": "node16/nodenext"
- ESM-aware resolution
- package.json "exports"
- extension-sensitive
```

##### noLib

Disables including TypeScript's built-in type definitions. Best practice:

-   Don't use noLib in normal projects
-   Only used in very specialized cases
-   Use lib array to control which APIs are available

##### paths

Allows you to create import aliases:

```
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@/*": ["./*"],                 // @/components -> src/components
      "@components/*": ["components/*"], // @components/Button -> src/components/Button
      "@utils": ["utils/index.ts"]      // @utils -> src/utils/index.ts
    }
  }
}

// Usage in code:
// Instead of:
import { Button } from '../../components/Button';
import { format } from '../../../utils/format';

// You can write:
import { Button } from '@/components/Button';
import { format } from '@utils/format';
```

##### rootDir

Specifies where TypeScript should look for source files. It helps maintain your project structure in the output:

```
// Project Structure
src/              // rootDir: "./src"
  ├── components/
  │   └── Button.tsx
  └── App.tsx

// Output Structure (maintains paths relative to rootDir)
dist/
  ├── components/
  │   └── Button.js
  └── App.js
```

Best practice:

-   Set rootDir to your source directory
-   Pair with outDir for clean output

##### target

Purpose: JavsScript language features Common target values:

-   "es2016": Good browser compatibility
-   "es2020": Modern features, better performance
-   "es2024": Latest features, needs modern browsers Choose based on:
-   Browser support needs
-   Node.js version
-   Performance requirements For Vite + React apps, you can usually use a modern target like "es2020" or newer because:
-   Vite handles browser compatibility
-   Modern browsers are common
-   Better performance
