---
title: 재귀
categories: computing
changes:
- - 2021-04-05
  - 첫 작성.
---

[재귀](https://en.wikipedia.org/wiki/Recursion)(recursion)는 통상적으로는 자가 참조(self-reference)를 가리킨다.
예를 들어서 이 문단에는 한글만 세어서 일흔 두개의 글자와 스물 네개의 낱말이 있다.
(이러한 식의 자가 참조를 {{<a autogram 오토그램>}}이라고 부른다.)

**그러나 재귀와 자가 참조는 다르다.**
수학과 {{<a programming 프로그래밍>}}에서 재귀는 귀납적인(inductive) 구조를 이른다.
이를테면 {{<a peano-axiom 페아노 공리>}}에 따라 0을 포함하는 {{<a natural-number 자연수>}}는 다음과 같이 재귀적·귀납적으로 정의할 수 있다.

1. 바탕 단계: 0은 자연수이다.

2. 귀납 단계: n이 자연수이면 S(n)은 자연수이다.

   그러니까 우리가 1, 2, …라고 부르는 것은 S(0), S(S(0)), …을 줄여 부르는 것이다.

3. 유일성: 위 단계들을 유한번 적용해서 얻을 수 있는 것만 자연수이다.
   <!-- 달리 말하면 자연수는 [...]의 최소 부동점least fixed point이다. (... 부분을 설명하기 귀찮아서 여기서 설명하지 않음) -->

만약 이 단계 중 하나라도 빠지거나 잘못 되어 있으면 귀납적인 구조가 될 수 없다.
예를 들어 바탕 단계가 빠져 있을 경우 자연수는 0부터 시작할 수도 있고, 1로부터 시작할 수도 있고, -0.5로부터 시작할 수도 있으며, 아예 하나도 존재하지 않을 수도 있다(유일성 조건이 남아 있다면 이 해석이 선호될 것이다).
귀납적인 구조는 (설령 그 결과가 무한하더라도) 그 의미를 하나로 확정지을 수 있기 때문에 유용한데,
자가 참조만으로는 그런 유용성을 보장할 수 없다.

더 나아가서, Shriram Krishnamurthi는 〈[재귀를 가르치지 않는 방법](https://parentheticallyspeaking.org/articles/how-not-to-teach-recursion/)〉이라는 글에서 재귀를 가르치는 데 흔히 쓰는 예시들이 엉터리라는 것을 지적한다.
그는 유용한 재귀를 크게 문제 자체가 ({{<a "data-structure/tree" 트리>}}와 같이) 자연스럽게 재귀적으로 기술되는 경우와,
그렇지 않은 경우(generative recursion) 둘로 나눌 수 있다는 걸 지적하며,
전자에 해당하는 귀납적인 구조를 먼저 가르치고 후자를 나중에 가르쳐야 한다고 지적한다.
동감하는 바이다.
