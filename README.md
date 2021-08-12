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
