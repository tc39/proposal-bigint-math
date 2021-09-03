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

As with the rest of the JavaScript language,
this proposal **avoids any implicit conversion** between Numbers or BigInts.
The only built-in function that could ever
return a regular Number when given a BigInt remains `Number`,
and the only built-in function that could ever
return BigInt when given a regular Number is `BigInt`.
All the other functions above, when given BigInt arguments,
return BigInts (never regular Numbers) or throw TypeErrors.

However, `min` and `max` accept mixed numeric types:
`Math.min(0, 1n, -1)` evaluates to `1n`,
and `Math.max(0, 1n, -1)` evaluates to `0`.
This is well defined because `<` is well defined over mixed numeric types;
there is no loss of precision.
(In contrast, `Math.pow` and `Math.hypot` do not accept mixed types.)

When `Math.min` and `Math.max` receive values of different numeric types
that nevertheless have equivalent mathematical values,
then `min` prefers the leftmost value and `max` prefers the rightmost value.
For example, `Math.min(0, 0n)` is `0` and `Math.max(0, 0n)` is `0n`.

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
