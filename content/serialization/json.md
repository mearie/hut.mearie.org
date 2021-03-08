---
title: JSON
url: /json/
categories: computing
changes:
- - 2021-03-01
  - 해커뉴스에 올렸던 글을 바탕으로 초벌 작성.
  - 2021-03-08
  - 미작성이었던 [표준이 왜 이따위인가?](#bad-spec)와 [대안](#alternatives) 섹션을 마저 작성.
---

[JSON](https://en.wikipedia.org/wiki/JSON)은 자바스크립트의 (거의) 부분집합으로 시작해서 현재는 언어 불문하고 아주 널리 쓰이는 {{<a serialization 직렬화>}} 포맷이다.
사실상 모든 프로그래밍 언어에 JSON 라이브러리가 있다고 보아도 좋다.
JSON보다 30년 정도 이전에 나온 [Apple II에서 돌아가는 JSON 라이브러리](https://github.com/ppelleti/json65)도 있을 정도이다.

**JSON은 쓰레기다.**
JSON은 대강 만들어진 표준이 얼마나 심각한 문제를 야기할 수 있는지 보여 주는 대표적인 예제이다.
이걸 만든 사람은 [Douglas Crockford](https://en.wikipedia.org/wiki/Douglas_Crockford)라는 사람인데,
당시 자바스크립트를 빡세게 사용하던 야후에서 근무한 덕분에 자바스크립트의 초기 발달에 상당한 영향을 미쳤다.
허나 [그가 스스로 말하듯](https://www.crockford.com/about.html), 그는 표준화에 관심이 있다기보다는 자바스크립트를 유용한 언어로 만드는 데에 더 관심이 있었고,
[“이 소프트웨어는 악을 행하는 용도로 쓸 수 없다”](https://en.wikipedia.org/wiki/Douglas_Crockford#%22Good,_not_Evil%22) 같은 드립이나 치면서 오픈 소스 소프트웨어 라이선싱을 꼬아 버리는 트롤링도 벌인 적이 있다.
돌이켜 보면 이 사람이 관여한 표준이 괜찮을 거라는 믿음은 허황된 것이었다.

## 효율적이지 않다 {#inefficiency}

JSON은 자바스크립트의 부분집합을 표방했기 때문에 근본적으로 인간이 읽을 수 있는 포맷이다.
허나 일반적으로 직렬화 포맷은 성능을 중시하고 인간이 읽을 수 있는 건 따로 설계하게 마련이다.
(그렇다고 [인간이 쓰기 편한 포맷이냐 하면 그건 또 아니다](#cumbersome-to-write).)
JSON은 상당히 비효율적인 포맷이며 설계에 성능이 전혀 고려되지 않았다.

- 문자열과 배열, 오브젝트의 크기가 아무데도 적혀 있지 않다.
  따라서 데이터의 일부를 읽지 않고 건너 뛰는 것이 불가능하다.
  JSON은 무조건 처음부터 끝까지 순서대로 읽어야만 올바르게 파싱할 수 있다.

- 문자열에 탈출열(escape sequence)이 나올 가능성이 상존한다.
  이 말인즉슨 문자열의 끝을 파악하고 나서도 진짜 문자열을 얻으려면 재처리가 필요하다는 말이다.

- 숫자를 십진법으로만 표현할 수 있다.
  특히 {{<a floating-point 부동소숫점>}} 숫자의 {{<a "floating-point/decimal-conversion" 십진법 변환>}}은 수치해석과 극도의 알고리즘 분석 없이는 성능을 높일 수 없다.
  까놓고 말해서 부동소숫점 십진 변환 알고리즘 자체가 JSON 때문에 발전했다고 봐야 할 정도다.

- [데이터 모델](#data-model)에 이진 바이트열이 누락되어 있어서 추가 인코딩이 필수이다.

이런 비효율성에도 불구하고 JSON이 살아남은 것은 거의 순전히 {{<a XML>}}이라는 훨씬 더 비효율적인 안티테제가 있었고,
덤으로 자바스크립트에서 바로 쓸 수 있다는 외부적 요인 때문이었다고 봐야 할 것이다.

### 반론: 요즘 JSON 구현은 효율적이지 않은가? {#recent-impls}

과거에는 JSON이 비효율적이었으나 요즘의 [RapidJSON](https://rapidjson.org/)이나 [simdjson](https://simdjson.org/) 같은 구현들은 효율적이라는 반론이 있을 수 있다.
이는 일정 부분 사실이나, 공정한 비교가 아니기도 하다.

앞선 구현들의 공통점은 JSON의 구조 분석과 트리 생성이 나뉘어 있다는 것이다.
`[1, 2, 3]`을 예로 들면,
구조 분석이란 이게 3개의 숫자가 오프셋 1, 4, 7에 위치한 배열이라는 것을 파악하는 것이며,
트리 생성은 이로부터 내가 사용하는 언어의 배열 타입을 실제로 얻어 내는 것이다.
그리고 **이들 구현이 자랑하는 무지막지한 성능은 구조 분석 성능에 국한된다.**

트리 생성 속도는 전통적으로 파싱 속도가 아니라 메모리 할당 속도에 제한을 받는데,
일개 라이브러리가 이 제한을 피하려면 메모리를 덜 할당하고 일부 처리를 최대한 지연시키는 자체 자료 구조를 만들어야 한다.
simdjson의 예를 들면 [`ondemand`](https://github.com/simdjson/simdjson/blob/master/doc/ondemand.md) 인터페이스가 이런 역할을 한다.
배열 안의 숫자 합을 구하는 식으로 한 번 읽고 버리는 용도라면 자체 자료 구조를 써도 큰 문제는 없겠지만,
JSON 문서의 일부가 아닌 전체를 읽어 들여야 하거나 읽어 들인 값을 변경해야 하는 상황이라면 자체 자료 구조는 큰 도움이 되지 못한다.

요즘 JSON이 빠르다는 주장은 이 점을 이해하지 못해서 나온 “착시”이다.
구조 분석이 필요한 JSON과는 달리, 대부분의 이진 직렬화 포맷은 구조 분석이라는 것 자체가 필요하지 않다.
하지만 JSON은 너무 널리 쓰여서 트리 생성이 분리된 성능 중시 라이브러리를 찾기가 더 쉬운 것 뿐이다.

## 쓰기도 귀찮다 {#cumbersome-to-write}

성능은 뭐 그렇다고 치고, 사람이 읽고 쓸 수 있는 포맷이라는 것 자체에 가치를 두는 사람도 있다.
그러나 JSON은 옛 자바스크립트(ECMAScript 3) 시절의 문법을 고수하고 있는데다,
자바스크립트에는 없던 자체적인 제한도 있어서 별로 인간이 직접 쓸만한 포맷이 되지 못한다.

- 자바스크립트와는 달리 문자열에 무조건 쌍따옴표(`"`)만을 쓸 수 있다.

- 문자열 안에 개행문자가 들어갈 수 없다. 무조건 탈출열을 써야 한다.

- 문자열 안에서 U+FFFF보다 큰 {{<a unicode 유니코드>}} 스칼라 값을 탈출열로 쓰려면 무조건 서로게이트 쌍 형태(U+10000 → `\ud800\udc00`)로 써야 한다.

- 배열과 오브젝트의 마지막 원소 뒤에 콤마(`,`)를 쓸 수 없다.

- 앞서 말했지만 숫자를 십진법이 아닌 다른 진법으로 쓸 수 없다.

- 주석을 아예 쓸 수 없다.
  주석을 쓰려면 오브젝트에 안 쓰는 키를 `"_comment": "주석 내용"` 식으로 쓰라는 어처구니 없는 제안이 팁이랍시고 나돌아다닌 적도 있다.

끔찍한 점은 이에도 불구하고 어쨌든 사람이 읽을 수 있다는 이유로 **{{<a config-format 설정 포맷>}}으로 잘못 쓰이는 경우가 흔하다는 것이다.**
그리고 JSON을 설정 포맷으로 쓸 경우 백이면 백 자체적인 확장으로 위 문제를 해결하는 경우가 많다.
표준화? 당연히 엄청나게 많은 시도가 있었지만 어느 하나 통일된 제안이 나오진 못했다.

## 데이터 모델이 이상하다 {#data-model}

데이터 모델은 직렬화 포맷을 해석해서 나오는 논리적인 자료형이다.
예를 들어 포맷에 “숫자”가 있다면 그 숫자의 허용 범위 같은 것이 자료형의 속성이 될 것이다.
“유니코드 문자열”과 “이진 바이트열”의 구분이 있는지 여부도 데이터 모델의 소관이 되겠다.

{{<hn 24950127>}}
This is true, but due to its origin I think we have a weak agreement over the JSON data model: it is a JavaScript value unless ambiguous. We do expect dotted number literals represent IEEE 754 binary64 numbers (importantly, this is pretty much the only case that excess precision do not alter the value), undotted number literals of the range (-2<sup>31</sup>, 2<sup>31</sup>) (exclusive) represent 32-bit integers, string literals with no lone surrogates and no unescaped U+2028/U+2029 represent Unicode strings, objects with no duplicate keys represent a partial mapping from strings to values.
{{</hn>}}

**JSON의 데이터 모델은 제대로 정의되어 있지 않다.**
고작해야 “모호하지 않으면 자바스크립트와 같은 의미론” 정도의 말을 할 수 있을 뿐이다.
이는 [RFC 8259](https://tools.ietf.org/html/rfc8259) 시점에서도 그다지 발전이 없으며,
구현체들이 이런 값에 아마도 이렇게 반응할 것이다, 정도의 추측만을 늘어 놓고 있는 실정이다.

- 자바스크립트는 ECMAScript 1 이래로 숫자가 IEEE 754 binary64(배정도 실수)라는 보장을 하지만, JSON에는 그런 보장이 없다.
  따라서 JSON 명세만으로는 다음 질문에 전혀 답할 수 없다.

	- `1e400`이라고 쓰면 오류가 나와야 하는가 무한대로 바뀌어야 하는가?

		- `1e-400`이라면?

		- `0e9999999999999999999999`는?

	- `0.1`이라고 썼을 때 이 값이 정확히 3602879701896397/2<sup>55</sup>라는 것이 보장되는가? 아니면 얼마나 오차가 날 수 있는가?

		- 오차가 보장되지 않는다 해도 적어도 `[0.1, 0.1]`이라고 썼을 때 두 값이 같다는 건 보장하겠지?

	- `30000000000000001`은 `30000000000000000`과 다른가? 전자는 binary64로 표현할 수 없으며, 이 때문에 [트위터가 API 포맷을 변경한 전적이 있다](https://developer.twitter.com/en/docs/twitter-ids).

	- 소숫점이 없는 정수에 범위 제한이 있는가?
	  일부 JSON 구현은 소숫점 여부로 정수 타입을 쓸지 실수 타입을 쓸지 결정하는데,
	  정수 타입에 범위 제한이 있으면 실수로는 표현 가능해도 예상치 않은 오류가 발생할 수 있다.

- {{<hn 26234476>}}
  Seriously, I believe the omission of Infinity and NaN from JSON is a huge mistake, if not Crockford's bad joke. It is commonly said that JSON was originally going to be `eval`ed and Infinity and NaN could have been redefined, but that eval had to be already preceded by filtering anyway so you can put a few lines to ensure that Infinity and NaN are expected values. Or use `Function` instead. Or use `1/0` or `0/0` as pseudo-literals. Not every JS value is present in the JSON data model, but it's absurd that not every JS number is present in it. 
  {{</hn>}}

  한 술 더 떠서, JSON에는 자바스크립트에 멀쩡하게 있는 `+Infinity`, `-Infinity`, `NaN`을 쓸 수 없다.
  보통 [자바스크립트에서 `Infinity`와 `NaN`이 재정의 가능한 것](https://stackoverflow.com/a/1424034/225272)이 이유라고 해석하지만,
  그렇게 따지면 `1/0`이나 `0/0`이라는 고정된 문법으로 대체해도 문제가 없다.

- 자바스크립트 문자열이 그렇듯, 서로게이트 쌍이 홀로 나오는 잘못된 UTF-16 문자열이 허용된다.
  UTF-16이 널리 쓰이던 시절에는 뭐 괜찮았을지도 모르겠지만,
  대세가 서로게이트 쌍 같은 거 없는 UTF-8이 된 현재는 도움이 되지 않는 특성이다.

- 이진 바이트열이 들어 있지 않다!
  그래서 보통은 {{<a base64>}}나 숫자 배열을 사용하는데, 어느 쪽이나 비효율적이기 짝이 없다.

- 한 오브젝트에 같은 키가 중복되는 게 허용된다.
  심지어 중복된 키에 대한 처리는 구현체별 차이가 꽤 커서, 첫번째 키만 남길지, 마지막 키만 남길지, 모두 반환할지, 오류를 낼지 예측할 수조차 없다.

- 물론 오브젝트의 키 순서도 정의되어 있지 않다.

- 자바스크립트와 같이 오브젝트 키가 문자열로 제한되어 있어서,
  임의 타입을 키에 쓸 수 있는 다른 언어에서는 난감한 상황이 꽤 생긴다.

## 표준이 왜 이따위인가? {#bad-spec}

본래 JSON은 Crockford의 [웹사이트](https://www.json.org/)에 대강 정의했던 것을 2006년에 [RFC 4627](https://tools.ietf.org/html/rfc4627)로 “표준화”한 것이 시초이다.
이 표준은 놀랄 정도로 내용이 없는 걸로 유명한데,
가장 강력한 문제는 JSON 파서가 JSON 텍스트(오브젝트나 배열)를 처리해서 내 놓는 출력에 대한 어떤 지침도 없다는 것이다.
이를테면 이 RFC에 따르면 이론적으로 JSON 파서는 올바른 JSON을 인식**하기만 한 뒤** 항상 빈 오브젝트를 반환해도 무방하다.
실제로 그런 구현체가 존재하는가? 아마 아닐 것이다.
그러나 지금껏 설명한 JSON의 모호한 구석에 대해 표준이 아무 부연도 하지 않는 건 상식을 벗어난다.

JSON의 인기에 힘입어 2009년에 발표된 ECMAScript 5에는 [`JSON` 오브젝트](https://262.ecma-international.org/5.1/#sec-15.12)가 추가되었다.
이 API는 Crockford의 [JSON2.js](https://github.com/douglascrockford/JSON-js) 라이브러리에 기반한 것으로,
원래는 문법 정의를 RFC 4627에 맡기고 있었으나 [2008-11-03 초안을 기점으로](https://web.archive.org/web/20081216161234/http://wiki.ecmascript.org:80/doku.php?id=es3.1:es3.1_proposal_working_draft) ECMAScript 표준에도 JSON 문법이 추가되었다.
RFC와 비교하면 이 표준은 상당한 발전이 있었지만, 동시에 JSON에 내재된 문제가 표면화된 첫 표준이기도 하다.

- 올바른 JSON 문자열을 올바른 자바스크립트 값으로 번역하는 멀쩡한 데이터 모델이 명시되었다.
  특히 숫자에 대해서는 기존의 ECMAScript 숫자 변환 알고리즘을 준용하였기에,
  아쉬운 대로 정밀도에 대한 명세와 상호 운용성이 확립되었다.
  <!-- ECMAScript 알고리즘의 엣지 케이스:
  ToNumber는 유효 자리가 20자리를 넘기면 자름,
  ToString은 올바르게 반올림되는 가장 짧은 표현을 선택하나 가장 "가까운" 표현일 필요는 없음.
  추후 flt2dec/dec2flt 관련 글을 쓸 때 옮겨 쓸 것
  -->

- 무한대나 NaN이 `null`로 인코딩된다는 점이 처음으로 명시되었다.

- 오브젝트의 키가 중복될 경우 나중에 나온 것이 우선함을 명시하였다.
  RFC에서는 키가 유일해야 한다고는 쓰여 있지만, 여기에 [{{<sc must>}}](https://tools.ietf.org/html/bcp14#section-1)가 아니라 [{{<sc should>}}](https://tools.ietf.org/html/bcp14#section-3)를 쓰는 바람에 중복 키가 허용되고 말았다.

- **JSON이 자바스크립트의 부분집합이 아니라는 걸 정확하게 기술하였다.**
  U+2028 {{<sc Line Separator>}}와 U+2029 {{<sc Paragraph Separator>}}는 자바스크립트에서는 개행 문자이지만 JSON에서는 아닌데,
  이 때문에 JSON의 문자열 리터럴에는 이들 문자가 탈출열 없이 들어갈 수 있었기 때문이다.

- JSON이 바로 지원하지 않는 값을 인코딩하는 인터페이스(`toJSON` 메소드)가 처음으로 명시되었다.
  당시 [es-discuss 메일링 리스트](https://mail.mozilla.org/pipermail/es-discuss/)를 보면 특히 `Date`를 JSON으로 어떻게 인코딩할 것인가를 두고 엄청난 양의 논의가 있었음을 알 수 있다.

- RFC에서는 최상위에 위치하는 JSON “텍스트”(오브젝트와 배열)와 그 안에 존재하는 JSON “값”의 구분이 있지만,
  ECMAScript에서는 JSON 텍스트와 값이 같다고 명시하여 최상위 단계에 모든 값이 쓰일 수 있다.
  왜 이런 선택을 했는지는 명확하지 않은데, 아마 상술한 `toJSON`과 어느 정도 연관이 있지 않나 싶다.

이러한 이유로 **한동안 JSON의 정의는 두 개가 있었다.**
이후 2013년에 발표된 [ECMA-404](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)는 순전히 RFC 4627을 ECMAScript 명세를 반영하도록 살짝 고친 것에 불과했다.
이런 꼬라지다 보니 RFC의 갱신이 필요했는데,
[Crockford가 표준화 과정에서 나가 떨어져 나간 뒤에 Tim Bray가 그 자리를 이어 받아](https://www.tbray.org/ongoing/When/201x/2014/03/05/RFC7159-JSON),
2014년에 갱신된 [RFC 7159](https://tools.ietf.org/html/rfc7159)가 발표되기에 이른다.
2021년 현재의 최신판 [RFC 8259](https://tools.ietf.org/html/rfc8259)은 몇 가지 사소한 수정을 빼고는 7159와 같다.

RFC 7159는 Crockford의 RFC보다는 상태가 많이 낫지만 여전히 이런 저런 문제가 남아 있다.

* 상호 운용성 문제가 있는 JSON을 금지하는 대신에 쓰지 말라고 권고하는 수준에 그친다.
  대신 JSON의 부분집합으로서 [I-JSON](https://tools.ietf.org/html/rfc7493)을 새로 정의하여 이것을 쓰도록 하였는데,
  I-JSON이 실제로 얼마나 큰 영향을 미쳤는지는 미지수이다.

* 여전히 자바스크립트의 부분집합이 아니었다.
  그런데 결국 보다 못한 ECMAScript 표준화 그룹(TC39) 쪽에서 나서서 **[ECMAScript 명세를 바꿔 버렸기 때문에](https://github.com/tc39/proposal-json-superset)**,
  ES2019가 나온 이후로는 JSON은 자바스크립트의 부분집합이 맞다.
  뭔가 앞뒤가 뒤바뀐 느낌이 들지만.

* [JWT](https://jwt.io/)와 같이 JSON을 보안 프로토콜의 페이로드로 쓰는 경우가 늘어서 {{<a "serialization/canonicalization" 정규화>}}가 필수가 되었는데,
  여전히 정규화에 대한 내용이 하나도 없다.
  그리하여 JSON 정규화는 여전히 필요한 사람마다 만들어 쓰는 실정이다.
  가장 최근의 표준화 시도로는 [RFC 8785](https://tools.ietf.org/html/rfc8785)가 있는데, 두고 볼 일이다.

## 대안 {#alternatives}

미안한 얘기지만, **현대 프로그래밍에서 JSON을 완전히 피할 방법은 없다.**
JSON의 문제와는 무관하게 JSON은 널리 쓰이는 포맷이기 때문에 좋든 싫든 써야 할 때가 올 것이다.
뭐 다음 상황을 만족한다면 JSON을 쓴다고 해도 큰 문제는 일어나지 않을 것이다.

* 데이터 크기가 고만고만하고 아주 자주 디코딩하지 않음.
* 모든 숫자가 절대값이 2<sup>31</sup>보다 작은 정수라는 걸 보장할 수 있음.
  그보다 크거나 소숫점이 있는 경우 문자열을 대신 쓸 수 있다.
* 보안 프로토콜의 일부로 사용하지 않음.
* 최대한 많은 환경을 지원해야 함.

물론, 만약 피할 수 있다고 판단되면 JSON을 피하는 것이 적절한 선택일 것이다.
{{<a "serialization/formats" 직렬화 포맷>}} 문서를 참고하자.

