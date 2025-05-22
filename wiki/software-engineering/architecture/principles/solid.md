### single-responsibility-principle (SRP)

-   **Responsibility !=** what a function is doing
-   **Responsibility =** only one reason to change
-   **Example of multiple responsibilities:** Employee class with 3 methods and if changed affects 3 different company departments
-   **Simple rule:** don't mix concerns in one class = don't put functions that change for different reasons into the same class

### open-closed-principle (OCP)

-   Class should be open for extension but closed for modification
-   **Open for extension** = being able to change what the class does
-   **Closed for modificaiton** = without changing the class
-   = A class behaviour or function can be extended by writing new code, not by modyfing existing class

### liskov-substitution-principle (LSP)

-   Derived classes must be useable through the base class interface, without the need for the user to know the difference
-   Typical violition of this principle is when the code needs to check of what type an object is (e.g instance of)

### interface-segregation-principle (ISP)

### dependency-inversion-principle (DIP)
