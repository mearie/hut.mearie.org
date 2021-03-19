---
title: 브레인퍽을 배워 보자
url: /brainfuck/lets-learn/
changes:
- - 2011-09-05
  - 풉;에 브레인퍽 문서의 일부로 첫 작성.
  - 2021-03-08
  - 허튼소리에 해당 부분만 잘라서 올림.
---

## 초기화 및 문자열 출력 {#assignment-and-output}

가장 간단한 예제는 항상 정해진 내용만 출력하는 코드일 것이다.
이는 현재 칸의 값을 `+`와 `-`로 이리 저리 바꿔 가면서 간단하게 만들 수 있다.
예를 들어 `-_-`({{<a ASCII>}} 값 45, 95, 45)을 출력한다면,

```brainfuck
+++++++++++++++++++++++++++++++++++++++++++++.      현재 칸을 45로 만들고 출력
++++++++++++++++++++++++++++++++++++++++++++++++++. 현재 칸에 50을 더해 95로 만들고 출력
--------------------------------------------------. 현재 칸에 50을 빼 45로 만들고 출력
```

이런 느낌이 된다.
그러나 45를 한 번 만들었다면 세번째 문자는 이미 만든 45를 다시 쓰면 되는 게 아닌가 하는 생각이 들 수 있는데,
이 때문에 어떤 칸에 있는 숫자를 다른 칸에 “더하는” 코드가 많이 등장한다.

```brainfuck
+++++++++++++++++++++++++++++++++++++++++++++.       현재 칸을 45로 만들고 출력
[->+>+<<]                                            현재 칸의 값을 다음 두 칸에 더함
                                                     현재 칸의 값은 0이 됨
>++++++++++++++++++++++++++++++++++++++++++++++++++. 왼쪽 칸으로 옮겨 가서 50을 더해
                                                     95로 만들고 출력
>.                                                   그 다음 칸은 그대로 출력
```

여기서 볼 수 있듯 이런 기본적인 더하기 코드는 여러 칸에 동시에 적용할 수 있다.
사실 일반적으로 생각하는 `a = b;` 같은 변수 대입 같은 걸 구현하려면 필수적인데,
다음과 같이 구현해야만 한다.

1. 원래 값이 있던 칸 b, 대입할 칸 a와 임시로 사용할 칸 t를 준비한다.

2. a와 t를 0으로 초기화한다.
   흔히 `[-]`를 많이 쓰며, 이미 0인 걸 알고 있다면 그대로 써도 된다.
   위 코드에서도 메모리는 항상 0으로 초기화된다는 점을 이용하고 있다.

3. a와 t를 b만큼 증가시킨다.
   b는 0이 된다.

4. b를 다시 t만큼 증가시킨다.

`a += b;`와 같은 경우 위에서 `a`를 0으로 초기화하는 과정을 빼면 쉽게 구현할 수 있다.
아무튼 중요한 것은, 적어도 저수준에서는 **칸 내용을 유지하면서 다른 칸에 영향을 주는 것이 불가능하다**는 것이다.
칸 내용을 유지하려면 별도의 과정이 필요하다.

이 코드의 또 다른 변종으로는 상수 곱하기가 있다.
이는 `a += b * k;` 같은 코드를 구현하는 데 쓰며,
한 칸을 비교적 큰 숫자(10 이상)로 초기화할 때도 사용한다.
이를 사용하면 다음과 같은 좀 더 짧은 코드가 가능하다.

```brainfuck
+++++                   현재 칸을 5로 만듦
[-                      아래 코드를 5회 반복:
  >+++++++++              다음 칸에 9를 더함
  >+++++++++++++++++++    그 다음 칸에 19를 더함
<<]                       다시 돌아감
                        위 코드를 실행하고 나면 세 칸이 순서대로 0 45 95로 초기화됨
>.>.<.                  이제 초기화된 값을 그대로 출력함
```

루프를 중첩하면 좀 더 짧은 코드가 가능하다.
이 경우 안쪽의 9 및 19를 더하는 과정을 좀 더 단순화하여 구현한 것이다.

