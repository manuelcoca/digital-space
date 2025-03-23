#packagejson #npm #pnpm #dependencies #peerDependencies #devDependencies

### Version management

| Code status                               | Stage         | Rule                                                               | Example version |
| ----------------------------------------- | ------------- | ------------------------------------------------------------------ | --------------- |
| First release                             | New product   | Start with 1.0.0                                                   | 1.0.0           |
| Backward compatible bug fixes             | Patch release | Increment the third digit                                          | 1.0.1           |
| Backward compatible new features          | Minor release | Increment the middle digit and reset last digit to zero            | 1.1.0           |
| Changes that break backward compatibility | Major release | Increment the first digit and reset middle and last digits to zero | 2.0.0           |

https://docs.npmjs.com/about-semantic-versioning#incrementing-semantic-versions-in-published-packages

### Dependencies

-   **peerDependency:** Expressing compatibility of your package, exposing a specific interface, expected by the host. Reduces bundle size, but increasing peerDependency adds complexity
-   **devDependency:** Packages that are only needed for local development and testing.
-   **dependency:** Packages required the application in production to run.

#### peerDependency vs. dependency

By adding a package with **`peerDependencies`**:

-   If the **`peerDependency`** already exists in **node_modules**, nothing happens.
-   If the **`peerDependency`** doesn't exists in **node_modules** or the wrong version is installed, it will not be added. But npm shows a warning that the **`peerDependency`** wasn’t found. By adding a package with **`dependencies`**:
-   If the **`dependency`** doesn’t already exists in my **node_modules**, it will be installed automatically.
    -   See how [[pnpm]] handles dependencies

#### Version conflicts

Npm deals with version conflicts by adding duplicate private versions of the conflicted package.

##### Example

When using react as dependency, the consuming systems might have a different react version installed. By installing a npm lib, it installs the same react version defined in the libs package.json dependencies. This can lead to version conflicts, because npm is installing 2 different react versions.

With peerDependency, the developer of the consuming system can decide which version to install.
