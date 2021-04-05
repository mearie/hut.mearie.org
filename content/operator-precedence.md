---
title: 연산자 우선순위
categories: computing
math: true
changes:
- - 2021-04-06
  - 첫 작성.
---

[연산자](operator) 우선순위(operator precedence)는 주어진 [식](expression)에서 어느 연산이 먼저 계산되는 지를 나타낸다.
예를 들어서 일반적인 [사칙 연산](arithmetic-operations)을 기준으로 다음이 성립한다.
`$$3 + 4 \times 5 \times 6 = 3 + ((4 \times 5) \times 6) = 3 + (20 \times 6) = 3 + 120 = 123$$`
이 식이 성립하는 이유는 우리가 덧셈(`$+$`)에 앞서 곱셈(`$\times$`)을 먼저 계산하고,
덧셈과 곱셈 모두 연달아 나올 경우 왼쪽부터 계산한다고 **약속**했기 때문이다.
이는 곱의 합이 합의 곱보다 더 많이 나타나므로 이리 하면 괄호를 줄일 수 있다는 경험칙에 따른 것이며 그 이상 그 이하도 아니다.
`$48 \div 2(9 + 3)$`과 같이, 이러한 약속이 없다면 굳이 해석을 하지 말고 모호하므로 잘못되었다고 판단하는 것이 옳다.

연산자 우선순위는 크게 연산자들 사이의 [순서](#order)와,
같은 부류의 연산자들이 연달아 있을 때 어떻게 할지를 결정하는 [결합칙](#associativity)으로 나눠 설명할 수 있다.
우습게도 ‘우선순위’라는 이름과는 달리 순서만을 다루진 않으며,
[소괄호](parentheses) `()`와 같은 것은 연산자로 취급하지 않거나 가장 먼저 평가되는 연산자로 취급한다는 데도 주의.

[형식 언어](formal-language)에서 연산자 우선순위는 [연산자 우선순위 문법](operator-precedence-grammar)으로 표현할 수 있다.

## 순서 {#order}

연산자의 계산 **순서**는 `$a \star b \diamond c$`와 같이 연산자 둘이 연달아 나올 때 둘 중 어느 쪽이 더 ‘강한’지를 나타낸다.
둘 중 한 쪽이 더 강하면 그 연산자가 먼저 계산되고,
어느 쪽도 강하지 않으면 순서에 따르는 [결합칙](#associativity)으로 넘어가게 된다.
이를테면 사칙 연산의 경우 다음 순서에서 먼저 나올수록 더 강하다.

1. 괄호가 있다면 맨 안쪽 괄호부터

2. 마이너스 기호 (`$-42$` 등)

3. 곱셈(`$\times$`)과 나눗셈(`$\div$`)

4. 덧셈(`$+$`)과 뺄셈(`$-$`)

이제 `$3 + 4 \times 5$` 같은 경우 곱셈(3)이 덧셈(4)보다 먼저 나오므로 우선 계산된다.
`$3 - 4 + 5$` 같은 경우 뺄셈(4)과 덧셈(4)은 같은 순서이므로 왼쪽에 나오는 뺄셈이 먼저 계산된다.
마이너스 기호와 뺄셈은 모양이 같은데,
기호 앞에 숫자나 괄호가 나오면 뺄셈, 다른 연산자가 나오면 마이너스로 구분할 수 있다.

{{% hn 23051165 %}}
It is pretty common that arithmetic comparison operators are grouped to a single precedence level and that's not a problem. But in Python `is`, `is not`, `in` and `not in` are also in that level. In particular two operands of `in` and `not in` have different types unlike others. Mixing them are, either with or without chained operators, almost surely incorrect.

This kind of precedence issue can be solved by introducing non-associative pairs of operators (or precedence levels), something that---unfortunately---I don't see much in common programming languages. [...] In fact, there is already one non-associative pair in Python: `not` and virtually every operator except boolean operators. It is understandable: the inability to parse `3 + not 4` is marginal but you don't want `3 is not 4` to be parsed as `3 is (not 4)`. My point is that, if we already have such a pair why can't we have more?

{{% /hn %}}

{{<claim>}}
이 순서를 나타낼 때는 보통 위와 같이 먼저 계산되는 것부터 나중에 계산되는 것까지 순서대로 나열하는 경향이 있다.
이는 연산자 사이의 계산 순서가 [전순서](total-order)라는 걸 함의하는데,
**이상적으로는 전순서가 아니라 [부분순서](partial-order)여야 한다.**
즉 어떤 연산자들끼리는 순서가 없으므로 `$a \star b \diamond c$`와 같이 연달아 쓸 수 없고 무조건 괄호를 쳐서 `$(a \star b) \diamond c$`나 `$a \star (b \diamond c)$`로 써야 한다는 것이다.
여기에는 그만한 이유가 있다.

* 연산자 종류가 많아질수록 연산자들 사이에 ‘자연스러운’ 순서를 찾기가 어려워진다.
  이를테면 [C](c-language)에는 사칙 연산과 함께 [비트 연산](bitwise-operations)이 있는데,
  둘을 연달아 쓰는 것은 무슨 [코드 골프](code-golf) 같은 걸 하지 않는 이상 드문 일이다.

* 설령 자연스러운 순서가 존재하더라도 읽을 때 오해할 여지가 있으면 순서를 두지 않는 게 바람직하다.
  이를테면 논리합(`$\vee$`)과 논리곱(`$\wedge$`) 사이의 자연스러운 순서는 사칙 연산과 똑같다고 볼 수도 있으나,
  사칙 연산과는 달리 둘 사이에는 서로 [분배 법칙](distributivity)이 성립하므로 어느 한 쪽이 우선순위를 가지면 이를 가려 버릴 소지가 있다.

## 결합칙 {#associativity}

연산자의 **결합칙**은 같은 [순서](#order)를 가지는 연산자들이 연속으로 나올 때,
연산자와 무관하게 어느 방향으로 계산해야 하는지를 나타낸다.
흔히 쓰이는 결합칙으로 다음이 있다.

* 왼쪽부터 결합(left associative):
  사칙 연산(`$3 + 4 - 5 = (3 + 4) - 5$`)이 대표적이다.

* 오른쪽부터 결합(right associative):
  [거듭제곱](exponentiation)(`$3^{4^5} = 3^{\left(4^5\right)}$`)이 대표적이다.
  거듭제곱의 경우 `$\left(3^4\right)^5 = 3^{4 \times 5}$`이므로 왼쪽부터 결합해서 얻을 수 있는 이득이 별로 없기 때문이다.

* 통째로 결합(chained associative) 또는 n진(n-ary) 연산:
  `$a_0 \star \cdots \star a_{n-1}$`과 같은 식을 이항 연산 `$\star$`를 `$n-1$`회 계산하는 것으로 보지 않고,
  피연산자를 `$n$`개 받는 연관된 함수 `$\star_n(a_0, \cdots, a_{n-1})$`로 해석하는 것이다.
  대표적으로 순서 비교 연산(`$3 < 4 \le 5$`)이 있는데,
  여기에서 보듯 연산자가 달라도 통째로 결합이 가능할 수 있다.

* 비결합(non-associative):
  연산자를 아예 연달아 쓸 수 없는 경우로,
  [내적](inner-product)(`$\mathbf{a} \cdot \mathbf{b} \cdot \mathbf{c}$`)과 같이 입력(이 경우 벡터)과 출력(이 경우 스칼라)이 다른 종류일 경우 흔히 볼 수 있다.

## 같이 보기 {#see-also}

* [*Operator precedence is broken*](https://foonathan.net/2017/07/operator-precedence/)

