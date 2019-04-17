## 👷🏻 DEVELOPMENT GUIDELINES

### General Guidelines

1. All the code is open source and published on GitHub.
1. Do not use classes unless absolutely necessary; keep data (structures) and behaviors (functions) separate
1. Whenever and wherever possible, prefer functional style over imperative and immutable data structure over mutable state.
1. Keep each individual change small: only change the minimum amount of code needed to accomplish the purpose of your pull request (i.e. do not change the format of the code if that's not the purpose of your change).
1. We're not for the 100% coverage religion but you should write tests for the core of your app or usually for the part that is more fragile to changes.
1. Use the OpenAPI standard to defining REST APIs exposed to clients. Follow [Zalando Guidelines](http://zalando.github.io/restful-api-guidelines/) when designing your APIs.

### TypeScript

* Always use [structured types](https://github.com/gcanti/io-ts): do not pass in input to unstructured data functions (eg `JSON` or `request.Express`)
* Use [tagged types](https://blog.mariusschulz.com/2016/11/03/typescript-2-0-tagged-union-types) and [algebraic types](https://stackoverflow.com/questions/33915459/algebraic-data-types-in-typescript) instead of classes
* Use [discriminated unions](http://www.typescriptlang.org/docs/handbook/advanced-types.html#discriminated-unions) instead of inheritance
* Favor immutable data structures and operators: use `const` instead of `let`, `map` and `filter` instead of `for` or `while` loops, [spread operators](https://davidwalsh.name/merge-objects) instead of direct assignments, etc.
* If you use classes do not provide setter methods: make sure that all members are readonly
* Favor the use of ["pure" functions](https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c)
* Use [Option](https://github.com/gcanti/fp-ts/blob/master/src/Option.ts) and avoids `null` / `undefined` checks
* To handle errors, return [Either](https://github.com/gcanti/fp-ts/blob/master/src/Either.ts) instead of throwing exceptions
* use `Promises` instead of callbacks for the asynchronous code. Limits the use of callbacks to interaction with existing libraries (ideally you wrap promisify callbaks though)
* Consider using `async` / `await` instead of `then` / `catch` if it can make the code more readable
* Use common code (types and functions) defined in [italia-ts-commons](https://github.com/teamdigitale/italia-ts-commons) (e.g. `NonEmptyString`, `DateFromString`, `EmailString`, etc.)
* Use [io-ts](https://github.com/gcanti/io-ts) to defined types that validate at compile and run-time.
* Use [fp-ts](https://github.com/gcanti/fp-ts) for functional data structures (i.e. `Option`, `Either`, `NonEmptyArray`, etc...)
* Use [italia-utils](https://github.com/teamdigitale/italia-utils) for generating `io-ts` models from OpenAPI specs.

### Editors and Code Formatting

You are free to use any code editor or IDE you like.

However, within some repositories, you will find configurations
for [Visual Studio Code](https://code.visualstudio.com/) (VSC).
VSC is an open source editor and has some features that are essential for the development of this project:

* excellent support for [Typescript](http://www.typescriptlang.org)
* effective integration with [tslint](https://palantir.github.io/tslint/)
* support for indentation _on-save_ via [prettier](https://github.com/prettier/prettier)

Before every PR, make sure that all the Typescript code (or Javascript)
is indented by [prettier](https://github.com/prettier/prettier).
The configuration is available in the `.prettierrc` files in each
project root directory.

If you use VSC, install the [prettier plugin](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) and enable _format on save_.

### Code Quality

To monitor the quality of the code we produce we use some tools
for automated code analysis that are well integrated into our development
workflow:

* [Codecov](https://codecov.io): test coverage
* [Codacy](https://www.codacy.com/): code quality and security
* [Codeclimate](https://codeclimate.com): code quality and maintainability
* [Snyk](https://snyk.io): security analysis of dependencies

Always check the feedback that these tools automatically provide to each
Pull Request and implement the changes necessary or suggested.


### Third-party libraries

IO includes several third-party open source libraries in its components. When including or updating a library it could happen that the library's code needs some changes (e.h. bugfixes). In that case two options can be evaluated:

1. fork the original github repository (the one where the library comes from), patch it, and then [set the dependency to the forked repo](https://docs.npmjs.com/files/package.json#github-urls) from `package.json`
2. create a [post install patch](https://www.npmjs.com/package/patch-package) to fix the original library code
