# BigInt `Math` for JavaScript
ECMAScript Stage-1 Proposal. J. S. Choi, 2021.

* **[Specification][]**
* **Babel plugin**: Not yet

[specification]: https://tc39.es/proposal-bigint-math/

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
yet JavaScript still does not have not support them.
They only support regular floating-point JavaScript Numbers.
This proposal adds the following functions
to the BigInt object acting as a namespace:

* `BigInt.abs`
* `BigInt.sign`
* `BigInt.sqrt`*
* `BigInt.cbrt`*
* `BigInt.pow`
* `BigInt.min`†
* `BigInt.max`†

* All of these functions return BigInts.
* None of these functions accept any arguments other than BigInts.
* * `sqrt` and `cbrt` truncate the result toward 0 into a BigInt.
* † `min` and `max` require at least one argument.

## Philosophy
This proposal balances performance with precedent.

1. Monomorphic functions are much easier for engines to optimize
   than polymorphic functions.
2. BigInts and Numbers are not semantically interchangeable.
   Developers should be aware when using BigInt versus Numbers to avoid errors.
3. It is ergonomically desirable for BigInt math functions’ API
   to be as consistent with existing number functions in Math as possible.
4. Numbers and BigInts are primitives,
   so math functions should be static functions like `BigInt.abs(v)`,
   rather than prototype methods like `v.abs()`.
   * This matches the precedent of `BigInt.asIntN` and `BigInt.asUintN`.
   * This is unlike the proposed [Decimal128s][].
     Decimal128s will be objects and thus should use prototype methods.)

[Decimal128s]: https://github.com/tc39/proposal-decimal

## Vision
This initial proposal adds only a few first `BigInt` methods.
The vision is that this proposal would open up the way
to new proposals for new BigInt math functions, like:

* `BigInt.gcd(v)`: Greatest common divisor (GCD)
* `BigInt.popCount(v)`: Population count
* `BigInt.bitLength(v)`: Bit length, i.e., truncating log<sub>2</sub>
* `BigInt.modPow(v, exponent, modulus)`: Modular exponentiation
* `BigInt.modInverse(v, modulus)`: Modular multiplicative inverse
* `BigInt.fromString(value, radix)`: Base-n string parsing
* `BigInt.toByteArray(v, endian)`: Conversion to byte array
* `BigInt.fromByteArray(bytes, endian)`: Conversion from byte array

Some of these may also be appropriate for ordinary integer Numbers.

## Excluded `Math`
`Math` functions that would not make sense with BigInts
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
|`ceil`         | No known use case; `Math.ceil(3n / 2n) == 1` may be surprising
|`clz32`        | No known use case
|`cos`          | Transcendental
|`cosh`         | Transcendental
|`exp`          | Transcendental
|`expm1`        | Transcendental
|`floor`        | No known use case
|`fround`       | Returns floating-point numbers by definition
|`hypot`        | No known use case
|`imul`         | No known use case
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
