# History of BigInt Math

## 2021-09
Presented to [plenary meeting for Stage 1 on 2021-09-01][2021-09-01]. Was accepted.


[2021-09-01]: https://github.com/tc39/notes/blob/HEAD/meetings/2021-08/sept-01.md#bigint-math-for-stage-1

## 2021-10
Presented to [plenary meeting as an update on 2021-10-26][2021-10-26].

[2021-10-26]: https://github.com/tc39/notes/blob/HEAD/meetings/2021-10/oct-26.md#bigint-math-update

## 2022-05
Presented to [incubator meeting on 2022-05-06][2022-05-06] with [follow-up
discussion on Matrix][2022-05-06 Matrix].

[2022-05-06]: https://github.com/tc39/incubator-agendas/blob/main/notes/2022/05-06.md
[2022-05-06 Matrix]: https://matrixlogs.bakkot.com/TC39_Delegates/2022-05-06

## 2023-11
The [operator-overloading proposal was withdrawn
at the 2023-11 plenary][2023-11].

[2023-11]: //github.com/tc39/notes/blob/main/meetings/2023-11/november-28.md#withdrawing-operator-overloading

* Discussion at the plenary makes clear
  that the polymorphism of the BigInt operators
  is now seen as an unforeseen burden and potential “mistake”
  imposing much complexity on the JavaScript engines with relatively little gain.
* Even the original BigInt champion, Daniel Ehrenberg,
  added that he too thinks that BigInt math functions
  should be separate functions in `BigInt` and not functions in `Math`.

## 2025-04

* Jesse Alama, champion of the Decimal proposal,
  [announces that they have decided to eschew type overloading
  for math functions on `Decimal`s][decimal #25].
* Following these precedents, the BigInt Math champion
  [changes BigInt Math to use separate functions in `BigInt`
  and not functions in `Math`][2025-04 #13].

[decimal #25]: https://github.com/tc39/proposal-decimal/issues/25
[2025-04 #13]: https://github.com/tc39/proposal-bigint-math/issues/13#issuecomment-2774304480
