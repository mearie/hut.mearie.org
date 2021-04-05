---
title: XML
categories: computing
changes:
- - 2011-09-26
  - 풉;에 첫 작성.
- - 2021-03-20
  - 현재 시각을 반영해 전체적으로 재구성. 가장 크게 쳐 낸 부분은 XML 문법을 상세 설명하는 “구조” 섹션이었다.
---

[XML](https://en.wikipedia.org/wiki/XML)(eXtensible Markup Language)은 [W3C]()에서 표준화한 [마크업 언어](markup-language)와 그에 연관된 기술이다.

{{%fig compact-pre%}}

```xml
<?xml version="1.0"?>
<book xmlns:html="http://www.w3.org/1999/xhtml">
  <title xml:lang="ko">그래도 마을은 돌아간다 <part volume="1">1권</part></title>
  <title xml:lang="ja">それでも町は廻っている<part volume="1">１</part></title>
  <isbn>978-89-532-7892-9</isbn>
  <date>2015-12-17</date>
  <publisher>
    <name>서울문화사</name><!-- 현 서울미디어코믹스 -->
    <url>https://www.ismg.co.kr/</url>
  </publisher>
  <author class="글 그림">
    <name>이시구로 마사카즈</name>
  </author>
  <notes>
    <html:div>메이드 카페는 나오지 않습니다.</html:div>
  </notes>
</book>
```

{{%caption%}}
책 정보를 나타내는 XML의 예제.
반구조화된 데이터(제목), 주석, [언어 태깅](language-tag)(`xml:lang`), XML 네임스페이스를 사용한 확장 등이 보인다.
{{%/caption%}}
{{%/fig%}}

본디 [ISO]()가 표준화한 [SGML]()(ISO 8879:1986)을 기반으로 해서 겉보기 문법은 거의 같지만,
SGML이 허용하는 상당한 유연성을 포기하는 대신 인터넷에서 쓰기 좋도록 크게 단순화한 것이다.
[XML 1.0](https://www.w3.org/TR/REC-xml/)이 가장 널리 쓰이며,
사소한 부분이 바뀐 [XML 1.1](https://www.w3.org/TR/xml11/)이 최신이지만 별로 쓰이지 않는다.

XML은 메타 포맷으로, 그 자체만으로 쓸 수도 있지만 사용하는 측에서 그 구조를 지정하는 게 보통이다.
이를테면 `<book>`이나 `<isbn>`과 같은 태그는 XML에 정의되어 있지 않으며,
XML은 오로지, 이를테면 태그가 제대로 여닫혔는지 여부와 같이,
문법이 올바른지(well-formed) 확인하는 역할만을 담당한다.
XML로 표현된 데이터의 구조를 좀 더 섬세하게 정의하는 것도 가능하지만,
궁극적으로 문법을 넘어서는 문제는 사용하는 쪽에서 검증해야 한다.

태그와 태그 사이에 자유로운 형식의 텍스트가 들어갈 수 있다는 점 때문에 XML은 반구조화(semi-structured)된 데이터에 가장 적합한 언어이다.
이는 구조화되어 있지 않은 데이터에 드문드문 표식(annotation)을 넣어 구조적인 요소를 추가한 것으로,
데이터가 정해진 구조에 맞지 않으면 오류가 나는 [데이터베이스](database) 시스템이나,
구조가 아예 존재하지 않는 [플레인 텍스트](plain-text) 포맷과 차이를 보인다.
그래서 이론적으로는 처리하는 쪽에서 예상하지 않은 구조에 좀 더 유연하게 대처할 수 있다.
이것이 XML의 이름에 들어 있는 “[확장성](extensibility)”의 본질이다.

## 쓸모 {#applications}

일단은 표준화된 [공개](open-standard) [텍스트 포맷](text-format)이며,
자기가 원하는 구조를 집어 넣을 수 있다는 장점이 있어서 XML을 기반으로 한 형식들이 많이 있다.

* [RSS](rss-format)와 [Atom](atom-format)은 피드 포맷이다.
* [XHTML]()은 [HTML]()의 XML 표현이다.
* [SVG]()는 2차원 [벡터](vector-graphics) [이미지 포맷](image-format)이다.
* [MathML]()은 수식을 표현하는 마크업 언어이다.
* [DocBook]()은 기술 문서를 위한 마크업 언어이다.
* [OpenDocument]()(ODF)와 [오피스 오픈 XML](ooxml)(OOXML)은 각각 [OpenOffice.org]()와 [마이크로소프트 오피스](microsoft-office)에서 유래한 표준화된 사무용 파일 포맷이다.

이 중 많은 수가 XML로 표현하면 엄청나게 큰 파일이 되는 경우가 많아서,
[ZIP](zip)이나 [gzip]()과 같은 [압축 포맷](compression/format)으로 저장할 수 있게 하는 경우가 잦다.
위 예제 중에서 SVG의 경우 gzip으로 압축한 경우 본래의 `.svg` 대신 `.svgz`라는 [확장자](file-extension)를 쓰게 되어 있고,
ODF와 OOXML은 ZIP 파일로 저장한 것을 확장자만 바꾼 것이다.

## 무쓸모 {#uselessness}

> XML은 데이터를 표현하는 외부 포맷으로 홍보되어 왔다. 이건 어려운 문제가 아니다. [생략] 그래서 XML의 본질은 이러한데, **푸는 문제는 어렵지 않음에도 그 문제를 잘 풀지 못한다는 것이다.**
>
> <details lang=en><summary>원문</summary> XML is touted as an external format for representing data. This is not a hard problem. [snip] So the essence of XML is this: <strong>the problem it solves is not hard, and it does not solve the problem well.</strong></details>
>
> —<cite>Jérôme Siméon and Philip Wadler, [The essence of XML](https://doi.org/10.1145/640128.604132)</cite> (강조는 원문에 없음)

{{<claim>}}
**XML은 보통 쓰레기도 아니고 핵쓰레기다.**
앞서 XML이 반구조화된 데이터에 가장 적합하다 했는데,
다르게 말하면 데이터가 비구조화(예: 바이트열)되었거나 완전 구조화(예: 데이터베이스)되어 있는 경우에는 훨씬 좋은 대안이 많으며,
특히 XML은 이러한 데이터에 대해선 궤멸적으로 불리하다.
그런데 이런 마크업 언어가 어쩌다가...

* [스키마 정의](https://www.w3.org/XML/Schema)에도 쓰이고
* [프로그래밍 언어라고 부를 수 없는 쓰레기](https://www.w3.org/TR/xslt/)에도 쓰이고
* [이름에 simple이 들어 있는데 명세는 40쪽이고 아무 짝에도 쓸모가 없는 프레이밍 표준](https://www.w3.org/TR/soap12/)에도 쓰이고
* [시맨틱 웹](semantic-web)이라는 [뜬구름 잡는 헛소리](https://www.w3.org/RDF/)의 근간이 되어 있다거나
* [완결되지 않은 XML 문서를 가지고 스트리밍을 하는 인스턴트 메시징 프로토콜](https://xmpp.org/rfcs/rfc6120.html#streams)도 생기게 되었는가?

XML 기술의 [비대화](software-bloat)에는 크게 두 가지 원인이 있다.
앞서 언급했듯 W3C는 시맨틱 웹에 상당히 크게 베팅을 하고 있었는데,
이 때문에 XML을 **미래엔 아무도 거들떠보지 않을** 시맨틱 웹 생태계의 기반이 되게 하려고 전혀 쓸모 없는 기술들을 양산해 냈다.
더불어 XML의 이름에도 확장성이 들어 있으니만큼,
XML을 넣으면 자동으로 확장하기 좋은 소프트웨어가 될 거라고 믿는 자들이 믿을 수 없을 정도로 많았다.

현재에 이르러 그 결과는 처참하다.
W3C는 [XHTML2]()를 기점으로 웹 기술에 대한 영향력을 상실했으며,
(뭐 항상 그래 오긴 했지만) 브라우저 벤더들의 결정을 확인하는 거수기에 불과한 존재로 전락했다.
[SVG]()와 같이 XML에 기반한 포맷이나 표준이 여전히 쓰이는 경우가 남아 있지만,
**널리** 쓰이는 새 표준이 XML에 기반하는 경우는 이제 정말 보기 힘들다.
이 모양이다 보니 보통이라면 쓰레기라서 채택될 이유가 없는 [JSON]()이 오로지 XML보다 훨씬 간단하다는 이유로 널리 쓰이게 되어 버렸(으며 XML의 문제점도 일부 이어 받고 말았)다.
XML의 유일한 장점이라면 표준 명세만은 멀쩡하게 쓰여 있다는 것이다.

