# branch coverage

* a methodology that measures, quantitatively, how comprehensive a code-base's tests are
* difference between code and branch coverage is this: code coverage measures individual lines of code; branch coverage measures groups of lines of code
* said groups must be related eg. `if`, `for` or `while` block (loops and conditionals),
`enum`, `interface`, `function` or `class` block (data structures) etc
* higher coverage implies high stability and less bugs

#### example
* assume that the following function *actually* does something useful

    ```javascript
        let f = n => {
            let x = "hello", result = [];
            for(let i=0; i<n; ++i){
                result.push(x+i);
                if(i%50==49){
                    result.push("50th: "+x+1);
                }
            }
            return result;
        }
    ```

* there are three branches here:
    * the function declaration level (is the main one)
        * will always execute
    * the `for` block
        * only executes when 
            $$ x \lt n $$
    * the `if` block
        * only executes when 
        
            ```math
                {i \mod 50} \eq 49
            ```
* branch coverage is 100% if all branches evaluate to `true`, that is, are executed; adjust proportions baed on the *truthy*-ness of a branch

#### how to calculate branch coverage

>   $$ branch coverage = {total no. of non-syntax groups with tests \over total number of non-syntax groups} $$

* consider the following test (as usual, it does nothing useful)

    ```javascript
        it("should work for two", () => {
            let expected = ["hello0", "hello1"];
            assertEquals(f(2), expected);
        });
    ```

 * the unit test above checks whether the return of function `f()` with the arg `2` passed to it is the same as the array `expected` (test passes, by the way)
 * recall: the function `f()` is from the code snippet in the example section above
    * 2 < 50, therefore, the special message will never be pushed into the `result` array
    * the test does not cover the part where the special message is pushed to the array; it only tests:
        * the function block
        * the `for` block
        * the `if` block
    * said test passes two out of three groups or 66% of the non-syntax groups
    * branch coverage, therefore, is 66%

#### when to care about branch coverage
* you have inherited a codebase
* your product has users
* said users can/will leave if/when affected by bugs

#### branch coverage *faux pas*
* too many tests for uncertain features
    * users care about the stability  of the system, not the number of tests the system passes
* sunk cost fallacy
    * "users do not like feature X. i created Y tests for said feature; it passes. why TF would I drop the feature?"