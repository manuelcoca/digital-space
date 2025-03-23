---
tags:
    - dotnet
---

Biggest mistakes of Object Mapping in .NET:

-   Having Business Logic in Object Mappings
-   Using AutoMapper for everything
    -   Automapper can be helpful if you need to map same object to another where it's pretty straight forward, but it shouldn't be used for manual mapping and business logic
-   Use constructors
    -   Example: Using constructor overload with different inputs in order to map to an domain object

Recommended for using manual mappings:

-   Create a manual mapper static class with static mapping functions
-   Mappings can be in folders in any layer of the application were it's needed
