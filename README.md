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
they only support regular JavaScript Numbers.
This proposal extends those functions’ behavior to accept BigInts:

* `Math.abs`
* `Math.acosh`
* `Math.asinh`
* `Math.atanh`
* `Math.cosh`
* `Math.cbrt`
* `Math.exp`
* `Math.expm1`
* `Math.hypot`
* `Math.log`
* `Math.log1p`
* `Math.log10`
* `Math.log2`
* `Math.min`
* `Math.pow`
* `Math.sign`
* `Math.sinh`
* `Math.sqrt`
* `Math.tanh`

In addition, the following new methods are added
as special versions of existing methods for BigInts,
due to their variadic parameters:
* `Math.bigHypot` for `Math.hypot` (returns `0n` with no arguments)
* `Math.bigMax` for `Math.max` (throws TypeError with no arguments)
* `Math.bigMin` for `Math.min` (throws TypeError with no arguments)

Existing `Math` functions that would not make sense with BigInts
are excluded from this proposal.
For instance, because BigInts never have fractional parts,
`Math.floor` would not be useful over BigInts, and so `Math.floor` is excluded.
Likewise, because cosines range between −1 and +1,
`Math.cos` cannot return useful BigInt values
(which would be restricted to `-1n`, `0n`, and `+1n`),
and so `Math.cos` is excluded.

As with the rest of the JavaScript language,
this proposal avoids any implicit conversion between Numbers or BigInts.
All functions above, when given BigInt arguments, return BigInts (never regular Numbers).

Functions that take multiple numeric arguments, like `Math.max`,
must be given all Number arguments
or all BigInt arguments or else they will throw a TypeError.
We do this to avoid unexpected returning of Numbers instead of BigInts –
for example, `Math.hypot(arrayOfBigInts)` returning the Number `+0` instead of `0n`
whenever `arrayOfBigInts` happens to be empty.

## Design choices

| Dilemma | Choice
| ------- | ------
Should we extend `round`, `floor`, etc. take BigInts? | No, but this could change.
Are there any real-use cases for hyperbolic/root/logarithm functions on BigInts? | We don’t yet know, but we’re extending them anyway.
If we do extend hyperbolic/root/logarithm functions to accept BigInts, then should they return BigInts, and if so, how should they be rounded from their irrational mathematical values? | Yes, they should return implementation-approximated BigInts.
Should trigonometric functions, which generally have small domains and ranges, return Numbers when given BigInts? | No, and because of this we are currently not extending the trigonometric functions to accept BigInts.
What should the variadic `Math.hypot`, `max`, and `min` return when given no BigInt arguments? | We make new `big` versions of each of them, and we make `bigHypot` return `0`, and `bigMax`/`bigMin` throw a TypeError. We do this to avoid unexpected returning of Numbers instead of BigInts – for example, `Math.hypot(arrayOfBigInts)` returning the Number `+0` instead of `0n` whenever `arrayOfBigInts` happens to be empty.

## Real-world examples
Only minor formatting changes have been made to the status-quo examples.

<table>
<thead>
<tr>
<th>Status quo
<th>With binding

<tbody>
<tr>
<td>

```js
???
```
From ???.

<td>

```js
???
```

</table>
