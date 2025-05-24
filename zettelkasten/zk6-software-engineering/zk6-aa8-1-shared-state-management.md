**`id`**: zk6-aa8-1
**`title`**: shared-state-management
**`date`**: 2025-05-21
**`tags`**: #software-engineering #software-architecture #software-design #programming-languages

---

###### Content

-   Favor pure functions for self-contained code when efficiency is not a priority.
-   Parameterize data to flow through function call graphs, minimizing shared state issues.
-   If necessary structure global state into structs, records, or classes for better organization.
-   Write class methods only when there is a clear relationship to one data type (often not the case).

==A pure functions is the only true self-contained unit of code.==
