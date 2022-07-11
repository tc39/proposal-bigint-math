# BigInt `Math` for JavaScript
ECMAScript Stage-1 Proposal. J. S. Choi, 2021.

* **[Specification][]**
* **Babel plugin**: Not yet

[specification]: http://jschoi.org/21/es-bigint-math/

## Description
(A [formal draft specification][specification] is available.)

BigInts are important for a myriad of
mathematical, financial, scientific, and timing applications
(such as in the [Node.js `process.hrtime.bigint` API][hrtime]),
and they have been therefore a valuable addition to JavaScript
since their standardization in ES 2021.

[hrtime]: https://nodejs.org/api/process.html#process_process_hrtime_bigint

Several built-in `Math` functions
would make sense with BigInts,
yet they still do not support them;
they only support regular floating-point JavaScript Numbers.
This proposal extends those functions’ behavior to accept BigInts:

`abs`\
`sign`\
`sqrt`\
`pow` \*

`min` †\
`max` †

**\*** `pow` does not accept mixed types.
`pow(4, 2n)` will throw a TypeError.

**†** `min` and `max` accept mixed numeric types:\
`min(0, 1n, -1)` evaluates to `0`,\
and `max(0, 1n, -1)` evaluates to `1n`.\
This is well defined because `<` is well defined over mixed numeric types;
there is no loss of precision.

When `Math.min` and `Math.max` receive values of different numeric types
that nevertheless have equivalent mathematical values,
then `min` prefers the leftmost value and `max` prefers the rightmost value.
For example, `Math.min(0, 0n)` is `0` and `Math.max(0, 0n)` is `0n`.
(See [issue #3][].)

## Philosophy
The philosophy is to be **consistent with the precedents** already set by the language.
These precedents include the following five rules:

1. BigInts and Numbers are *not* semantically interchangeable.
   It is important for the developer to reason about them differently.
2. But, for ease of use, *many* (but not all) numeric operations
   (such as division `/` and exponentiation `**`)
   are type overloaded to accept both Numbers and BigInts.
3. These type-overloaded numeric operations
   *cannot mix* Numbers and BigInts, with the exception of *comparison* operations.
4. Some numeric operations are not overloaded (such as unary `+`).
   The programmer has to remember which operations are overloaded and which ones are not.
5. asm.js is still important, and operations on which it depends are not type overloaded.

In this precedent, only syntactic operators are currently considered as math operations.
We extend this precedent such that `Math` methods are also considered math operations.

## Vision
This initial proposal overloads only a few first `Math` methods.
The vision is that this proposal would open up the way
to new proposals that would further extend `Math` with type-overloaded methods.
These may include:

* [`Math.popCount`](https://vaibhavsagar.com/blog/2019/09/08/popcount/)
* [`Math.bitLength`](https://en.wikipedia.org/wiki/Bit-length)
* [`Math.modPow`](https://en.wikipedia.org/wiki/Modular_exponentiation)
* [`Math.gcd`](https://en.wikipedia.org/wiki/Greatest_common_divisor)
* [`Math.modInv`](https://en.wikipedia.org/wiki/Modular_multiplicative_inverse)
* [`Math.range`](https://github.com/tc39/proposal-Number.range)

## Excluded `Math`
Similarly to how some numeric operators are not overloaded (such as unary `+`),
many `Math` functions that would not make sense with BigInts
are excluded from this proposal. These include:

|`Math` method  | Exclusion reason
| ------------- | ----------------
|`acos`         | Transcendental: very difficult to calculate when large
|`acosh`        | Transcendental
|`asin`         | Transcendental
|`asinh`        | Transcendental
|`atan`         | Transcendental
|`atan2`        | Transcendental
|`atanh`        | Transcendental
|`cbrt`         | No known use case
|`ceil`         | No known use case; `Math.ceil(3n / 2n) == 1` may be surprising
|`clz32`        | No known use case
|`cos`          | Transcendental
|`cosh`         | Transcendental
|`exp`          | Transcendental
|`expm1`        | Transcendental
|`floor`        | No known use case
|`fround`       | Returns floating-point numbers by definition
|`hypot`        | No known use case
|`imul`         | No known use case; may complicated asm.js (see [issue #9][])
|`log`          | Transcendental
|`log10`        | Truncation may be surprising; no known use case
|`log2`         | Truncation may be surprising; deferred to future `bitLength` proposal
|`log1p`        | Transcendental
|`random`       | No conceptual integer-only analogue
|`round`        | No known use case; `Math.round(3n / 2n) == 1` may be surprising
|`sin`          | Transcendental
|`sinh`         | No known use case
|`tan`          | Transcendental
|`tanh`         | Transcendental
|`trunc`        | No known use case

[issue #3]: https://github.com/js-choi/proposal-bigint-math/issues/3#issuecomment-912133467
[issue #6]: https://github.com/js-choi/proposal-bigint-math/issues/6
[issue #8]: https://github.com/js-choi/proposal-bigint-math/issues/8
[issue #9]: https://github.com/js-choi/proposal-bigint-math/issues/9
