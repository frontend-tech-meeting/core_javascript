## 코어 자바스크립트 - #2 실행 컨텍스트

- 일시 : 2021.08.06(금) 19:00~
- 발제자 : 김성훈
- 참석자 : 안도현, 오종택, 김성훈, 황연욱
- 발표 자료 : [성훈님 블로그](https://wecode.notion.site/ec32f36a0814465d9a30583134d90d66) 참고

<br>

### 1. 논의된 / 새롭게 알게 된 내용

- 컨텍스트 - 문맥, 맥락
- LIFO (Last In First Out) → 스택 (Stack) 자료구조

- 실행 컨텍스트는? ⇒ 3가지
    - `Lexical Environment` (렉시컬 환경)
    - `Variable Environment` (변수 환경)
    - `ThisBinding`: `this`에 대한 정보를 담는 곳. 함수 안에서 `this`에 접근할 때 실행 컨텍스트의 `ThisBinding` 안에 담긴 정보를 따름.

- Lexical Environment
    - Environment Record ⇒ 호이스팅
      - 코드가 실제로 실행되기 전에 코드 전체를 훑는 과정을 통해, 사용되는 모든 식별자들에 대한 정보를 수집하여 Environment Record 에 저장한다. 이 과정으로 인해, 호이스팅이란 현상이 발생.
    - Outer Environment Reference ⇒ 여러 실행 컨텍스트처럼 `단방향 연결형 리스트` ⇒ 스코프 체인 형성
      - 현재 실행 컨텍스트가 생성되는 시점의 바깥 실행 컨텍스트에 대한 참조를 Outer Environment Referece 에 저장한다. 덕분에 여러 실행 컨텍스트들이 `단방향 연결형 리스트` 처럼 서로 연결. 이를 통해 스코프 체인이 형성된다.

종택) 하위 스코프(자식)가 상위 스코프(부모)의 상속을 받는다는 관점에서 이해할 수도 있을거 같다. (딥다이브)

성훈) ECMAScript? → `Lexical Environment` / `Variable Environment` 차이? 스냅샷을 굳이 뜨는 이유를 모르겠습니다. 명세를 봐야 할거 같은데 명세를 읽기에는 어렵네요.

도현) 실행 컨텍스트가 생성 안되는 경우는?
- **연욱) ES5까지는 블록 레벨 스코프가 없다 ⇒ ES6 부터는 실행 컨텍스트 안에(함수) + 블록 레벨 스코프를 나누기 위한 렉시컬 스코프가 더 생긴다. ⇒ 컨텍스트 하나 당 렉시컬 하나가 아니다**
- 종택) with, try/catch ⇒ 명세를 봐야 이해가 될듯

연욱) 소스 코드 4개 타입으로 나눈거 ⇒ 각 경우에 생성하는 렉시컬 환경 내부들이 다름.

연욱) 함수는 선언될 때 환경을 기억한다
- 실행 컨텍스트 콜 스택에 쌓임
- **중요) 실행 컨텍스트 ⇒ 실행 후 콜 스택에서 빠지면 렉시컬도 없어지나?
- 그게 아니라 독립적이라서 실행 컨텍스트가 참조하고 있을 뿐이다
- 콜스택에서 실행 컨텍스트가 빠지더라도 렉시컬 환경은 함수에서 참조하고 있으면 안없어짐 (참조 카운트) ⇒ 클로저 구현!**


</br>

### 2. 면접 질문 & 답변

**성훈**
- `실행 컨텍스트`는 언제 생성되고, 구성요소가 어떤 것들이 있는지
    - `함수가 실행될 때` + `전역 코드`, `eval` + `모듈 컨텍스트`(딥다이브 참고)
- `호이스팅`이 무엇인지 그리고 왜 발생하는지
    - let, const도 호이스팅이 일어나는지 => 일어난다, 다만 초기화 안될 뿐이다 (`initialize` 관련 에러 발생)

**종택**

- 자바스크립트 엔진이 코드를 실행하기 까지의 과정을 두 단계로 나누어 설명하고, 이를 `실행 컨텍스트`와 연관지어 설명해주세요.
- `let`과 `const`는 `호이스팅`이 일어날까요?
- `실행 컨텍스트`와 연관되어 있는 자료구조 두 가지를 말하고, 각각 어떻게 연관되어 있는지 설명해주세요.

**연욱**
- 자바스크립트의 호이스팅은 어떻게 일어나는가

**도현**
- 변수 은닉화 - 스코프 체인 상에 중복된 식별자 ⇒ 상속 내려올 때 현재 실행 중인 스코프 체인 내에서 부터 위로 올라가면서 찾으면 검색 중단 ⇒ 가장 가까이 있는거
    - 연욱) eslint `no shadowing error`가 이 경우