**`id`**: zk1-aa2-34
**`title`**: rules-for-dependency-management
**`date`**: 2025-02-17
**`tags`**: #micro-frontends #frontend-architecture #module-federation #dependencies #peerDependencies #devDependencies

---

###### Content

Peer Dependencies:

-   Limit to major libraries (e.g., React)
-   Each addition increases consumer complexity
-   It doesnt't reduce bundle size

Cross Dependencies:

-   Should remain stable (infrequent updates)
-   Less cross-references = better maintainability

==Move code to a new package when either==

-   It's highly independent with minimal cross-references
-   OR has stable cross-references
