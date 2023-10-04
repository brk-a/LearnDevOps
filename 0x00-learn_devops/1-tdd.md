# test-driven development (TDD)

* a coding methodology where tests are written before the code is

#### tests
* unit tests -> make sure individual parts of the app work
    * does the tank in a coffee-maker hold water?
* integration tests -> make sure that individual components (collection of parts that achieves an goal/performs an action) of the app work
    * does the grinder assembly in a coffee -maker work? how about the heating assembly?
* system (end-to-end) tests -> make sure that the sum-of-parts (the whole app) works
    * does the coffee-maker make a cup of coffee?
* acceptance tests -> see if test users find the app intuitive to use
    * can users find the `Espresso` and `Double Espresso` functionality intuitively?
    * does the coffee-maker break before the warranty period expires?

#### goals
* know when something breaks
* know where said something broke
* know that the whole system works correctly

#### workflow w/o TDD
* choose a task
* build on the basis of specifications
* test using small scripts

> idea is: assume a coffee-maker exists. Assume that it operates as required by specs. How do we tell that it does? Using that info, let us build the cheapest possbile coffee-maker that satisfies said specs
