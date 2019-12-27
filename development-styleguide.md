## 👨‍💻 DEVELOPMENT STYLE GUIDE

A practical reason to use a style guide is that it helps you write code.
Furthermore, it shows commons mistakes or scenarios and how to handle them

### Typescript

#### Avoid object modification.

- Due to its dynamic nature, JavaScript makes it extremely easy to modify objects that you do not own
- Such practice should generally be avoided as it can lead to maintenance problems and can produce hard to find bugs
- If you don't own it, don't modify it


```typescript
let bob = { name : "Bob", surname: "Smith", age : 23 }
bob.age = 24; // avoid
// create a new object using spread operator to update only specific fields
const olderBob = { ...bob, age: 24 }
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

Consider this example

write a function to validate a string
it has to be greater than 8 chars

#### a possible implementaion
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
  //ops something goes wrong
  console.error(err);
}
```
#### using Either
```typescript
export const validateStringEither = (value: string): Either<Error, string> => {
  if (value.length < 8) {
    return left(Error("at least 6 characters"));
  }
  return right(value);
};

const validatedStringEither = validateStringEither(myString);

validatedStringEither.fold(
  err => {
    console.error(`the password is too weak: ${err.message}`);
  },
  value => {
    // do stuffs with the validated string
  }
);
```
if your validation function could raise multiple errors
you should use [Validation][https://dev.to/gcanti/getting-started-with-fp-ts-either-vs-validation-5eja] to collect them

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
