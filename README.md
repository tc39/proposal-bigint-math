# BigInt `Math` for JavaScript
ECMAScript Stage-1 Proposal. J. S. Choi, 2021.

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
yet they still do not support them;
they only support regular floating-point JavaScript Numbers.

This proposal extends those functions’ behavior to accept BigInts.
Its philosophy is that BigInts and Numbers
should **always be interchangeable by default**,
**unless** that would cause silent precision loss –
i.e., unless there's a strong reason they should not be.

`abs`\
`sign`\
`clz32`\
`pow` §

`floor` \*\
`ceil` \*\
`round` \*\
`trunc` \*

`log2` †\
`log10` †\
`sqrt` †\
`cbrt` †\
`hypot` † §

`min` ‡\
`max` ‡

**\*** The integer-rounding methods `floor`, `ceil`, `round`, and `trunc`
are included as identity functions
in order to increase interchangability between BigInts and Numbers.
This is being debated in [issue #8][].

**†** `log10`, `log2`, `sqrt`, and `cbrt` all return BigInts when given BigInts.
They truncate BigInt results towards zero
when their results would not have had integer mathematical values,
just like with BigInt division:

* `(3000000000n - 1n) / 1000000000n` already returns `2n`.
* `cbrt(131329n ** 3n - 1n)` would return `131328n`.
* `sqrt(67108865n ** 2n - 1n)` would return `67108864n`.
* `log2(2n ** 49n - 1n)` would return `48n`.
* `log10(10n ** 15n - 1n)` would return `14n`.

**‡** `min` and `max` accept mixed numeric types:\
`min(0, 1n, -1)` evaluates to `0`,\
and `max(0, 1n, -1)` evaluates to `1n`.\
This is well defined because `<` is well defined over mixed numeric types;
there is no loss of precision.

When `Math.min` and `Math.max` receive values of different numeric types
that nevertheless have equivalent mathematical values,
then `min` prefers the leftmost value and `max` prefers the rightmost value.
For example, `Math.min(0, 0n)` is `0` and `Math.max(0, 0n)` is `0n`.
See [issue #3][].

**§** In contrast to `min` and `max`, `pow` and `hypot` do not accept mixed types;
for example, `pow(4, 2n)` and `hypot(1n, 2)` will throw TypeErrors.
(Nullary `hypot()`, with no arguments, still returns the number value `+0`.
The developer must ensure that `hypot`’s arguments are not empty
in order to guarantee that `hypot` returns a BigInt.
See [issue #6][].

***

Existing `Math` functions that would not make sense with BigInts
are excluded from this proposal. These include:

|`Math` method  | Exclusion reason
| ------------- | ----------------
|`acos`         | Transcendental, very difficult to calculate when large
|`acosh`        | Transcendental
|`asin`         | Transcendental
|`asinh`        | Transcendental
|`atan`         | Transcendental
|`atan2`        | Transcendental
|`atanh`        | Transcendental
|`cos`          | Transcendental
|`cosh`         | Transcendental
|`exp`          | Transcendental
|`expm1`        | Transcendental
|`fround`       | Returns floating-point numbers by definition
|`imul`         | [Issue #9][]
|`log`          | Transcendental
|`log1p`        | Transcendental
|`random`       | No conceptual integer-only analogue
|`sin`          | Transcendental
|`sinh`         | Transcendental
|`tan`          | Transcendental
|`tanh`         | Transcendental

[issue #3]: https://github.com/js-choi/proposal-bigint-math/issues/3#issuecomment-912133467
[issue #6]: https://github.com/js-choi/proposal-bigint-math/issues/6
[issue #8]: https://github.com/js-choi/proposal-bigint-math/issues/8
[issue #9]: https://github.com/js-choi/proposal-bigint-math/issues/9
