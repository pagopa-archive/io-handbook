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
// create a new object using spread operator to update only that specific field
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

For some scenario, you may be interested why a computation failed.
Using _Option_ is useful when you want to know if a value is present or not.
The reason why it is not present it's not interesting.
If you want to know more about computation failure you should use Either.

Consider this example

> write a function to validate a password
>
> it has to be:
>
> length >= 8
>
> contain at least a char in upper case
>
> contain at least a number

#### a possible implementaion
```typescript
// return the error message if the password is not compliant, null otherwise
export const validatePassword = (pwd: string): string | null => {
  if (pwd.length < 8) {
    return "at least 8 characters";
  }
  if (!/[A-Z]/g.test(pwd)) {
    return "at least one capital letter";
  }
  if (!/[0-9]/g.test(pwd)) {
    return "at least one number";
  }
  return null;
};

const myPws = "123456A";

const validatePwd = validatePassword(myPws);
if (myPws === null) {
  console.log("password is ok");
} else {
  console.error(`the password is too weak: ${validatePwd}`);
}
```
#### using Either
```typescript
// return the error if the password is not compliant, the password otherwise
export const validatePassword = (pwd: string): Either<Error, string> => {
  if (pwd.length < 8) {
    return left(Error("at least 6 characters"));
  }
  if (!/[A-Z]/g.test(pwd)) {
    return left(Error("at least one capital letter"));
  }
  if (!/[0-9]/g.test(pwd)) {
    return left(Error("at least one number"));
  }
  return right(pwd);
};
const myPws = "123456A";
const maybePassword = validatePassword(myPws);
maybePassword.fold(
  err => {
    console.error(`the password is too weak: ${err.message}`);
  },
  pwd => {
    console.log("password is ok");
  }
);
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
