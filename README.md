# ECMAScript Object.clone / clone

## [Status](https://tc39.github.io/process-document/)

**Stage**: Pre-0

**Author**: Mike Piccolo (FullStack Labs, [@mfpiccolo](https://twitter.com/mfpiccolo))

**Champions**: -

## Introduction

### Part 1 - Long standing problem in JavaScript

Cloning in JavaScript has been an open question for a long time that has been answered in userland in many different ways.

[How do I correctly clone a javascript object](https://stackoverflow.com/questions/728360/how-do-i-correctly-clone-a-javascript-object)

[What is the most efficient way to deep-clone-an-object-in-javascript](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript)

These two StackOverflow questions have both been open for over ten years,are amonst the highest viewed questions of all time and are still extreemly active. Folks are still actively trying to figure out this problem.

### Part 2 - Immutable patterns

JavaScript has been moving towards immutable patterns.

[immutable](https://www.npmjs.com/package/immutable) - ~2.5 million weekly downloads

[redux](https://www.npmjs.com/package/redux) - ~3 million weekly downloads

[immer](https://www.npmjs.com/package/immer) - ~2 million weekly downloads

## Object method

```js
const test = { a: { b: { c: 1 } } }

const copy = Object.clone(test)
assert(test.a.b.c === copy.a.b.c)
```

## Immutable updates

Following immer's api, you can update the new object

```js
const test = { a: { b: { c: 1 } } }
const copy2 = Object.clone(test, draft => (draft.a.b.c = 2))
assert(test.a.b.c !== copy2.a.b.c)
assert(copy2.a.b.c === 2)
```

Example reducer

```js
const reducer = (state, action) =>
  Object.clone(state, draft => {
    switch (action.type) {
      case RECEIVE_PRODUCTS:
        action.products.forEach(product => {
          draft[product.id] = product
        })
      case REMOVE_PRODUCT:
        delete draft[action.productId]
    }
  })
```

### Related Active Proposals

- [Structured Clone](https://github.com/dslomov/ecmascript-structured-clone)
- [Const Value Types](https://github.com/rricard/proposal-const-value-types)
- [Immutable Data Structures](https://github.com/sebmarkbage/ecmascript-immutable-data-structures)
