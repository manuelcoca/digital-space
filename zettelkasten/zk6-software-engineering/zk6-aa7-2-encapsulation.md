**`id`**: zk6-aa7-2 \*\*
**`title`**: object-oriented-programming
**`date`**: 2025-05-22
**`tags`**: #software-engineering #software-design #software-architecture #programming-languages

---

###### Content

-   Encapsulation = bundles state and methods that should not be modified by other parts
-   Promises = reduced coupling, enhanced maintainability, and simplified interactions with the object.
-   Is achieved by strict hierarchy of objects with a root object coordinating state
    -   Correct OOP = giant abstraction hierarchy = no simplicity and maintainability
    -   Wrong OOP = too many shared objects = no encapsulation anymore
    -   = makes only sense until a certain level of granity
-   Doesn't provide a fundamental solution to the shared state issue = as soon as objects are shared there is no encapsulation anymore
    -   Shared objects = 5 objects communicate and indirectly modify one object's state, is no difference than a global variable and 5 functions interacting with the state
-   SOLID principles, dependency injection, test-driven-development etc., compensate for encapsulation’s failures.
