#micro-frontends #vite #module-federation #frontend-architecture #frontend-tooling #packagejson

### Configure a react-ts npm library

**Requirements**

-   Library is packaged correctly for both CommonJS and ES module environments

##### vite.config.ts

-   **global** is only necessary for UMD builds, because it can run directly in browsers via script tags
-   **preserveModules:** true is like telling your build tool: "Don't squash all my files into one big file, keep them separate." = Better for libs
    -   Treeshaking is better = When someone imports just your Button, they don't get your Modal code
    -   Better code splitting = When used in modern applications, allows the app to load only what it needs
    -   Bundle size seems to be smaller
-   **external:** can include all dependencies and peerDependencies. It tells vite, what to not include in the bundle and what will be provided at runtime. For libs, this is what we want, because the consuming system will provide it during runtime
-   **plugin dts**:
    -   Use `rollupTypes: true` to to merge all declarations into one file. This is helpful when having custom declaration files and you need to export them

```JSON
import { defineConfig } from 'vite';
import dts from 'vite-plugin-dts';
import { dependencies, peerDependencies } from './package.json';
import { copyFileSync } from 'fs';
import { resolve } from 'path';

export default defineConfig({
	build: {
		outDir: 'dist',
		emptyOutDir: true,
		lib: {
			name: 'lib-name',
			entry: {
				index: resolve(__dirname, 'src/index.ts'),
				module1: resolve(__dirname, 'src/lib1/index.ts'),
				module2: resolve(__dirname, 'src/lib2/index.ts'),
			},
			formats: ['es', 'cjs'],
			fileName: (format, entryName) => `${entryName}${format === 'es' ? '.js' : '.cjs'}`,
		},
		rollupOptions: {
			external: [
				...Object.keys(peerDependencies || {}),
				dependencies['@reduxjs/toolkit'],
				dependencies['react-redux'],
				// Subfolder imports
				'react/jsx-runtime',
			],
			output: {
				preserveModules: true,
				preserveModulesRoot: 'src',
			},
		},
	},
	plugins: [
		dts({ rollupTypes: true }),
	],
	resolve: {
		alias: {
			'@': resolve(__dirname, './src'),
		},
	},
});
```

##### package.json

```json
{
	"name": "<lib-name>",
	"version": "1.0.0",
	"type": "module",
	"main": "./dist/index.cjs.js",
	"module": "./dist/index.es.js",
	"types": "./dist/types/index.d.ts",
	"exports": {
		".": {
			"types": "./dist/types/index.d.ts",
			"import": "./dist/index.es.js",
			"require": "./dist/index.cjs.js"
		},
		"./lib1": {
			"types": "./dist/types/lib1/index.d.ts",
			"import": "./dist/lib1.es.js",
			"require": "./dist/lib1.cjs.js"
		},
		"./lib2": {
			"types": "./dist/types/lib2/index.d.ts",
			"import": "./dist/lib2.es.js",
			"require": "./dist/lib2.cjs.js"
		}
	},
	"files": ["dist"],
	"publishConfig": {
		"registry": "http://localhost:4873"
	},
	"peerDependencies": {
		"react": "^18.2.0",
		"react-dom": "^18.2.0"
	}
}
```

-   **`name`**: Name of npm package.
-   **`version`**: Package version.
-   **`type`**: Module type. `module` = ES modules.
-   **`main`**: Entry point for commonJS environments. Used by node.js.
-   **`module`**: Entry point for es module environments. Used by bundlers like Webpack or Rollup to take advantage of tree-shaking.
-   **`types`**: Providing type information for TypeScript consumers of your library.
-   **`exports`**: Entry points for your package. Specify different entry points for different environments (e.g., ES modules vs. CommonJS) and define sub-path exports for different parts of your library.
    -   **`.`**: Main entry point of the library.
    -   **`"./lib1"` and `"./lib2"`**: Sub-path exports for different modules within the library. Each sub-path specifies its own `types`, `import`, and `require` paths.
-   **`files`**: Specifies which files should be included when the package is published. In this case, the `dist` directory.
    -   When publishing, it automatically adds the package.json and README to the artifacts
-   **`publishConfig`**: Configuration for publishing the package. The `registry` field specifies the registry to which the package should be published. Here, it's set to a local Verdaccio instance.
-   **`peerDependencies`**: Lists packages that your library depends on, but which are expected to be provided by the consumer of your library. This is common for libraries that rely on specific versions of React, ensuring that the consumer uses a compatible version.

### tsconfigs

Vite creates 3 tsconfig files, because vite is using two different environments in which type-script code is executed:

1. App (`src` folder) is targeting (will be running) inside the browser
2. Vite itself is running on the host computer inside Node, which requires a different tsconfig

Following tsconfigs are created:

1. **`tsconfig.json`**: Shared config.
2. **`tsconfig.app.json`**: Config for app (`src` folder), targeting the browser.
3. **`tsconfig.node.json`**: Config for vite node environment.

### Vite and module federation

#### @originjs/vite-plugin-federation

Currently only working module federation plugin for vite, but not working properly in dev mode:

-   https://github.com/originjs/vite-plugin-federation/issues/204
-   https://github.com/vitejs/vite/issues/11804

#### How it works

##### dev environment

-   Module Federation creates virtual modules, when running remote apps with vite dev
    -   For providing a development-time version of React, so remote apps can run standalone
-   Simulate how the shared dependencies would work in production

No need to run host app during development.

##### production environment

