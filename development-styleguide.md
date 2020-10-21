## 👨‍💻 DEVELOPMENT STYLE GUIDE

A practical reason to use a style guide is that it helps you write code.
Furthermore, it shows commons mistakes or scenarios and how to handle them

### Typescript

#### Avoid object modification.

- Due to its dynamic nature, JavaScript makes it extremely easy to modify objects that you do not own
- Such practice should generally be avoided as it can lead to maintenance problems and can produce hard to find bugs
- If you don't own it, don't modify it

```typescript
let bob = { name: "Bob", surname: "Smith", age: 23 };
bob.age = 24; // avoid
// create a new object using spread operator to update only specific fields
const olderBob = { ...bob, age: 24 };
```

#### Avoid hiding exceptions

Usually in Javascript and Typescript is hard to figure out whether a function throws an exception or not.
Try not to use exceptions at all, you can use eithers instead in 99% of cases. Exceptions must be limited to the scope of the main() method in case you need to early exit the process with an error message.
If you need to throw an exception, try to be explicit at least.

```typescript
//
// ✅good: no exception is thrown
//

const config = (key: string): Option<string> => (
  fromNullable(process.env[key])
);

const callService () => {
  sequenceS(option)({
    token: config("token"),
    url: config("url")
  }).map(({ toke, url }) => {
    // do stuff here ...
  })
};

//
// ✅good: exception is thrown explicitly at least
//

const config = (key: string): Option<string> => (
  fromNullable(process.env[key])
);

const callService () => {
  const token: string = config("token").getOrElseL(() => {
    throw new Error("Must provide a token!");
  });
  const url: string = config("url").getOrElseL(() => {
    throw new Error("Must provide a url!");
  });

  // do stuff here ...
};


//
// ❌bad: exception is hidden inside the "config" function
//

const config = (key: string): string => {
  const value = process.env[key];
  if(!value) throw new Error(`Must provide a value for key ${key}!`);
  return value;
};

const callService () => {
  const token: string = config("token");
  const url: string = config("url");

  // do stuff here ...
};

```

### [FP-TS](https://gcanti.github.io/fp-ts/modules/Option.ts.html)

#### Use [Options](https://github.com/gcanti/fp-ts/blob/1.12.0/src/Option.ts) to handle nullable values.

```typescript
import { fromNullable, Option, some } from "fp-ts/lib/Option";

// this is a common scenario where we want to model an user with email preferences

// undefined -> user doesn't give any feedback
// true -> users wants to receive email
// false -> users doesn't want to receive email
type EmailPreference = boolean | undefined;

type User = {
  id: number;
  emailPreference: EmailPreference;
};

const user: User = { id: 1, emailPreference: true };

// check if user wants to receive email
if (user.emailPreference !== undefined && user.emailPreference === true) {
  // send email
} else {
  console.log("can't send email due to the user's preference");
}

// use option
fromNullable(user.emailPreference).map(pref => {
  if (pref) {
    // send email
  } else {
    console.log("can't send email due to the user's preference");
  }
});
```

#### Use [Either](https://github.com/gcanti/fp-ts/blob/1.12.0/src/Either.ts) to handle success or failure.

For some scenario you should be interested why a computation failed
Using Option is usefull when you want know if a value is present or not.
The reason why it is not present it's not interesting
If you want to know more about computation failure you should use Either

Consider this example:
write a function to validate a string
it has to be greater than 8 chars

#### a possible implementation

```typescript
// throw an error if the value is not compliant
export const validateString = (value: string): string => {
  if (value.length < 8) {
    throw new Error("the string it too short");
  }
  return value;
};

const myString = "123456A";

try {
  const validatedString = validateString(myString);
  // do stuffs with the validated string
} catch (err) {
  //ops, something has gone wrong
  console.error(err);
}
```

#### using Either

```typescript
export const validateStringEither = (value: string): Either<Error, string> => {
  if (value.length < 8) {
    return left(Error("the string it too short"));
  }
  return right(value);
};

const validatedStringEither = validateStringEither(myString);

validatedStringEither.fold(
  err => {
    console.error(err);
  },
  value => {
    // do stuffs with the validated string
  }
);
```

