---
title: C 프로그래밍 언어
categories: computing
changes:
- - 2011-04-25
  - 풉;에 첫 작성.
- - 2021-04-05
  - 허튼소리로 옮기고 윤문 및 내용 추가.
  - 2021-04-06
  - 마크업 변경 과정에서 한영 병행 표기를 살짝 수정.
---

[C](https://en.wikipedia.org/wiki/C_(programming_language))는 1970년대에 데니스 리치(Dennis Ritchie)가 [유닉스](unix) 운영체제에 쓰려고 만든 [프로그래밍 언어](programming-language)이다.

{{%fig compact-pre%}}

```c
main()
{
    printf("hello, world\n");
}
```

{{%caption%}}
[《C 프로그래밍 언어(*The C Programming Language*)》 1판](https://archive.org/details/TheCProgrammingLanguageFirstEdition/page/n13/mode/1up)<!-- p. 6 -->에 나오는 최초의 “[Hello, world!]()” 프로그램.
{{%/caption%}}
{{%/fig%}}

시초에서 알 수 있듯 C는 본디 [시스템 프로그래밍](system-programming) 용으로 만들어졌지만,
사실상 모든 플랫폼에 [컴파일러](compiler)가 있다는 특성 때문에 [이식성](portability) 높은 소프트웨어를 개발하는 데 더 많이 쓰인다.
그럼에도 불구하고 <strong>이식성 높은 [어셈블리](assembly-language)</strong>라는 농담 아닌 농담이 있을 정도로 현대에 와서는 저수준으로 취급된다.

유닉스의 대성공과 함께 C는 역사상 가장 중요한 프로그래밍 언어 중 하나로 부상하게 되었는데,
이 때문에 C에서 영향을 받은 언어가 수도 없이 많다.
가장 대표적인 예로 [C++](cpp-language), [C#](csharp-language), [자바](java-language), [자바스크립트](javascript)가 여기에 속한다.
C 하면 바로 생각나는 [중괄호](curly-brace)를 사용한 문법은 보편화되어 [중괄호 프로그래밍 언어](programming-language/curly-brace)라는 분류가 생기까지 했다.

## 버전 {#versions}

브라이언 커니핸(Brian **K**ernighan)과 데니스 리치(Dennis **R**itchie)가 쓴 책 《C 프로그래밍 언어(*The C Programming Language*)》는 오랫동안 [K&R](k-and-r) C라 불리며 사실상의 C 표준으로 취급받았다.
이후 [ANSI]() X3.159(통칭 ANSI C) 및 [ISO]()/[IEC]() 9899(통칭 ISO C)로 [표준화](programming-language/standardization)되어 현재에 이른다.
K&R C를 제외하면 표준이 나온 연도에 따라 C 뒤에 [두 자리 연도](gregorian-calendar/two-digit-year)를 붙이는 것이 관례이며,
미래 표준은 보통 C2x와 같이 마지막 자리를 생략한다.

* K&R C (1978년)
* [C89](./c89) = ANSI X3.159-1989 = ISO/IEC 9899:1990
* [C99](./c99) = ISO/IEC 9899:1999
* C11 = ISO/IEC 9899:2011
* C17 = ISO/IEC 9899:2017

## 문제점 {#problems}

C는 2021년 시점에서 나온지 반세기가 된 프로그래밍 언어이다.
그 사이에 프로그래밍 언어 이론이나 컴퓨터는 질적·양적으로 엄청난 성장을 이루었으며,
당시에 당연하다고 생각했던 제약이 현재는 쓸데 없는 것을 넘어서 **프로그래머를 적극적으로 방해하는 수준에 도달했다**.

### 메모리 안전성의 부재 {#memory-unsafety}

C는 [포인터](pointer)에 아무 제약도 없어서 [메모리 안전성](memory-safety)이 부재한 대표적인 언어로 꼽힌다.
C 호환성을 유지하는 C++ 등의 언어에서도 이 문제가 상존하지만,
C++의 일부 기능만 사용할 경우 문제를 피하는 게 비교적 할 만 하다는 점과도 대조된다.

1970년대 당시에는 생각도 할 수 없던 개념이지만,
1988년의 [모리스 웜](morris-worm)을 시작으로 메모리 안전성은 [컴퓨터 보안](computer-security)에서 빼 놓을 수 없는 요소가 되었다.
현대에는 이를 [쓰레기 수거](garbage-collection)(GC)나 (드물게) [정적 분석](static-analysis)으로 해결하는데,
C에도 이를 지원하는 도구들이 많이 있지만 어느 쪽도 일반적으로 쓰이지 않는다.

지금 와서 C 코드를 짜야 한다면 **무조건** 메모리 안전성을 확인해 주는 도구를 사용해야 한다.
이를테면 Zed Shaw의 《[깐깐하게 배우는 C](https://learncodethehardway.org/c/)(*Learn C the Hard Way*)》에서는 초장에 바로 [valgrind]()를 쓰도록 하고 있다.

### 미정의 동작 {#undefined-behaviors}

C의 [미정의 동작](./undefined-behavior)(Undefined Behavior, UB)은 본디 호환성을 위해 일부 언어 동작을 의도적으로 정의하지 않은 것이었다.
이를테면 플랫폼마다 부호 있는 정수가 [오버플로](integer-overflow)되었을 때의 동작이 다르기 때문에,
이식성 높은 프로그램은 애초에 그런 동작을 하지 못하도록 한 것이다.
하지만 어느 플랫폼에서 프로그램이 실행될지 알고 있다면 정의된 동작을 한다고 가정하고 코드를 짤 수 있었던 것이다.
꽤 오랫동안 대부분의 컴파일러들이 이런 코드를 용인해 왔다.

C가 고수준 프로그래밍에서 점점 덜 쓰이게 되며 상대적으로 저수준 성능을 더 요구받게 되면서 이 상황이 꼬여 버렸다.
예를 들어서 [포트란](fortran)은 임의의 포인터를 지원하지 않는다는 점에서 C에 비해 최적화 여지가 더 많다고 평가되었는데,
이를 만회하기 위해 C99에서 한 주소를 서로 다른 타입을 가지는 포인터로 접근하는 것을 UB로 막아 버렸다([strict aliasing](./strict-aliasing)).
이런 추세에 따라 컴파일러들이 UB를 최적화 기회로 여기게 되자,
지금껏 문제 없이 동작하던 코드들이 갑자기 최적화되어 사라지며 오동작하기 시작한 것이다.
더 심각한 것은 그렇게 사라진 코드 중 일부는 [바로 앞선 메모리 안전성을 확인하기 위한 코드](https://my.eng.utah.edu/~cs5785/slides-f10/Dangerous+Optimizations.pdf#page=11)였다는 것이다.

{{<claim>}}
C의 이러한 변화는 두 가지의 서로 상충되는 목표,
즉 충분히 저수준이어서 극한의 성능을 뽑아낼 수 있는 프로그래밍 언어라는 목표와,
이식성 높은 프로그래밍 언어라는 목표 사이의 충돌을 보여 준다.
물론 현재의 C는 두 마리 토끼 중 하나도 제대로 잡지 못한 상태로,
**어느 한 쪽을 포기하지 않는다면 이러한 문제는 계속 반복될 것이다.**

### 대규모 프로그래밍에 부적합 {#unsuitable-at-the-large}

소프트웨어가 대규모화되면서 프로그래밍 언어도 이에 맞춰서 변화할 필요가 생겼다.
공개되는 이름의 목록을 제어하고 이름간의 충돌을 조율하는 [모듈 시스템](module-system)이나,
[라이브러리](software-library)를 지원하기 위한 [패키지 관리자](package-manager) 등이 그러한 요구의 결과이다.

**C에는 그런 기능이 하나도 없다.**
C에서 여러 파일을 연결하기 위한 수단은 여전히 선형적으로 실행되는 [헤더 파일](./header)이며,
하다 못해 `#pragma once`와 같이 널리 쓰이는데다 최적화에도 도움이 되는 기능조차 표준화되지 않은 상태이다.
이 때문에 컴파일러들은 미리 컴파일된 헤더 파일(precompiled header file) 같은 것을 만들어 가며 고생하고 있다.
패키지 관리자 같은 건 꿈도 꾸지 못할 노릇인지라,
[파일 하나짜리 라이브러리](https://github.com/nothings/single_file_libs) 같은 것이 호응을 얻어 널리 쓰이는 상황이다.

### 반동: 엘리트 주의 {#elitism}

이런 상황인데도 C를 굳이 꼭 써야 한다고 주장하는 사람들이 꽤 남아 있다.
이들은 크게 둘로 나눌 수 있으며,
정도의 차이는 있지만 프로그래머 안에서의 엘리트 주의를 반영하기에 대체로 무익하다.

* “저수준 하드웨어를 이해하기 위해” C를 써야 한다고 주장하는 사람들이 있다.
  그러나 C만 알아서는 실제 하드웨어의 [캐시](processor-cache)나 [분기 예측](branch-prediction) 등의 요소를 전혀 이해할 수 없다. 
  상술했듯 C에는 UB가 있으며, UB라는 거름망으로 걸러 낸 기계는 실제 하드웨어가 아니라 표준에만 정의된 이상적인 기계에 불과할 뿐이다.

* “간명한 코드를 작성하기 위해” C를 써야 한다고 주장하는 사람들이 있다.
  [유닉스 철학](unix-philosophy)이나 요즘은 [suckless.org]()로 대표되는 무리의 사람들로,
  현대의 복잡한 소프트웨어를 [발전이 아닌 퇴행으로 보고](software-bloat) 최소한의 코드로 최대한의 결과를 내기 위해 C가 적격이라고 생각하는 것이다.
  이들의 주장은 그 자체로는 설득력이 없진 않지만,
  C와 비견할 만한 언어들이 어느 정도 시장에 나와 있는 현 시점에서까지 C를 강조하는 것은 결국 반동적일 수 밖에 없다.

