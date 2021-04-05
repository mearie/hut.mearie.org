---
title: 윤초
categories: world
changes:
- - 2021-03-11
  - 초벌 작성.
- - 2021-03-12
  - |
    [윤시간대](#leap-hour-time-zone)에 대한 주장을 추가.
---

[협정 세계시](utc)(UTC)에서 윤초(leap second)는 평균 태양시와의 오차를 보정하기 위해 가끔 추가되거나 삭제되는 [초](second)를 가리킨다.

하루의 길이(LOD, length of day)는 정확히 86 400초가 아니고,
지구 자전 속도의 변화에 따라 바뀔 수 있어서 일정하지 않다.
이 때문에 누적된 오차를 청산하기 위해,
지정된 달의 마지막 날 23:59:59(UTC 기준)를 삭제하여 하루를 86 399초로 만들거나(**음의 윤초**),
23:59:60을 추가하여 하루를 84 001초로 만드는 것이다(**양의 윤초**).
UTC가 시작된 1972년 이래 윤초는 2021년 6월까지 27회 추가되었으며 삭제된 적은 없다.

{{%fig%}}
{{<caption>}}윤초가 추가된 달 목록. ★은 2021년으로, 이후 윤초 일정은 모두 미정이다.{{</caption>}}

| 연도 | 윤초 | | | | | | | | | |
|------|------|-|-|-|-|-|-|-|-|-|
| **1970년대** | | | 1972-06<br>1972-12 | 1973-12 | 1974-12 | 1975-12 | 1976-12 | 1977-12 | 1978-12 | 1979-12 |
| **1980년대** | | 1981-06 | 1982-06 | 1983-06 | | 1985-06 | | 1987-12 | | 1989-12 |
| **1990년대** | 1990-12 | | 1992-06 | 1993-06 | 1994-06 | 1995-12 | | 1997-06 | 1998-12 | |
| **2000년대** | | | | | | 2005-12 | | | 2008-12 | |
| **2010년대** | | | 2012-06 | | | 2015-06 | 2016-12 | | | |
| **2020년대** | | ★ | | | | | | | | |
{{%/fig%}}

## 결정 {#determining}

협정 세계시(UTC)는 지구 자전에 따른 시각 표준인 세계시(universal time), 구체적으로는 UT1을 근사한다.
두 시각 표준의 차이는 DUT1 = UT1 - UTC로 정의되는데,
DUT1 자체의 오차는 ±0.1초 이하여야 하며 DUT1 자체는 오차까지 고려해도 ±0.9초 이하여야 한다고 [ITU 권고 TF.460-6](https://www.itu.int/rec/R-REC-TF.460/en)에 [명시](https://www.itu.int/dms_pubrec/itu-r/rec/tf/R-REC-TF.460-6-200202-I!!PDF-E.pdf#page=3)되어 있다.
가까운 미래에 DUT1이 이 범위를 넘어설 가능성이 있다고 판단되면,
6월과 12월 말을 우선으로, 3월과 9월 말을 차선으로, 그리고 나머지 달을 마지막에 고려하여 최소 8주 전에 윤초의 삽입·삭제를 결정한다.

윤초를 결정하는 주체는 UTC를 비롯한 시각 표준을 관리하는 [국제 지구 자전국](https://www.iers.org/)(IERS, International Earth and Reference Systems Service)이다.
강제 사항은 아니지만 IERS는 DUT1이 +0.6초 이상이거나 -0.6초 이하일 때 윤초를 추가하며,
[Bulletin C](https://datacenter.iers.org/data/latestVersion/16_BULLETIN_C16.txt) 문서에 6개월마다 다음 6개월간의 윤초 추가 계획을 게시한다.
따라서 일반적으로 **6개월 이후의 윤초는 예측할 수 없다.**

일단 윤초가 결정되면 세계 언론에 전파되며,
[tz 데이터베이스](tz-database) 같은 곳이나 소프트웨어 업데이트 등을 통해 실제로 적용된다.
시보국이나 [GPS]() 신호 등에서도 미래의 윤초 정보를 미리 전달하기 때문에,
이론적으로 전파 수신 환경이 갖춰져 있으면 윤초에 대응할 수는 있다. **어떻게든.**

## 난점 {#difficulty}

**윤초는 컴퓨터 상의 시각 구현을 아주 많이 복잡하게 만든다.**
사실상 모든 곳이 컴퓨터에 의존하는 현대 사회에서 이건 소프트웨어를 올바르게 만들면 된다는 반박으로 넘길 수 있는 문제가 아니다.

{{%todo%}}

### 반박 {#opposition-to-abolishment}

음의 윤초가 지금껏 한 번도 없었다는 점에서 알 수 있듯,
장기적으로 지구의 자전은 달의 조석력으로 인해 느려지고, 하루의 길이는 계속 늘어날 것이다.
본디 현재의 “초” 단위 자체가 특정 시점의 지구의 자전과 공전 속도로부터 산출된 것으로,
상당히 오랫동안 하루의 길이는 86 400초를 웃돌았다.
예를 들어 [2000년에 하루의 평균 길이는 86 400초보다 0.716±0.028밀리초 길었고](https://hpiers.obspm.fr/eoppc/eop/eopc04/eopc04.62-now),
이에 따라 DUT1은 262±10밀리초만큼 증가했다.
2020년대 들어 하루의 길이가 다시 짧아지는 현상이 관측되고는 있지만 일시적인 것으로 추정된다.

<!--
const req = await fetch('https://hpiers.obspm.fr/eoppc/eop/eopc04/eopc04.62-now');
const body = await req.text();
let loy = 0, loyerr = 0, ndays = 0;
for (const line of body.split(/\n/).filter(s => s.startsWith('2000'))) {
    const [year, mon, day, mjd, x, y, dut1, lod, dpsi, deps, xerr, yerr, dut1err, loderr, dpsierr, depserr] = line.split(/\s+/);
    loy += parseFloat(lod);
    loyerr += parseFloat(loderr);
    ++ndays;
}
console.log(`LOY delta: ${loy * 1000}ms +/- ${loyerr * 1000}ms`);
console.log(`LOD delta: ${loy / ndays * 1000}ms +/- ${loyerr / ndays * 1000}ms`);
-->

하루의 길이가 계속 늘어난다는 것은, 윤초가 없을 때의 오차는 시간이 지날수록 **제곱**으로 늘어난다는 얘기다.
이 때문에 캘리포니아 대학교 [릭 천문대](http://mthamilton.ucolick.org/)의 스티브 앨런(Steve Allen) 같은 사람은 [윤초를 폐지해서는 안된다](https://www.ucolick.org/~sla/leapsecs/)고 강력하게 주장한다.
요컨대 **초의 정의를 바꾸거나, 평균 태양시의 의미를 축소시키지 않는 한 윤초나 그와 유사한 메커니즘은 피할 수 없다**는 것이다.
많은 국가에서 정오 즈음에 태양이 가장 높이 뜬다는 것을 당연시 여기고 있는데,
윤초가 없으면 [500년 안에](https://www.ucolick.org/~sla/leapsecs/dutc.html) 이 가정이 눈에 띄일 정도로 틀어지게 된다.
500년까지 기다리지 않아도 3~50년 안에 평균 태양시와 UTC가 1분 정도 차이가 나게 되는데,
이 경우 분 단위로 기록되는 사법 문서의 신빙성에 문제가 생길 수 있다는 지적도 있다.

### 대안: 윤시간대 {#leap-hour-time-zone}

윤초에 대한 찬반 논쟁을 보면 윤초 존치 및 윤치 폐지라는 두 가지 선택지만 있으며,
둘 다 그럴듯한 이유를 가지고 있기 때문에 어쩔 수 없는 트레이드오프의 문제라고 생각하기 쉽다.
그러나 일각에서는 제 3의 대안을 제시하기도 하는데,
대표적으로 **[윤분](http://hanksville.org/futureofutc/program/presentations/11_AAS_13-510.ppt.pdf)**(leap minute) 또는 **[윤시](http://www.leapsecond.com/LEAPHOUR/)**(leap hour)라고 하는 것이 있다.
윤초와 기본적인 개념은 같으나,
한 번에 1분 또는 1시간만큼씩 조정해서 빈도를 그에 반비례하여 수십년 또는 수백년에 한 번 꼴로 줄이려 하는 점이 다르다.
이에 윤초 존치 측에서는 오히려 빈도가 적어지면 테스트가 안 되어서 [Y2K]() 수준의 마이그레이션이 필요할 수 있는 것 아니냐고 반박한다.

{{%twen 1329314793908359170%}}
I'm a diehard proponent of leap hours. We would only adjust UTC by an hour when ΔT reaches, say, ±2000s, not ±0.7s today. Every single application sensitive to length-of-day changes is already using the Terrestrial Time (TT), so we have much larger headroom for civil timekeeping. Leap hours are much doable than leap seconds because we already have a regular (uh, irregular in fact) adjustment to the civil time, namely “daylight saving” time. We can treat a leap hour as a large swath of time zone offset changes instead—already possible with tzdata today!

[...] First of all, leap hours would be very infrequent; the first one won't occur this century (ΔT changed by <100s since 1972). DST changes would be much more frequent than leap hours, and software vendors have to support them anyway, so we can piggyback leap hours on DST changes.

[...] Essentially, [yes](http://leapsecond.com/LEAPHOUR/). But even if leap hours become more frequent (and [Steve Allen does predict this](https://ucolick.org/~sla/leapsecs/year2100.html)) this scheme would be much easier to handle than leap seconds. We should not need to have multiple ways to adjust the civil time. That's also possible. In fact there was also a [serious discussion of “leap minutes”](http://hanksville.org/futureofutc/preprints/files/19_AAS%2013-510_Seago.pdf) that would happen once or twice a century. I prefer leap hours because it's an internationally recommended increment for time zones, thus needs are unlikely to go away. One of main arguments against leap hours was that, it would probably happen 5–600 years later and we can't predict how the future people would react. So I'm predicting even at that point earthlings would still use time zones with hour increments. (Hopefully no more DST though)
{{%/twen%}}

{{<claim>}}
나는 윤시의 기본 가정은 정당하나, 적은 빈도로 인한 마이그레이션 문제는 온당한 지적이라고 생각한다.
이에 나는 제 4의 대안으로서 **윤시간대**를 강력하게 지지하는데,
윤시를 추가하되 이를 24:00:00부터 24:59:59까지의 시간으로 추가하지 않고 **시간대 오프셋을 변경하는 것으로** 처리하는 것이다.
따라서 하루의 길이는 항상 86 400초로 유지되며,
윤시 적용은 세계 표준의 문제가 아니라 각국의 시간대 전환의 문제가 되고,
굳이 한 시점에 윤시가 동시에 적용될 필요도 없어진다.
오프셋 변경은 전 세계에서 이미 자주 일어나는 일이므로 소프트웨어를 바꿀 필요도 없다.

구체적인 예로, 2500년에 (양의) 윤시간대가 추가된다 하면 본래의 UTC 옆에 제1 수정 협정 세계시(UTC<sub>1</sub>)가 추가될 것이다.
UTC<sub>1</sub>은 윤시 때문에 한 시간 앞당겨져서 1:00 UTC = 0:00 UTC<sub>1</sub>이 된다.
본래의 UTC 자체는 변화가 없기 때문에 이를테면 [한국 표준시](korea-standard-time)(KST)는 이 시점에서도 UTC+9일 것이나,
필요하다면 법령 개정을 통해 UTC+9에서 UTC<sub>1</sub>+9 = UTC+8로 전환할 수 있다.
이 때 이를테면 영국이 아직 전환을 하지 않았다면 영국과의 시차는 일시적으로 한 시간 줄어들게 되는데,
이미 영국은 [서머 타임](summer-time)을 실시하고 있기 때문에 시차가 한 시간 왔다 갔다 하는 건 새삼스러운 일이 아니다.

윤시간대는 “[시간대](time-zone)”라는 개념 자체가 [근시일 내에 사라질 수 없다](https://qntm.org/abolish)는 가정에 기반한다.
우주라면 또 모르겠지만, 적어도 지구 상에서는 평균 태양시와 비슷한 지역 시간대를 쓰려는 요구가 지속적으로 있을 것이다.
이 때문에 서로 다른 시간대의 존재와, 그 시간대의 오프셋이 시간에 따라 바뀔 수 있다는 점 또한 소프트웨어에 이미 반영되어 있고 뭘 또 새로 만들 필요가 없다.
그럼 이미 있는 시간대 오프셋을 윤초 비스무리한 것을 구현하는 데 재활용하는 것이 합당한 것이다.
만에 하나 시간대의 개념이 사라진다면, 그 시점에서는 지구 자전이 시각 표준에 사용될 이유도 사라질테니 윤초도 신경 쓸 필요가 없다.

