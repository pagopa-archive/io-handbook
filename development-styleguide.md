## 👨‍💻 DEVELOPMENT STYLE GUIDE

A practical reason to use a style guide is that it helps you write code.
Furthermore, it shows commons mistakes or scenarios and how to handle them

### Typescript

#### Avoid object modification.

- Due to its dynamic nature, JavaScript makes it extremely easy to modify objects that you do not own
- Such practice should generally be avoided as it can lead to maintenance problems and can produce hard to find bugs
- If you don't own it, don't modify it

side effects, example

```typescript
type Person = {
  name: string;
  surname: string;
  age: number;
};

function printNameMutation(p: Person) {
  console.log(`${p.name} ${p.surname}`);
  // this object mutation could break software logic
  p.name = "Rob";
}

function printName(p: Person) {
  console.log(`${p.name} ${p.surname}`);
}

const david: Person = { name: "David", surname: "Smith", age: 20 };
printNameMutation(david); // David Smith
// the object has been mutated
printName(david); // Rob Smith
```

### [FP-TS](https://gcanti.github.io/fp-ts/modules/Option.ts.html)

#### Use [Options](https://github.com/gcanti/fp-ts/blob/1.12.0/src/Option.ts) to handle nullable values.

```typescript
import { Option, some } from "fp-ts/lib/Option";

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

// use option to avoid if/else
type UserO = {
  id: number;
  emailPreference: Option<boolean>;
};

const userO: UserO = { id: 1, emailPreference: some(true) };
// send email if user's preference is set to true
userO.emailPreference.fold(
  () => {
    console.log("can't send email due to the user's preference");
  },
  canSendEmail => {
    // send email
  }
);
```

### REACT / REACT NATIVE

#### Avoid potentially inconsistent state update (React)

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
