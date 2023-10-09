# linting

* a NBA to testing
* linters look at a programme's source code to find problems; this process is automatic
* a linter is a common feature of pull request automation because it ensure that *obvious* bugs do not make it to prod

#### example
* consider this code snippet

    ```javascript
        var x = "5";
        function f(elems){console.log(elems)
        let x = 0;
        while(x<100){
            console.log(x)}
        }
    ```

* the code stores the string `5` in var `x` and defines a function, `f`, that accepts `elems` as a param. said function logs the value of tha param `elems` and `x`. `x` has been initialised at zero and is logged as long as it is less than 100
* a number of things are *wrong* with the code:
    * two variables have been given tha same name `x`: the string `5` and the int initialised at `0`
    * infinite loop: `x` is not adjusted inside the `while` loop; it will never get to 101, therefore, will loop forever
    * there is code on the same line as the opening brace, `{`, of the function `f`

        ```javascript
            //snip
            function f(elems){console.log(elems)
            //snip
        ```
    
    * inconsistent statement termination: the `console.log`s do not have a terminating semi-colon, alternatively, the variable assignment statements hace terminating semi-colons; pick one and stick to it
    * the `while` loop's closing brace is not on its own line
    * inconsistent indetation: the assignment to `x`, the `while` loop and its body are off-indent
* what to do
    * linting rules
        * do not shadow variables; each variable name must be unique whether or not said variables are in different scopes -> takes care of the first problem
        * have at least one statement within the loop that changes the value of the comparison variable, `x` -> takes care of second problem
        * use style guide to format code. if none use common effing sense -> takes care of other problems
    * decide on the *modus operandi* (the style guide) beforehand
        * tabs or spaces
        * camelCase, snake_case, MACRO_CASE, UPPERCASE, lowercase [etc][def]

#### the *nit* approach
* *nits* are little comments on a dev's code place there by a reviewer. said dev can ignore said comments until the stage where broader reviews are conducted. nits are helpful as future references and, at the same time, prevent blocking important changes

#### auto-formatter
* a tool that allows a dev to apply code style rules automatically based on a given guide
* example: Go has `gofmt`:

    ```bash
        find . -name '.go' -not -path "/vendor/*" -exec gofmt -s -w {};
    ```

* `find`, in the current directory, all the files that have a `.go` extension; apply (`exec`) the `gofmt` command, with flags `-s` and `-w` on said files. do not include those that contain `/vendor/` in in their path names

#### linting *faux pas*
* having a human reviewer let you know that your code is linted and styled correctly
* not having linting and formatting w/i CI pipeline
    * easy solution: no merging if linting fails
        * example, docker file that runs lint as *modus operandi*:

            ```docker
                COPY . .
                RUN npm run lint
            ```

    * better solution: fix issues automatically:
        * example, same docker file

            ```docker
                COPY . .
                RUN if ! npm run eslint; then \
                    npm run eslint --fix && \
                    git config user.name 'lint bot' && \
                    git config user.email 'lintbot@example.com && \
                    git add -A && \
                    git commit -m 'lint code automatically' && \
                    git checkout -b $(git branch --show-current)-linted && \
                    git push -uf origin $(git branch --show-current)-linted  && \
                    echo 'lint failed!; exit 1; \
                fi
            ```
   
* not having a linter, static analysis tools etc when team size > 1 dev

#### linters for various languages

|language|ESLint|TyeScript ESLint|pylint|flake8|cpplint (GOOG)|gofmt|CheckStyle|FindBugs|RuboCop|Pronto|SonarQube|DeepSource|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|JS|:heavy_check_mark:||||||||||:heavy_check_mark:|:heavy_check_mark:|
|TS||:heavy_check_mark:||||||||||:heavy_check_mark:||
|Py|||:heavy_check_mark:|:heavy_check_mark:|||||||:heavy_check_mark:|:heavy_check_mark:|
|C++|||||:heavy_check_mark:||||||||
|Go||||||:heavy_check_mark:|||||:heavy_check_mark:|:heavy_check_mark:|
|Java|||||||:heavy_check_mark:|:heavy_check_mark:|||:heavy_check_mark:|:heavy_check_mark:|
|Ruby|||||||||:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|C#|||||||||||:heavy_check_mark:||
|Kotlin|||||||||||:heavy_check_mark:||
|Scala|||||||||||:heavy_check_mark:||
|Flex|||||||||||:heavy_check_mark:||
|PHP|||||||||||:heavy_check_mark:||
|HTML|||||||||||:heavy_check_mark:||
|CSS|||||||||||:heavy_check_mark:||
|XML|||||||||||:heavy_check_mark:||
|VB.NET|||||||||||:heavy_check_mark:||
|Docker||||||||||||:heavy_check_mark:|
|Terraform||||||||||||:heavy_check_mark:|
|SQL||||||||||||:heavy_check_mark:|
|Shell (not open-source)||||||||||||:heavy_check_mark:|

[def]: https://en.wikipedia.org/wiki/Naming_convention_(programming)