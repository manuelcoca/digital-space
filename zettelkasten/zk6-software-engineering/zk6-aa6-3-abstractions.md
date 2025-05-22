**`id`**: zk6-aa6-3
**`title`**: abstractions
**`date`**: 2025-05-21
**`tags`**: #softwareengineering #softwarearchitecture #programminglanguages #object-oriented-programming #functional-programming

---

###### Content

-   Don't implement abstractions unless the need is there. But the need is there when a change affects other parts of the code.
-   The ultimate goal is that a requirement change requires a change in only one module, in one test and it will not break other code. It's very hard to reach that.
-   The most difficult part of abstraction and test-driven development is to find a good design leading to least possible changes later. In reality the system and business requirements evolve and change over time.

==Unpredictable changes, even during development, will always come up.==
