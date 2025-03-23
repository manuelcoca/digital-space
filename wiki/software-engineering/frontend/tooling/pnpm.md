#packagejson #npm #pnpm #dependencies #peerDependencies #devDependencies

### pnpm vs. npm

-   **Saving disk space**: When using npm, if you have 100 projects using a dependency, you will have 100 copies of that dependency saved on disk. With pnpm, the dependency will be stored in a content-addressable store
-   **Faster installation**: Fast installations through hard linking from store to `node_modules`
-   **No version conflicts**: Prevents version conflicts via improved node_modules structure

### How pnpm manages dependencies in a monorepo

##### Centralized Storage:

-   Dependencies are stored in `root/node_modules/.pnpm`.
-   This setup allows sharing across different projects.

##### Monorepo Structure:

-   `root/node_modules`
    -   `.pnpm`
        -   **@tanstack/react-query** (stored here)
-   `app1/node_modules`
    -   `@custom/libs`
    -   **@tanstack/react-query** (symlink to root)
-   `app2/node_modules`
    -   `@custom/libs`

##### Example Scenario:

When installing **@tanstack/react-query** in `app1`, pnpm shows:

`Progress: resolved 373, reused 331, downloaded 0, added 0, done`

This means the library is already in `.pnpm`, and pnpm just creates a symlink in `app1/node_modules`. Traditional npm would install separate copies in each app's `node_modules`, causing :

-   `app1/node_modules`
    -   **@tanstack/react-query**
-   `app2/node_modules`
    -   **@tanstack/react-query**