-   The host application bundles React and exposes it through Module Federation's shared dependency system (if it's shared)
-   Virtual modules won't be used
-   The build output of remotes also include js files for the shared react libraries, but are only used as a fallback
-   The actual remote bundle will be smaller on runtime, because it will load the shared lib from the host
-   The manifest file is basically the contract for shared dependencies

##### Builds outputs

When no deps are shared, the build output looks like this:

```
|-dist
|---|.vite
|------manifest.json
|---|src
|------federation_expose_App_hash.js
|------index_hash.js
|------remoteEntry.js
```

-   **`federation_expose_App_hash.js`** comes bundled with react. Size of ~ 37KB.

When deps are shared, the build output looks like this:

```
|-dist
|---|.vite
|------manifest.json
|---|src
|------federation_expose_App_hash.js
|------index_hash.js
|------index2_hash.js
|------index3_hash.js
|------commonjsHelpers-hash.js
|------federation_shared_react-hash.js
|------federation_shared_react-dom-F6oo-G3a.js
|------remoteEntry.js
```

-   **`federation_expose_App_hash.js`** comes bundled without react. Size of ~ 16KB.

##### How files are loaded

4. **`host-index.js`**: Multiple index files of the host are loaded for intialising host application
5. **`federation_shared_react.js`**: Previous loaded index file loads the shared react lib
6. **`remoteEntry.js`**: Host loads remoteEntry.js
7. **`federation_expose_App_hash.js`**: Exposed remote component is loaded
8. **`federation_fn_import.js`**: Import fn for asynchronous loading of remote modules like shared react lib and providing fallback mechanism.

#### Bundle sizes

It doesn't matter if react is a **`dependency`** or **`devDependency`**, it is required for the build. Either react is bundled or a shared federate js file is created. Here is a detailed build output with different combinations:

##### devDeps with shared federation

###### host

../commonjsHelpers-CqkleIqs.js 0.12 kB │ gzip: 0.13 kB ../federation_shared_react-BqRz74IV.js 0.14 kB │ gzip: 0.14 kB ../federation_shared_react-dom-F6oo-G3a.js 0.14 kB │ gzip: 0.14 kB ../index--jJ4wUNT.js 9.58 kB │ gzip: 3.08 kB ../index-DyMXFzA9.js 23.18 kB │ gzip: 8.46 kB ../index-sPThxm96.js 340.32 kB │ gzip: 103.43 kB

###### remote

../commonjsHelpers-CqkleIqs.js 0.12 kB │ gzip: 0.13 kB ../federation_shared_react-BqRz74IV.js 0.14 kB │ gzip: 0.14 kB ../federation_shared_react-dom-F6oo-G3a.js 0.14 kB │ gzip: 0.14 kB ../remoteEntry.js 1.55 kB │ gzip: 0.86 kB ../federation_fn_import-Du49MKBN.js 4.96 kB │ gzip: 2.01 kB ../index--jJ4wUNT.js 9.58 kB │ gzip: 3.08 kB ../federation_expose_App-CmgEoGVY.js 12.38 kB │ gzip: 4.57 kB ../index-DyMXFzA9.js 23.18 kB │ gzip: 8.46 kB ../index-FEfkY2kd.js 323.00 kB │ gzip: 97.70 kB

##### deps with shared federation

###### host

../commonjsHelpers-CqkleIqs.js 0.12 kB │ gzip: 0.13 kB ../federation_shared_react-BqRz74IV.js 0.14 kB │ gzip: 0.14 kB ../federation_shared_react-dom-F6oo-G3a.js 0.14 kB │ gzip: 0.14 kB ../index--jJ4wUNT.js 9.58 kB │ gzip: 3.08 kB ../index-DyMXFzA9.js 23.18 kB │ gzip: 8.46 kB ../index-sPThxm96.js 340.32 kB │ gzip: 103.43 kB

###### remote

../commonjsHelpers-CqkleIqs.js 0.12 kB │ gzip: 0.13 kB ../federation_shared_react-BqRz74IV.js 0.14 kB │ gzip: 0.14 kB ../federation_shared_react-dom-F6oo-G3a.js 0.14 kB │ gzip: 0.14 kB ../remoteEntry.js 1.55 kB │ gzip: 0.86 kB ../federation_fn_import-Du49MKBN.js 4.96 kB │ gzip: 2.01 kB ../ndex--jJ4wUNT.js 9.58 kB │ gzip: 3.08 kB ../federation_expose_App-CmgEoGVY.js **12.38 kB │ gzip: 4.57 kB** ../index-DyMXFzA9.js 23.18 kB │ gzip: 8.46 kB ../index-FEfkY2kd.js 323.00 kB │ gzip: 97.70 kB

##### devDeps without shared federation

###### host

../index-G2fS_P_Y.js **368.20 kB │ gzip: 110.11 kB**

###### remote

../remoteEntry.js 1.55 kB │ gzip: 0.86 kB ../federation_expose_App-C3bS-JaY.js **35.62 kB │ gzip: 11.21 kB** ../index-DLrrm4jr.js 332.52 kB │ gzip: 100.11 kB

##### deps without shared federartion

###### host

../index-G2fS_P_Y.js 368.20 kB │ gzip: 110.11 kB

###### remote

../remoteEntry.js 1.55 kB │ gzip: 0.86 kB ../federation_expose_App-C3bS-JaY.js 35.62 kB │ gzip: 11.21 kB ../index-DLrrm4jr.js 332.52 kB │ gzip: 100.11 kB
