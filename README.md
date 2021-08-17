# BigInt `Math` for JavaScript
ECMAScript Stage-0 Proposal. J. S. Choi, 2021.

* **[Specification][]**
* **Babel plugin**: Not yet

[specification]: http://jschoi.org/21/es-bigint-math/

## Description
(A [formal draft specification][specification] is available.)

BigInts are important for a myriad of
mathematical, financial, and scientific applications
(such as in the Web Performance API),
and they have been therefore a valuable addition to JavaScript
since their standardization in ES 2021.

Several built-in `Math` functions
would make sense with BigInts,
yet still do not support them;
they only support regular floating-point JavaScript Numbers.
This proposal extends those functions’ behavior to accept BigInts:

* `Math.abs`
* `Math.acosh`
* `Math.asinh`
* `Math.ceil`
* `Math.clz32`
* `Math.cosh`
* `Math.cbrt`
* `Math.exp`
* `Math.expm1`
* `Math.floor`
* `Math.hypot`
* `Math.imul`
* `Math.log`
* `Math.log1p`
* `Math.log10`
* `Math.log2`
* `Math.min`
* `Math.pow`
* `Math.round`
* `Math.sign`
* `Math.sinh`
* `Math.sqrt`
* `Math.trunc`

In addition, the following new methods are added
as special versions of existing methods for BigInts,
due to their variadic parameters:
* `Math.bigHypot` for `Math.hypot` (returns `0n` with no arguments)
* `Math.bigMax` for `Math.max` (throws TypeError with no arguments)
* `Math.bigMin` for `Math.min` (throws TypeError with no arguments)

(We do this to avoid unexpected returning of regular Numbers instead of BigInts –
for example, `Math.hypot(arrayOfBigInts)` returning the Number `+0` instead of `0n`
whenever `arrayOfBigInts` happens to be empty.)

Existing `Math` functions that would not make sense with BigInts
are excluded from this proposal. These include:

|`Math` method  | Exclusion reason
| ------------- | ----------------
|`acos`         | Mathematical domain is between −1 and +1
|`asin`         | Mathematical domain is between −1 and +1
|`atan`         | Mathematical range is between −π/2 and +π/2
|`atan2`        | Mathematical range is between −π/2 and +π/2
|`atanh`        | Mathematical domain is between −1 and +1
|`cos`          | Mathematical range is between −1 and +1
|`fround`       | Returns floating-point numbers by definition
|`random`       | No conceptual analogue
|`sin`          | Mathematical range is between −1 and +1
|`tanh`         | Mathematical range is between −1 and +1

For instance, because cosines range between −1 and +1,
`Math.cos` cannot return useful BigInt values
(which would be restricted to `-1n`, `0n`, and `+1n`).

## Design choices

As with the rest of the JavaScript language,
this proposal **avoids any implicit conversion** between Numbers or BigInts.
The only built-in function that could ever
return a regular Number when given a BigInt remains `Number`,
and the only built-in function that could ever
return BigInt when given a regular Number is `BigInt`.
All the other functions above, when given BigInt arguments,
return BigInts (never regular Numbers) or throw TypeErrors.

| Dilemma | Choice
| ------- | ------
Should we extend `round`, `floor`, `ceil`, and `trunc` to take BigInts? | Yes: they are just identity functions.
Are there any real-use cases for applying irrational-returning functions like `sqrt` to BigInts? | We don’t yet know, but we’re extending them anyway.
Should those irrational-returning functions return BigInts? | Yes, we must continue to avoid implicit conversions.
How should their irrational results be rounded? | They should return implementation-approximated BigInts.
Should `sin`, `cos`, `atan`, `atan2`, and `tanh` return BigInts when given BigInts? | No, returning only one of three values (`-1n`, `0n`, or `+1n`) is not useful for trigonometric functions.
Should `sin`, `cos`, `atan`, `atan2`, and `tanh` return Numbers when given BigInts? | No, we must continue to avoid implicit conversions. Because of this, we are not extending these functions to accept BigInts.
What should the variadic `Math.hypot`, `max`, and `min` return when given no BigInt arguments? | We have made new `big` versions of each of these functions, and we make `bigHypot` return `0`, and `bigMax`/`bigMin` throw a TypeError. We do this to avoid unexpected returning of Numbers instead of BigInts – for example, `Math.hypot(arrayOfBigInts)` returning the Number `+0` instead of `0n` whenever `arrayOfBigInts` happens to be empty.
