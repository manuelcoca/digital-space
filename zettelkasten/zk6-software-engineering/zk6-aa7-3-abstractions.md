**`id`**: zk6-aa7-3
**`title`**: abstractions
**`date`**: 2025-05-21
**`tags`**: #softwareengineering #softwaredesign #softwarearchitecture #programminglanguages

---

###### Content

-   Abstraction = simplifies complexity
-   Abstraction OPP is often non-intuitive
-   Not abstraction itself is the problem, but bad abstraction is.
-   Don't implement abstractions unless the need is there. The need is there when a change affects other parts of the code.
-   The ultimate goal is that a requirement change requires a change in only one module, in one test and it will not break other code. It's very hard to reach that.
-   The most difficult part of abstraction and test-driven development is to find a good design leading to least possible changes later. In reality the system and business requirements evolve and change over time.

==Unpredictable changes, even during development, will always come up.==

###### References

[[zk6-aa9-0-software-design-is-dependency-management]]
