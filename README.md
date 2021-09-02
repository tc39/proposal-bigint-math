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
yet they still do not support them;
they only support regular floating-point JavaScript Numbers.
This proposal extends those functions’ behavior to accept BigInts:

* `Math.abs`
* `Math.ceil`
* `Math.cbrt`
* `Math.floor`
* `Math.hypot`
* `Math.log10`
* `Math.log2`
* `Math.max`
* `Math.min`
* `Math.pow`
* `Math.round`
* `Math.sign`
* `Math.sqrt`
* `Math.trunc`

`log10`, `log2`, `sqrt`, and `cbrt` all truncate BigInt results towards zero
when their results would not have been integers,
just like BigInt division.

In addition, the following new methods are added
as special versions of existing methods for BigInts,
due to their variadic parameters:
* `Math.bigHypot` for `Math.hypot` (returns `0n` with no arguments)
* `Math.bigMax` for `Math.max` (throws TypeError with no arguments)
* `Math.bigMin` for `Math.min` (throws TypeError with no arguments)

(We do this to avoid unexpected returning of regular Numbers instead of BigInts –
for example, `Math.hypot(arrayOfBigInts)` returning the Number `+0` instead of `0n`
whenever `arrayOfBigInts` happens to be empty.)

<details>
<summary>Alternative solution for BigInt `hypot`, `max`, and `min`</summary>

Alternatively, instead of adding `Math.bigHypot`, `Math.bigMax`, and `Math.min`,
we could the add `hypot`, `max`, and `min` methods to `BigInt` (and to `Number` too).
See [issue #3](https://github.com/js-choi/proposal-bigint-math/issues/3).

</details>

As with the rest of the JavaScript language,
this proposal **avoids any implicit conversion** between Numbers or BigInts.
The only built-in function that could ever
return a regular Number when given a BigInt remains `Number`,
and the only built-in function that could ever
return BigInt when given a regular Number is `BigInt`.
All the other functions above, when given BigInt arguments,
return BigInts (never regular Numbers) or throw TypeErrors.

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
|`clz32`        | ???
|`cos`          | Transcendental
|`cosh`         | Transcendental
|`exp`          | Transcendental
|`expm1`        | Transcendental
|`fround`       | Returns floating-point numbers by definition
|`imul`         | ???
|`log`          | Transcendental
|`log1p`        | Transcendental
|`random`       | No conceptual integer-only analogue
|`sin`          | Transcendental
|`sinh`         | Transcendental
|`tan`          | Transcendental
|`tanh`         | Transcendental
