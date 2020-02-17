# Typescript compiler issues

It sometimes happens that the Typescript server or compiler hang during build phase (`tsc`).

This is most likely due to the fact that you're importing two different versions of
the same package. We've noticed that this often happens because of some
different transitive dependencies on `fp-ts` or `io-ts` packages.

To list which version you are currently importing though your
package.json / yarn.lock file:

```sh
yarn why io-ts
yarn why fp-ts
```

If the output mention different versions, pin
the most updated one using the field `resolution` in your package.json:

1. Add

```js
  "resolutions": {
    "fp-ts": "1.17.0"
  }
```

at the end of your package.json (replace 1.17.0 with the most updated version you get by running `yarn why`).
This forces all the other packages that use fp-ts to import the 1.17.0 release.

1. Run `yarn install` again

1. Try to recompile