```brainfuck
+++++         현재 칸을 5로 만듦
[-            아래 코드를 5회 반복:
  >+++++++++    다음 칸에 9를 더함
  [->+>++<<]    그 다음 및 다음 다음 칸에 순서대로 1과 2를 더함
                위 루프를 실행하고 나면 해당 칸에 각각 9와 18이 더해짐
  >>+           18을 더했던 칸에 1을 더 더함
<<<]            다시 돌아감
              위 코드를 실행하고 나면 네 칸이 순서대로 0 0 45 95로 초기화됨
>>.>.<.       이제 초기화된 값을 그대로 출력함
```

이 과정은 워낙 손으로 짜기 귀찮기 때문에,
난해한 프로그래밍 언어 위키에는 아예 어떤 값을 현재 칸에 초기화하는 [가장 짧은 코드](https://esolangs.org/wiki/Brainfuck_constants)를 모아 놓은 페이지도 있다.
하지만 {{<a code-golf 코드골프>}}를 한다면 어차피 별 쓸모가 없다.

## 조건문 {#conditional}

브레인퍽에는 **루프문 딱 하나**만 존재하기 때문에 조건문은 루프문을 가지고 잘 뒤틀어서 만들어야 한다.
여기서는 정확히 입력 한 줄을 입력받아서 그 안에 들어 있는 숫자(ASCII 값 48부터 57까지)만 다시 출력하는 프로그램을 예제로 하겠다.

우선 현재 칸의 값이 0부터 9 사이의 값 안에 들어 있는지 확인하는 코드를 생각해 보자.
아이디어는 10중 루프를 써서, 0부터 9 사이의 값 안에 들어 있지 않으면 특정한 칸의 값을 1로 바꾸는 것이다.
(루프문은 항상 현재 칸이 0이 아닐 때만 돌아가기 때문에, 특정 조건이 **아닐** 경우 코드를 실행하는 게 보통 더 쉽다.)
루프는 딱 한 번만 실행되어야 하므로 확인 뒤에는 현재 칸을 항상 0으로 바꿔야 하는 걸 잊으면 안 된다.

```brainfuck
[              현재 칸이 0이 아니면:
 -              현재 칸에서 1을 뺌
 [              현재 칸이 0이 아니면 (즉 본래는 1이 아니었다면):
  -[             현재 칸이 2가 아니었다면:
   -[             현재 칸이 3이 아니었다면:
    -[             현재 칸이 4가 아니었다면:
     -[             현재 칸이 5가 아니었다면:
      -[             현재 칸이 6이 아니었다면:
       -[             현재 칸이 7이 아니었다면:
        -[             현재 칸이 8이 아니었다면:
         -[             현재 칸이 9가 아니었다면:
          >+<            다음 칸에 1을 더함
          [-]            현재 칸을 0으로 초기화
]]]]]]]]]]     루프 끝
               현재 칸은 0으로 초기화되고 다음 칸은 0 또는 1이 됨
               만약 현재 칸이 0부터 9 사이의 값이었을 경우
               해당하는 루프는 실행되지 않은 채로 현재 칸이 0으로 남으므로
               바깥에 있는 루프는 별다른 처리 없이 그대로 종료됨
```

이 코드는 (당연하게도) 현재 칸의 값을 지워버리기 때문에,
실제로는 복사 명령과 함께 사용해야 원하는 결과를 얻을 수 있다. 다음 코드는 전체 프로그램이다.

```brainfuck
+                       현재 칸을 1로 초기화 (다음 입력 때 바로 지워짐)
[                       현재 칸이 0이 아니면:
  ,                       현재 칸을 입력받은 문자로 초기화
  [->+>+<<]               다음 두 칸에 입력받았던 문자를 복사
                          이 중 첫 칸은 출력용으로 다른 칸은 비교용으로 쓰임
  >>                      비교용 칸으로 옮겨감
  ----------              현재 칸에서 10을 뺌
  [                       만약 현재 칸이 0이 아니면 (즉 개행문자가 아니었다면):
    -->++++++               현재 칸에서 2를 빼고 다음 칸을 6으로 초기화
    [-<------>]             현재 칸을 6만큼 6번 뺌
                            루프가 끝나면 현재 칸은 36만큼 줄어들고 다음 칸은 0이 됨
                            앞에서 뺐던 것을 모두 합하면 48만큼 줄어든 상태
    +<                      다음 칸을 1로 초기화
    [-[-[-[-[-[-[-[-[-[     현재 칸이 0부터 9 바깥에 있으면 (즉 원래 48부터 57 바깥이면):
      >-<[-]                  다음 칸에서 1을 빼서 0으로 만들고 루프 종료
    ]]]]]]]]]]
    >[                      다음 칸이 1이었다면 (즉 원래 48부터 57 사이였다면):
      <<.                     출력용 칸으로 옮겨 가서 출력
    >>-]                      본래 칸은 다시 0으로 초기화해서 루프 종료
    <<[-]                   이제 0이 아닌 칸은 출력용 칸만 남아 있으므로 0으로 재설정
    <+                      최초에 시작했던 칸을 1로 설정해서 다음 문자를 입력받도록 함
  >>]                       비교용 칸에서 시작했으므로 비교용 칸으로 다시 돌아감
  <<                      처음 시작했던 칸으로 다시 돌아감
                          입력을 계속 받아야 한다면 1 아니면 0으로 설정되어 있음
]
++++++++++.             현재 칸은 0이므로 10으로 설정한 뒤 개행문자 출력
```

이 코드에서 하나 주의깊게 볼 점은,
루프 안에 들어 있는 모든 코드는 시작할 때와 끝날 때 테이프가 가리키는 현재 칸이 항상 같다는 것이다.
이렇게 하지 않으면 루프가 몇 번 실행되었느냐에 따라서 같은 코드라도 완전히 다른 동작을 할 수 있다.
특수한 목적이 없는 한 대부분의 루프는 이런 **균형 잡힌 루프**(balanced loop)에 속하게 된다.

또 하나, 위 코드에서는 개행문자는 항상 ASCII 값 10이라고 가정하고 있으며 {{<a end-of-file EOF>}}는 별도로 처리하고 있지 않지만,
완전하게 짜려면 이 둘에 대해서 좀 심사숙고를 할 필요가 있다.
보통 문제가 되는 건 후자로,
구현에 따라서 0으로 설정하거나, -1로 설정하거나, 아예 현재 칸의 값을 바꾸지 않거나 하는 경우가 있기 때문에 어느 쪽을 따를지를 생각해 봐야 한다.
다음 코드는 이 중 0으로 설정하는 경우와 칸을 변경하지 않는 경우에 대해서만 따로 처리한 것이다.

```brainfuck
+[                       개행문자나 EOF가 나올 때까지 반복:
  [-],                     현재 칸을 입력받은 문자로 초기화
                           EOF일 때 칸을 변경하지 않는 구현에서는 0으로 초기화
  [->+>+<<]                다음 두 칸에 입력받았던 문자를 복사
  >>                       비교용 칸으로 옮겨감
  [                        만약 현재 칸이 0이 아니면 (EOF):
    ----------               현재 칸에서 10을 뺌
    [                        만약 현재 칸이 0이 아니면 (즉 개행문자가 아니었다면):
      (중간 생략)
    ]
  ]
  <<                       처음 시작했던 칸으로 다시 돌아감
]
++++++++++.              현재 칸은 0이므로 10으로 설정한 뒤 개행문자 출력
```

## 배열 {#array}

지금까지는 테이프 상의 모든 칸이 하는 역할이 항상 정해져 있었지만,
임의 크기의 자료를 다뤄야 할 경우 한 칸이 상황에 따라서 다른 역할을 하도록 만들어야 할 수도 있다.
이 때문에 제대로 된 브레인퍽 코드를 짜려면 배열을 어떻게 짜는가를 알 필요가 있다.

배열을 써야만 하는 대표적인 예로 문자열을 뒤집어 출력하는 프로그램이 있겠다.
편의를 위해 EOF일 때는 항상 0이 입력된다고 가정하고,
따라서 문자열에는 ASCII 값 0인 문자가 없다고 가정을 하겠다.

```brainfuck
>                       배열의 시작점으로 쓰기 위해 첫 칸을 비움
+[                      EOF가 나올 때까지 반복:
  -                       현재 칸을 0으로 재설정
  >[-],                   현재 칸의 다음 칸을 입력받은 문자로 초기화
  [-<+>>+<]               현재 칸과 현재 칸 다음 다음 칸에 복사
                          전자는 저장용으로 후자는 루프 종료를 확인하는 데 사용
  >[                      입력받았던 문자가 0이 아니라면 (EOF):
    <+                      다음 칸(이미 0으로 재설정되었음)을 1로 설정
    >[-]                    루프 종료를 위해 다음 다음 칸은 0으로 재설정
  ]
  <                       저장용 칸을 넘어서 다음 칸으로 옮겨감
                          이제 현재 칸에는 앞 칸이 0이었다면 0 아니면 1이 들어 있음
]
<<                      저장된 문자열의 마지막 문자로 이동
[                       배열의 시작(0으로 구분)이 나올 때까지 반복:
  .<                      현재 칸을 출력하고 앞 칸으로 이동
]
```

여기서 중요한 점은, 배열은 **0으로 가로막힌 연속된 칸들**로 표현된다는 점이다.
이 때문에 배열의 시작점에서 끝점으로 이동하는 `>[>]` 같은 코드나 반대 역할의 `<[<]` 같은 코드가 필수적으로 필요하게 된다.
(배열의 배열이라면 `>[>[>]>]` 따위 코드도 볼 수 있다.)
따라서 위 코드를 조금 바꿔서,
본래 입력되었던 문자열을 한 번 뒤집어 출력하고 다시 원래대로 출력하는 코드는 쉽게 구현할 수 있다.

```brainfuck
>+[->[-],[-<+>>+<]>[<+>[-]]<]  설명 생략
<<[.<]                         배열 뒤에서 앞으로 훑으며 한 글자씩 출력
>[.>]                          배열 앞에서 뒤로 훑으며 한 글자씩 출력
```

만약 배열의 각 원소가 한 칸이 아니라 여러 칸으로 표현된다거나,
한 칸이라 하더라도 0이 들어 갈 수 있는 경우 표현을 바꿔야 할 필요가 있다.
이 경우 어떤 식으로든 배열의 끝을 구분할 방법이 있어야 하는데,
배열의 끝일 때 0이고 아닐 때는 1인 칸을 항상 둔다거나 하는 방법으로 해결할 수 있다.
다음 코드는 위 코드를 이런 식으로 한 원소에 두 칸을 쓰도록 고친 것이다.
(실질적으로는 별 쓸모는 없다.)

```brainfuck
>>                          배열의 시작점으로 쓰기 위해 첫 두 칸을 비움
+[                          EOF가 나올 때까지 반복:
  ->[-],[-<+>>+<]>[<+>[-]]    설명 생략
  <[->+>+<<]                  다음 칸을 그 뒤의 두 칸으로 옮김
  >>[-<<+>>]                  뒷쪽 칸을 다시 본래의 다음 칸으로 옮김
  <                           이제 연속된 두 칸 중 현재 원소에 속하지 않는 뒷쪽으로 이동
                              현재 칸(본래의 다음 다음 칸)에는 0 또는 1이 들어 있음
]
<<<                         저장된 문자열의 마지막 문자에 해당하는 칸으로 이동
[                           배열의 시작이 나올 때까지 반복:
  <.<                         해당하는 문자를 출력하고 앞 문자로 이동
]                           루프가 끝나면 현재 위치는 배열의 시작(첫 문자가 아닌)임
>>                          저장된 문자열의 첫 문자에 해당하는 칸으로 이동
[                           배열의 끝이 나올 때까지 반복:
  <.>>>                       해당하는 문자(순서가 반대임)를 출력하고 뒷 문자로 이동
]
```

이 코드의 경우 배열에 “ABC”가 들어 있으면 그 표현은 `0 0 65 1 66 1 67 1 0 0`이 된다.
즉 각 원소의 앞 칸은 본래의 문자, 뒷 칸은 현재 칸이 배열 경계가 아니면 1이고 아니면 0인 것이다.
이 경우 앞으로 훑을 때와 뒤로 훑을 때 코드 길이가 차이가 나게 되는데 어느 쪽을 선택할 지는 적절히 상황을 봐서 선택하면 된다.

{{%note%}}
엄밀히 말하면 맨 처음 0은 어떻게 해도 접근을 안 하므로 무시해도 된다.
여기서는 설명을 간단하게 하기 위해 포함시켰다.
{{%/note%}}

여러 개의 배열을 함께 관리하는 건 간단한 일이 아니며,
짧은 코드를 목표로 한다면 최대한 배열의 수를 줄이는 게 좋다.
그래도 꼭 해야 한다면, 대부분의 경우 선택은 두 가지로 나뉜다.

* 한 배열을 다른 배열 뒤에 놓는다.

  이 경우 잘 하면 두 배열의 경계를 겹쳐 놓는다던가 하는 꼼수를 쓸 수도 있다.
  그러나 만약 배열을 확장하거나 한다면 뒷쪽 배열의 위치도 함께 고쳐야 한다거나 하는 문제가 있을 수도 있어서 순서를 결정하는 게 매우 중요하다.

* 배열을 겹쳐 놓는다.

  이를테면, 두 배열이 하나는 2칸, 다른 하나는 3칸을 차지한다면 이를 한 원소에 5칸을 차지하는 하나의 배열로 처리해서 관리하는 것이다.
  (배열의 경계 또한 한 원소를 차지하기 때문에 두 배열의 길이가 항상 같아야 할 필요는 없다.)
  만약 배열의 각 원소들이 충분히 단순하다면 이 쪽이 더 간편할 수 있다.

거의 모든 브레인퍽 프로그램은 지금까지 설명했던 요소들을 바탕으로 설명할 수 있다.
물론 복잡한 알고리즘의 경우 코드를 줄이기 위해서 조건문과 배열의 경계를 흐뜨러 놓는다거나 하는 꼼수를 많이 쓰기는 하지만 개념은 크게 다르지 않다.

{{%note%}}
예를 들자면 [몫과 나머지를 함께 계산하는 코드](https://esolangs.org/wiki/Brainfuck_algorithms#Divmod_algorithm) `[->-[>+>>]>[+[-<+>]>+>>]<<<<<]`의 안쪽 루프 두 개는 균형이 안 잡혀 있지만,
실제로는 배열 연산이 아니라 두 개의 서로 반대되는 조건의 조건문을 합쳐 놓은 것이다.
그러니 어느 경우든 항상 루프 끝에서는 일정한 칸에 도달하게 된다.
{{%/note%}}

## 주석 {#comments}

지금까지 설명한 코드에서는 일부러 설명에 브레인퍽 명령으로 오인될 수 있는 문자를 사용하지 않으면서 설명을 넣었다.
하지만 좀 더 긴 코드라면 이런 식으로 설명하는 게 매우 귀찮을 수 있는데,
이 때문에 의도적으로 안의 코드를 전혀 실행하지 않도록 하는 방법이 몇 가지 널리 쓰인다.

* 가장 쉬운 방법은 프로그램 바로 맨 앞에다가 루프를 넣는 것이다.
  이 경우 메모리의 초기값이 0일테니 실행이 전혀 될 리가 없다.
  프로그램 전체 설명을 넣을 때 흔히 쓰인다.

* 만약 코드 중간에 설명을 넣어야 할 경우,
  루프가 끝나고 나서 **바로 다음에** 또 다른 루프를 넣어서 뒷쪽 루프를 실행되지 않도록 할 수 있다.
  이는 루프가 끝나면 현재 칸의 값이 항상 0이라는 점을 이용한 것이다.

* 이도 저도 안 되겠다면 현재 칸이 0인 게 확실한 위치에다가 루프를 넣을 수 있다.
  하지만 별로 추천하고 싶지는 않다.

어느 쪽이든 대괄호가 서로 짝이 맞아야만 브레인퍽 코드로 인식된다는 점을 잊지 말자.
