**`id`**: zk6-aa6-2 **`title`**: polymorhphism **`date`**: 2025-05-21 **`tags`**:

---

###### Content

-   All programs have a function call tree. Flow of control = top to bottom.
    -   Problem: Parent modules depend on child modules (via imports), creating tight coupling.
-   **Tight coupling** = fragile code which is hard to change.
-   Polymorphism = inverting the dependency / flow of control by defining an interface between two modules = **loose coupling**
-   Polymorphism = gives you control of the dependency structure
-   Good dependency management = more reusable, less fragile code.
-   OO = Polymorhphism.
-   OO is NOT modelling the real world (marketing non-sense)
-   Polymorphism can also be achieved in non-oop, e.g:
    -   Go = uses interfaces, not inheritance required - if a method implements the interface it just works without explicitly telling a method implements the interface.
    -   Python = uses duck typing - if an object has the right method, it just works
