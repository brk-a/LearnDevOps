# code coverage

* a methodology that measures, quantitatively, how comprehensive a code-base's tests are
* higher coverage implies high stability and less bugs
* 

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

* the function takes in a number, loops that number of times, appends the current number to the string `hello`, pushes the concatenated string to the array `result`, pushes a special message every 50th loop and returns said array
* three types of lines in lines of code: syntactic, logic and branch
    * syntactic lines are simply that: syntactic. they exists to fulfill the requirements  of the structure of the programming language (or to avoid syntax errors, take your pick)
        * example: the curly braces that close the `if` and `for` blocks and the function declaration statement
    * logic lines have side-effects; the behaviour of the code changes when they are modified
        * example: the `push` operation right before the `if` statement
    * branch lines change the flow of code; they change the order that commands are run and/or whether, at all, a block of code will be run
        * example the `push` operation inside the `if` statements and the code block inside the `for` loop

#### how to calculate code coverage

>   $$ code coverage = total no. of non-syntax lines with tests \over total number of non-syntax lines $$

* logic and branch lines are the non-syntactic ones, clearly
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
        * the assignment op
        * the first `push` op
        * the `return`
    * said test covers five out of six lines or 83% of the non-syntax lines
    * code coverage, therefore, is 83%
* 