GitHub Copilot Chat Assistant — README for RANDOMLYKEY

# RANDOMLY
A small JavaScript library to generate random numbers, letters, special characters and mixed sequences.

RANDOMLY exposes a single exported object, `randomly`, with two main groups of helpers:
- `between` — generate values between two bounds
- `random` — generate random sequences (numbers, letters, special chars, booleans, or mixes)

> Note: This README documents the library as implemented in `src/index.js`. It also calls out a few caveats and recommended fixes you may want to apply.

---

## Installation
Install however you publish the package (npm/yarn/pnpm). Example:

npm
```
npm install randomlykey
```

yarn
```
yarn add randomlykey
```

Or import directly in a bundler/ESM environment:
```
import { randomly } from 'randomlykey'
```
CommonJS (if you transpile or expose a CJS build):
```
const { randomly } = require('randomlykey')
```

---

## API

randomly is exported as:
```
export const randomly = { between: { ... }, random: { ... } }
```

### between.number(min, max)
Returns a random integer between `min` and `max` (inclusive).
- Parameters: `min` (Number or numeric string), `max` (Number or numeric string)
- Returns: Number

Example:
```js
randomly.between.number(1, 10) // e.g. 7
randomly.between.number('0', '9') // e.g. 3
```

Note: The implementation increments `max` internally to treat the range as inclusive.

### between.string(min, max)
Returns a random lowercase letter between `min` and `max`.
- Parameters: `min` (single-letter string), `max` (single-letter string)
- Returns: single lowercase letter (string)

Example:
```js
randomly.between.string('a', 'f') // e.g. 'c'
```

Limitations: Only accepts single lowercase letters like `'a'`..`'z'`. Uppercase or multi-character ranges are not supported by the current implementation.

### random.number(length)
Generates a random numeric sequence of `length` digits and returns it as a Number.
- Parameter: `length` (Number) — optional (defaults to 18)
- Returns: Number

Example:
```js
randomly.random.number(6) // might return 482913
```

Caveats:
- Because the return value is converted to a Number, leading zeros are lost (e.g. `'007'` becomes `7`).
- Very long lengths can overflow JavaScript Number precision. If you need a safe representation for long numeric sequences or to preserve leading zeros, consider returning a string instead (see Recommendations below).

### random.string(length)
Generates a string of random lowercase letters.
- Parameter: `length` (Number) — optional (defaults to 18)
- Returns: string

Example:
```js
randomly.random.string(5) // e.g. 'qfzmb'
```

### random.boolean()
Returns a random boolean (true or false).
- Usage:
```js
randomly.random.boolean() // true or false
```

### random.specialCharacters(length)
Generates a string composed of special characters.
- Parameter: `length` (Number) — optional (defaults to 18)
- Returns: string

Example:
```js
randomly.random.specialCharacters(4) // e.g. '@$%&'
```

Notes:
- The library maps indices to a small set of special characters.
- The current mapping has duplicate entries (e.g. `$` appears more than once).

### random.mix(length)
Generates a mixed sequence (letters, digits, or special characters).
- Parameter: `length` (Number) — optional (defaults to 18)
- Returns: string

Example:
```js
randomly.random.mix(10) // e.g. 'a7$g4m@q2z'
```

---

## Examples

Import and generate values:

```js
import { randomly } from 'randomlykey'

// number between 1 and 100
console.log(randomly.between.number(1, 100))

// letter between 'b' and 'k'
console.log(randomly.between.string('b', 'k'))

// random 12-digit number (as Number — loses leading zeros)
console.log(randomly.random.number(12))

// random 8-letter string
console.log(randomly.random.string(8))

// random boolean
console.log(randomly.random.boolean())

// 6 special characters
console.log(randomly.random.specialCharacters(6))

// mixed 16-char string
console.log(randomly.random.mix(16))
```

---

## Implementation caveats & recommended improvements
These are present in the current `src/index.js` and you may want to fix or improve them:

- Naming typo: `numberToAlfhabet` is misspelled (should be `numberToAlphabet`).
- `alphabetToNumber` and `numberToAlfhabet` only support single lowercase letters. They do not accept uppercase or multi-character ranges.
- `between.number` increments `max` (works as inclusive, but it's unusual — document or change to explicit inclusive behavior).
- `random.number` returns a Number which:
  - loses leading zeros,
  - can overflow/lose precision for long lengths.
  Consider returning a string for arbitrary-length numeric sequences or exposing both options.
- `numberToSpecialCharacter` has duplicate mappings (e.g., `$` appears twice) and a default that returns the input index (possibly unexpected). Consider using an array and an explicit error/validation for out-of-range indices.
- Input validation is limited. Many functions silently return `undefined` for invalid inputs. Consider throwing errors or returning consistent types.
- `random.between.string('a','z')` expects lowercase only; you may want to normalize input (toLowerCase()).

---

## Tests
No tests are included in the repository. Suggested unit tests:
- between.number: min <= max, min > max, non-numeric input
- between.string: valid letters, uppercase input, invalid/multi-char input
- random.number: lengths 0, 1, >18 (check precision), leading zeros
- random.mix: composition correctness (letters/numbers/specials)
- random.specialCharacters: coverage of each mapped character

---

## Contributing
If you'd like to improve the library:
- Fix naming and typos
- Add input validation and helpful error messages
- Replace long switch statements with concise helpers (char codes or lookup arrays)
- Add unit tests and CI
- Consider TypeScript types for better DX

If you want, I can:
- prepare a refactor that fixes the typos, replaces the switch statements with arrays/char codes, and returns numeric sequences as strings to preserve leading zeros, or
- create a PR with unit tests and a TypeScript version.

Tell me which you'd prefer and I can generate the patch.

---
Author: Cláudio Bento Dala
---

## License

MIT ©claudiobentodaladev