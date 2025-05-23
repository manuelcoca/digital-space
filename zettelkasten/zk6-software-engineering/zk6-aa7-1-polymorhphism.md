**`id`**: zk6-aa7-1
**`title`**: polymorhphism
**`date`**: 2025-05-21
**`tags`**: #softwareengineering #softwaredesign #softwarearchitecture #programminglanguages

---

###### Content

-   Polymorphism = uniform treatment of types via interfaces.
-   All programs have a function call tree. Flow of control = top to bottom.
    -   Problem: Parent modules depend on child modules (via imports), creating tight coupling.
-   Tight coupling = fragile code which is hard to change.
-   Polymorphism = inverting the dependency / flow of control by defining an interface between two modules = loose coupling
-   Polymorphism = gives you control of the dependency structure
-   Good dependency management = more reusable, less fragile code.
-   Polymorphism is not excusively OOP and can also be achieved in non-oop, e.g:
    -   Go = uses interfaces, not inheritance required - if a method implements the interface it just works without explicitly telling a method implements the interface.
    -   Python = uses duck typing - if an object has the right method, it just works