if your validation function could raise multiple errors
you should use [Validation](https://dev.to/gcanti/getting-started-with-fp-ts-either-vs-validation-5eja) to collect them.

#### Avoid the use of imperative methods like `isLeft` and `isNone`.

When possible avoid to use `isNone` / `isLeft` checks that makes code verbose (and more imperative). Use functional construct from [`fp-ts`](https://gcanti.github.io/fp-ts/modules/Option.ts.html) types instead (fold, map, mapLeft, bimap, getOrElse, ...).

```typescript
//
// ✅good: use of fp-ts's algebric constructs
//

// with Option
const name: string = fromNullable(user.name).getOrElseL(() => {
  // handle none here
});

// with Either
const validString: string = validatedStringEither.fold(
  err => {
    // handle left here
  },
  value => value
);

//
// ❌bad: imperative code
//

// with Option
const nameOption = fromNullable(user.name);
if (isNone(nameOption)) {
  // handle none here
}

// with Either
if (isLeft(validatedStringEither)) {
  // handle left here
}
```

### [ITALIA-TS-COMMONS](https://github.com/teamdigitale/io-ts-commons)

#### Use [Pot](https://github.com/teamdigitale/io-ts-commons/blob/master/src/pot.ts) to handle the states of remote (potential) data.

Usually an application could deal with loading some data, from remote or local sources.
Pot (for potential) is a data type that helps you to keep track of states, value and errors.
This example shows a reducer that handles the loading of a remote user

```typescript
type User = {
  name: string;
  username: string;
};

type UserState = {
  currentUser: pot.Pot<User, Error>;
};

function userReducer(
  state: UserState = { currentUser: pot.none },
  action: Action
): UserState {
  switch (action.type) {
    case getType(contentServiceLoad.request):
      return {
        ...state,
        // a loading is requested, set the pot in loading with none value
        currentUser: pot.noneLoading
      };
    case getType(contentServiceLoad.success):
      return {
        ...state,
        // the loading is completed, set the pot to some with the retrieved value
        currentUser: pot.some(action.payload)
      };

    case getType(contentServiceLoad.failure):
      return {
        ...state,
        // some errors occured, set the pot to error by setting the error (action.payload)
        // and keep the current state value (eventually we can access the previous value)
        currentUser: pot.toError(state.currentUser, action.payload)
      };
  }
}
```

### REACT / REACT NATIVE

#### Avoid potentially inconsistent state update (React)

[How to handle state in React. The missing FAQ.](https://medium.com/react-ecosystem/how-to-handle-state-in-react-6f2d3cd73a0c)

Updating the state of a component based on the current value of _this.state_ or _this.props_ may lead to inconsistent component state.

```typescript
// this is a potentially inconsistent state update
this.setState({ amount: this.state.amount + 1 });

// simple solution
// - get data from state
// - update data
// - set state with the updated data
const amount = this.state.amount + 1;
this.setState({ amount });

// react solution
// pass an updater to setState
this.setState((prevState: State, _: Props) => {
  return { amount: prevState.amount + 1 };
});
```

### TESTING

#### Test files must be close to the component that test

Place test files in a `__tests__` folder at the same level of the file containing the module you want to test. When possible, call test files with the same name of the module file but with `.test.ts` extension.
To test `foobar.ts`:
```
/src
|_ /__tests__
|  |_ foobar.test.ts
|_ foobar.ts
```  

#### Test titles must be in the form _"it should ..."_

```typescript
// ✅good
it("should have `name` property", () => { ... });

// ❌bad: test title must be in "should" form
it("has `name` property", () => { ... });

// ❌bad: use "it" instead of "test"
test("should have `name` property", () => { ... });
```

#### Avoid throwing exceptions

Sometimes test code can have unhappy branches you don't want to test and hence consider as a test failure. Try not to throw exceptions in there.
If you are using `Jest`, you can use [`expect.assertions`](https://jest-bot.github.io/jest/docs/expect.html#expectassertionsnumber).

```typescript
// ✅good
expect.assertions(1);
myDecoder.fold(
  () => {},
  (value) => {
    expect(value).toEqual( ... );
  }
);

// ❌bad
myDecoder.fold(
  () => {
    throw new Error("test should not pass here");
  },
  (value) => {
    expect(value).toEqual( ... );
  }
);

```

#### Avoid wrapping tests with helper functions

Be explicit in the test you are executing. If you are repeating te same test in many places, don't be tempted to wrap common tests into helpers. We encourage code duplication in favor of more explicit logic.

```typescript
//
// ✅good: each suite has its own test explicitly declared
//

// sum.test.ts
describe("Sum", () => {
  it("should be 6", () => {
    expect(2 + 4).toBe(6);
  });
});

// prod.test.ts
describe("Prod", () => {
  it("should be 8", () => {
    expect(2 * 4).toBe(8);
  });
});


//
// ❌bad: "it" calls are hidden inside an helper function
//

// helpers.ts
export const testValue = (title, value, expected) => (
  it(title, () => {
    expect(value).toBe( ... );
  });
)

// sum.test.ts
import { testValue } from "./helpers.ts";

describe("Sum", () => {
  testValue("should be 6", 2 + 4, 6);
});

// prod.test.ts
import { testValue } from "./helpers.ts";

describe("Prod", () => {+
  testValue("should be 8", 2 * 4, 8);
});
```
