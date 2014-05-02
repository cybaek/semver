소프트웨어의 버전 정하기
=====================

소프트웨어나 라이브러리를 만들어서 주변사람이나 세상에 공개할 때, 버전을 어떻게 정하시나요? 쉽게는 0.1부터 올라가는 방법도 있을테고, 아니면 0.0.1부터 시작해서 열심히 달려가서 1.0.0을 정식으로 배포할 수도 있겠습니다. 각기 나름의 방법으로 버전을 정해서 사용하고는 하지만, 여기에 조금의 형식을 정해서 사용한다면, 만드는 사람과 쓰는 사람 모두가 더 나은 의미를 주고 받고, 더 부드럽게 소프트웨어 프로젝트를 진행할 수 있습니다.

이미 여러 소프트웨어 패키지 관리 시스템에서 잘 활용되고 있는 [Semantic Versioning](http://semver.org/)이라는 체계가 있습니다. 어떻게 소프트웨어나 라이브러리의 버전을 정하고 관리할 지를 명시적으로 선언함으로써 그 소프트웨어의 버전을 사용하는 이의 입장에서도 이번에 새로 나온 버전의 API가 어느정도 수준으로 바뀌었는지, 예전처럼 그대로 써도 되는건지, 아니면 바뀐 API에 맞게 고쳐작성해야하는 지를 쉽게 판단할 수 있습니다.

아쉽게도, 이 명세가 한국어로 번역되지 않은것 같아서, 무리해서 번역을 해봤습니다. 편집했으면 하는 부분이나, 오탈자 수정등의 의견은 [깃헙이슈](https://github.com/hatemogi/semver/issues)에 남겨주시면 참고해서 반영하도록 하겠습니다.

어느 정도 잘 정리됐다 판단되는 시점이 오면, 원저작자 사이트에 번역본을 실어달라 부탁하겠습니다.

이하, 한국어 번역판 이어집니다.

유의적 버전 2.0.0-ko1
======================

요약
----

버전을 주.부.수 숫자로 하고:

1. 기존 버전과 호환되지 않게 API가 바뀌면 "주(主) 버전"을 올리고,
1. 기존 버전과 호환되면서 새로운 기능을 추가할 때는 "부(部) 버전"을 올리고,
1. 기존 버전과 호환되면서 버그를 수정한 것이라면 "수(修) 버전"을 올린다.

주.부.수 형식에 정식배포전 버전이나 빌드 메타데이타를 위한 라벨을 덧붙이는 방법도 있다.

머리말
------

소프트웨어 관리의 세계에는 "의존성 지옥"이라 불리는 성가신 문제가 있다. 시스템 규모가 커질수록, 그리고 더 많은 패키지를 가져다 쓸 수록, 언젠가, 이 절망의 늪에 빠진 스스로를 발견하게 되기 쉽다. 

의존성이 높은 시스템에서는, 새 패키지 버전을 배포하는 일이 금방 끔찍해지곤 한다. 의존성 명세를 너무 엄격하게 관리하면, 버전에 갇히게 될 위험이 있다(의존하는 모든 패키지의 새 버전을 배포하지 않고는 업그레이드 할 수 없게 된다). 의존성을 너무 느슨하게 관리하면, 버전이 엉켜서 괴롭게 될 것이다(지나치게 나중 버전까지 호환될 거라 가정한 경우). 버전에 갇히거나 엉켜서 쉽고 안전하게 프로젝트를 계속 진행할 수 없다면 의존성 지옥에 빠진 것이다.

이 문제의 해결책으로, 버전 번호를 어떻게 정하고 올려야하는지를 명시하는 규칙과 요구사항을 제안한다. 이 규칙들은 기존 오픈 소스/비공개 소스 소프트웨어에 널리 활용되는 규칙을 바탕으로 했으나, 반드시 따르려는 제약을 두지는 않았다. 이 시스템이 동작하려면, 먼저 공개(public) API를 선언해야한다. 문서화 작업과 소스 코드 자체로 드러낼 수 있다. 어떤 방식이든 API가 명확해야한다. 한번 공개 API를 정의하고 나면, 버전 번호를 올리는 방식을 통해 API가 어떻게 바뀌는지 표현 한다. 버전을 가.나.다 (주.부.수) 형식으로 정한다. API에 영향이 없는 버그 수정은 수(修)버전을 올리고, API가 호환되면서 바꾸거나 추가하는 경우에는 부(部)버전을 올리고, API가 호환되지 않는 변경이라면 주(主)버전을 올린다. 

이 체계를 "유의적 버전"이라고 부르고자 한다. 이 체계를 따르면, 버전 번호와 그 번호를 바꾸는 방법을 통해 특정 버전에서 다음 버전으로 넘어가면서 코드가 어떻게 바뀌는 지를 드러내게 된다.


유의적 버전 명세 (SemVer)
------------------------

이하 "반드시(MUST, REQUIRED, SHALL) ~한다", "절대 ~해서는 안된다(MUST NOT, SHALL NOT)", "가급적(SHOULD, RECOMMENDED) ~한다", "~하지 않는게 좋다(SHOULD NOT)", "할 수 있다(MAY, OPTIONAL)"의 표현은 [RFC 2119](http://tools.ietf.org/html/rfc2119)에 기술된 대로 해석한다.

1. 유의적 버전을 쓰는 소프트웨어는 반드시 공개 API를 선언한다. 이 API는 코드 자체로 선언하거나 문서로 엄격히 명시해야한다. 어떤 방식으로든, 정확하고 이해하기 쉬워야 한다.

1. 보통 버전 번호는 반드시 X.Y.Z의 형태로 하고, X, Y, Z는 각각 자연수이고, 절대로 0이 앞에 붙어서는 안된다. X는 주(主)버전 번호이고, Y는 부(部)버전 번호이며, Z는 수(修)버전 번호이다. 각각은 반드시 증가하는 수여야한다. 예: 1.9.0 -> 1.10.0 -> 1.11.0.

1. 특정 버전으로 패키지를 배포하고 나면, 그 버전의 내용은 절대 변경하지 말아야 한다. 변경분이 있다면 반드시 새로운 버전으로 배포하도록 한다.

1. 주버전 0(0.y.z)은 초기 개발을 위해서 쓴다. 아무때나 마음대로 바꿀 수 있다. 이 공개 API는 안정판으로 보지 않는게 좋다.

1. 1.0.0 버전은 공개 API를 정의한다. 이후의 버전 번호는 이때 배포한 공개 API에서 어떻게 바뀌는 지에 따라 올린다.

1. 수버전 Z (x.y.Z | x > 0)는 반드시 그전 버전 API와 호환되는 버그 수정의 경우에만 올린다. 버그 수정은 잘못된 내부 기능을 고치는 것이라 정의한다. 

1. 공개 API에 기존과 호환되는 새로운 기능을 추가할 때는 반드시 부버전 Y(x.Y.z | x > 0)를 올린다. 공개 API의 일부를 앞으로 제거할 것(deprecate)으로 표시한 경우에도 반드시 올리도록 한다. 내부 비공개 코드에 새로운 기능이 대폭 추가되거나 개선사항이 있을 경우에도 올릴 수 있다. 부버전을 올릴 때 수버전을 올릴 때만큼의 변화를 포함 할 수도 있다. 부버전이 올라가면 수버전은 반드시 0에서 다시 시작한다.

1. 공개 API에 기존과 호환되지 않는 변화가 있을 때는 반드시 주버전 X(X.y.z | X > 0)를 올린다. 부버전이나 수버전급 변화를 포함할 수 있다. 주버전 번호를 올릴 때는 반드시 부버전과 수버전을 0으로 초기화 한다.

1. 수버전 바로 뒤에 하이픈(-)을 붙이고 마침표(.)로 구분된 식별자를 더해서 정식 배포를 앞둔 (pre-release) 버전을 표기할 수 있다. 식별자는 반드시 아스키(ASCII) 문자, 숫자, 하이픈으로만 구성한다[0-9A-Za-z-]. 식별자는 반드시 한 글자 이상으로 한다. 숫자 식별자의 경우 절대 앞에 0을 붙인 숫자로 표기하지 않는다. 정식배포전 버전은 관련한 보통 버전보다 우선순위가 낮다. 정식배포전 버전은 아직 불안정하며 연관된 일반 버전에 대해 호환성 요구사항이 충족되지 않을 수도 있다. 예: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.  

1. 빌드 메타데이터는 수버전이나 정식배포전 식별자 뒤에 더하기(+) 기호를 붙인뒤에 마침표로 구분된 식별자를 덧붙여서 표현할 수 있다. 식별자는 반드시 아스키 문자와 숫자와 하이픈으로만 구성한다 [0-9A-Za-z-]. 식별자는 반드시 한 글자 이상으로 한다. 빌드 메타데이터는 버전간의 우선 순위를 판단하고자 할 때 반드시 무시해야한다. 그러므로, 빌드 메타데이터만 다른 두 버전의 우선 순위는 같다. 예: 1.0.0-alpha+001, 1.0.0+20130313144700, 1.0.0-beta+exp.sha.5114f85.

1. 우선순위는 버전의 순서를 정렬할 때 서로를 어떻게 비교할지를 나타낸다. 우선순위는 반드시 주, 부, 수 버전, 그리고 정식배포전 버전의 식별자를 나누어 계산하도록 한다 (빌드 메타데이터는 우선순위에 영향을 주지 않는다). 우선순위는 다음의 순서로 차례로 비교하면서, 차이가 나는 부분이 나타나면 결정된다: 주, 부, 수는 숫자로 비교한다. 예: 1.0.0 < 2.0.0 < 2.1.0 < 2.1.1. 주, 부, 수버전이 같을 경우, 정식배포전 버전이 표기된 경우의 우선순위가 더 낮다. 예: 1.0.0-alpha < 1.0.0. 주, 부, 수버전이 같은 두 배포전 버전간의 우선순위는 반드시 마침표로 구분된 식별자를 각각 차례로 비교하면서 차이점을 찾는다: 숫자로만 구성된 식별자는 수의 크기로 비교하고 알파벳이나 하이픈이 포함된 경우에는 아스키 문자열 정렬을 하도록 한다. 숫자로만 구성된 식별자는 어떤 경우에도 문자와 하이픈이 있는 식별자보다 낮은 우선순위로 여긴다. 앞선 식별자가 모두 같은 배포전버전의 경우에는 필드 수가 많은 쪽이 더 높은 우선순위를 갖는다. 예: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0.

유효한 유의적 버전의 BNF(Backus-Naur Form) 문법
-----------------------------------------------

    <유의적 버전> ::= <버전 몸통>
                    | <버전 몸통> "-" <배포전>
                    | <버전 몸통> "+" <빌드>
                    | <버전 몸통> "-" <배포전> "+" <빌드>

    <버전 몸통> ::= <주> "." <부> "." <수>

    <주> ::= <숫자 식별자>

    <부> ::= <숫자 식별자>

    <수> ::= <숫자 식별자>

    <배포전> ::= <마침표로 구분된 배포전 식별자들>

    <마침표로 구분된 배포전 식별자들> ::= <배포전 식별자>
                                        | <배포전 식별자> "." <마침표로 구분된 배포전 식별자들>

    <빌드> ::= <마침표로 구분된 빌드 식별자들>

    <마침표로 구분된 빌드 식별자들> ::= <빌드 식별자>
                                      | <빌드 식별자> "." <마침표로 구분된 빌드 식별자들>

    <배포전 식별자> ::= <숫자와 알파벳으로 구성된 식별자>
                      | <숫자 식별자>

    <빌드 식별자> ::= <숫자와 알파벳으로 구성된 식별자>
                    | <숫자들>

    <숫자와 알파벳으로 구성된 식별자> ::= <숫자 아닌 것>
                                        | <숫자 아닌 것> <식별자 문자들>
                                        | <식별자 문자들> <숫자 아닌것>
                                        | <식별자 문자들> <숫자 아닌것> <식별자 문자들>

    <숫자 식별자> ::= "0"
                    | <양의 숫자>
                    | <양의 숫자> <숫자들>

    <식별자 문자들> ::= <식별자 문자>
                      | <식별자 문자> <식별자 문자들>

    <식별자 문자> ::= <숫자>
                    | <숫자 아닌 것>

    <숫자 아닌 것> ::= <문자>
                     | "-"

    <숫자들> ::= <숫자>
               | <숫자> <숫자들>

    <숫자> ::= "0"
             | <양의 숫자>

    <양의 숫자> ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

    <문자> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J"
             | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T"
             | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d"
             | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n"
             | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
             | "y" | "z"


유의적 버전을 써야하는 이유
---------------------------

유의적 버전은 혁신적인 아이디어가 아니다. 사실, 이미 이와 비슷한 방식으로 버전을 정해서 쓰고 있었을 수 있다. 문제는 이와 "비슷한" 방식으로는 충분치 않다는 점이다. 어떠한 형태로 정식 명세를 정해서 따르지 않는다면, 버전 번호는 의존성 관리에 있어 무의미하다. 이상의 아이디어에 이름을 정하고 명시적인 정의를 내림으로써, 소프트웨어의 사용자에게 제작자의 의도를 전달하기 쉬워진다. 의도가 명확해져야만, (너무 지나치지는 않은) 융통성 있는 의존성 명세를 만들 수 있다.

유의적 버전으로 어떻게 의존성 지옥을 벗어날 수 있는지 간단한 예를 들어보자. "Firetruck"이라는 라이브러리가 있다고 하자. 이 라이브러리는 유의적 버전이 붙은 "Ladder"라는 패키지에 의존한다. Firetruck을 만들었을 때, Ladder는 버전 3.1.0이었다. Firetruck이 3.1.0에 처음으로 추가된 기능을 사용했기 때문에, Ladder의 의존성을 3.1.0 이상, 4.0.0 미만으로 지정할 수 있다. 이제, Ladder의 3.1.1 버전과 3.2.0 버전이 공개된다면, 패키지 관리 시스템에 그 버전을 넣을 수 있고 기존 소프트웨어와 호환될 것이라고 알 수 있다. 

물론, 책임감 있는 개발자로서 패키지가 업그레이드된 부분이 홍보된대로 잘 동작하는지 검증하고자 할 것이다. 실상은 지저분하다; 조심해서 관리하는 것 외에는 달리 도리가 없다. 유의적 버전을 사용함으로써 의존하는 패키지들의 새 버전들과 씨름하지 않고, 시간 낭비와 소란 없이 패키지를 공개하고 업그레이드 할 수 있다. 

만약 이상의 내용이 그럴싸하다면, 유의적 버전을 쓰기 시작하기 위해서 할 일은, 그렇게 하겠다 마음먹고 규칙을 따르기만 하면 된다. 여러분의 README 파일에 이 웹사이트의 링크를 추가해서 다른 사람들도 유용하게 쓸 수 있게 하자. 


FAQ
---

### 초기 개발 단계에 0.y.z 버전 관리는 어떻게 할까?

가장 간단한 방법은 최초 개발 배포를 0.1.0으로 하고, 이후 매 배포마다 부버전을 올리는 것이다.

### 언제 1.0.0을 배포해야할지 어떻게 알 수 있나?

소프트웨어가 실 서비스에 쓰이기 시작했다면 이미 1.0.0이라고 여길 수 있다. 사용자들이 믿고 쓸 수 있는 안정한 API가 있다면 1.0.0일 것이다. 하위 버전 호환성에 대해 우려하기 시작했다면 이미 1.0.0일 수 있다. 

### 유의적 버전 짓기가 신속한 개발과 빠른 이터레이션에 방해가 되지는 않을까?

주버전 0이 신속한 개발을 위한 것이다. 만약 API를 매일같이 바꾸고 있다면 0.y.z 버전을 쓰거나 별도의 다음번 주버전 배포를 앞 둔 개발 브랜치를 써야 한다. 

### 공개 API의 아주 사소한 부분이 하위호환이 되지 않는다고 주버전을 매번 올려야 한다면, 어느새 버전 42.0.0이 되어버리지는 않나?

책임감 있고 선견지명이 있는 질문이다. 의존하는 코드가 많은 소프트웨어에, 호환되지 않는 변화를 가볍게 도입해서는 안된다. 관련해서 업그레이드하기 위해 필요한 비용이 어마어마해질 수 있다. 호환되지 않는 변경분을 배포하기 위해 주버전을 올리려면, 바뀌는 부분으로 인한 여파와 그 비용과 혜택을 충분히 평가해야 한다.

### 공개 API 전체를 문서화하는 것은 너무 일이 많다!

프로 개발자로서 다른 사람들이 쓰게 하려고 만든 소프트웨어를 적절히 문서화하는 것은 책임이다. 프로젝트를 효율적으로 유지하기 위해 소프트웨어의 복잡성을 관리하는 일은 매우 중요한 일이고, 남이 소프트웨어를 어떻게 쓰는지 모르거나 어떤 메소드들을 안전하게 호출할 수 있는건지 모른다면 어려울 것이다. 장기적으로 볼 때, 유의미 버전과 잘 정의한 공개 API는 관련된 모든 사람과 모든 것이 순조롭게 지낼 수 있게 한다. 

### 부버전을 올리는데 실수로 호환되지 않는 변경이 들어갔다면 어떻게 해야하나?

유의미 버전 명세를 어겼다는 사실을 알게되면, 즉시 문제를 해결하고 호환성이 깨진 부분을 복구해서 새 부버전을 배포한다. 이 경우라도 이미 배포된 버전을 변경해서는 안된다. 필요한 경우라면 문제되는 버전을 문서화해서 사용자들로 하여금 주의하도록 한다.

### 공개 API는 유지한 채 내부의 의존성을 바꾼다면 어떻게 할까?

공개 API에 영향을 주지 않으므로 호환된다고 여긴다. 당신의 패키지와 똑같은 의존성을 명시한 소프트웨어는 별도로 의존성 명세가 있어야하고 작성자는 충돌을 눈치 챌 것이다. 변화가 수버전 수준인지 부버전 수준인지 결정하는 것은 버그를 수정하고자 한 작업인지, 새로운 기능을 추가하기 위해서인지에 달려있다. 후자의 경우, 추가적인 코드를 예상할 것이며, 그렇다면 당연히 부버전을 증가해야할 것이다.

### 버전 증가와 맞지 않는 공개 API 변경을 부주의하게 했다면 어떻게하나? (수버전 배포에서 잘못된 코드가 들어가서 깨지게 된 경우)

스스로 최선의 판단을 하자. 공개 API가 원래 의도했던대로 사용될 수 있게 바꾸는 작업에 영향을 받는 대규모 사용자 층이 있다면, 수정사항이 엄밀히 따지자면 수버전 배포로 여겨져야한다 하더라도, 주버전 배포를 하는 것이 최선일 것이다. 유의적 버전은 어떻게 버전 번호가 바뀌는지 의미를 전달하는 것이 전부임을 잊지 말자. 변경이 사용자에게 중요한 의미가 있다면, 버전 번호로 잘 알릴 수 있도록 한다. 

### 제거하는 기능들에 대해서는 어떻게 할까?

기존의 기능들을 사용하지 못하게 없애는 것은 소프트웨어 개발과정의 자연스러운 일부분이며, 때로는 앞으로 나아가기 위해 필수적인 일이기도 하다. 공개 API의 일부를 제거하고자 하다면 다음의 두가지 일을 해야한다: (1) 문서를 업데이트해서 사용자들에게 변화를 알리도록 한다. (2) 해당 기능이 제거될 거라 표시된 새 부버전을 적절한 시기에 배포한다. 새 주버전에서 완전히 기능을 제거하기 전에, 제거 될 것이라 표시한 부버전 배포를 최소한 한 번은 진행해서 사용자들이 원할하게 새로운 API를 사용할 수 있도록 해야한다.  

### 유의적 버전(SemVer)의 버전 문자열 길이에 제한이 있나?

없지만, 잘 판단 하자. 예를 들어, 255자 버전 문자열은 아마도 지나치게 긴 것일 것이다. 또, 몇몇 시스템은 문자열 길이에 대해 나름의 제한이 있을 것이다. 

작성자
------

유의적 버전 명세는 그라바타(Gravatars)의 창시자이자 깃헙(GitHub)의 공동창업자인 [톰 프레스턴-베르너(Tom
Preston-Werner)](http://tom.preston-werner.com)가 작성했다. 

원문에 대한 의견을 남기고자 한다면, [원문 깃헙에 이슈를 작성](https://github.com/mojombo/semver/issues)해 주기 바란다.

한국어 번역은 [김대현](http://hatemogi.com)이 했고, 관련한 의견은 [한국어판 깃헙에 이슈로 남겨주기](https://github.com/hatemogi/semver/issues) 바란다.

라이선스
--------

Creative Commons - CC BY 3.0
http://creativecommons.org/licenses/by/3.0/
