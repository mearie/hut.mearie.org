---
title: 자료형
categories: computing
math: true
changes:
- - 2021-03-21
  - 첫 작성.
---

{{<a programming 프로그래밍>}}에서 [자료형](https://en.wikipedia.org/wiki/Data_type)(data type) 내지 타입(type)은 특정 데이터가 어떻게 해석되고 다뤄져야 하는지를 가리키는 메타데이터(데이터에 대한 데이터)이다.
이는 다음 두 가지 각도로 해석할 수 있다.

1. 메모리 상의 {{<a bit 비트>}}열을 해석하는 방법. 이를테면 `abcdefgh`를 정수 `$2^7 a + 2^6 b + \cdots + h$`로 해석할 수 있다.
2. 허용되는 값의 {{<a set 집합>}}. 이를테면 어떤 값이 `$\{0, 1, \cdots, 2^8 - 1\}$` 중 하나만 허용된다 할 수 있다.

전자는 물리적인 실체에서 거슬러 올라가는 관점이고 후자는 추상적인 수학 개념으로부터 내려가는 관점인데,
어느 한 해석이 맞다기보다는 서로 호환성이 있어서 선택의 여지가 있다.
한국어에서 보통 “자료형”이라고 하면 전자에 더 가깝고, “타입”은 후자에 더 가까운 편이다.
특히 타입을 중심으로 일련의 검증 규칙을 만든 것을 **{{<a type-system 타입 체계>}}**라 한다.
