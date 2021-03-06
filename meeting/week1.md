## 코어 자바스크립트 - #1 데이터 타입

---

- 일시 : 2021.08.03(화) 19:00~
- 발제자 : 안도현
- 참석자 : 안도현, 오종택, 김성훈, 황연욱
- 발표 자료 : [도현님 블로그](https://brunch.co.kr/@a3869b174cc1492/1) 참고

<br>

### 1. 논의된 / 새롭게 알게 된 내용

- `심볼(Symbol)` 타입 자체가 이해 가지 않는 건 아니었지만, 실제 프로젝트에 적용해 사용할 수 있는 예제가 뭐가 있는지 모르겠다.

<br>

- 프로그래밍 언어에서 데이터 타입 별로 데이터 영역의 크기를 미리 정해놓는 이유에 대한 연욱님 비유 좋았음.

> 예를 들어, 당신이 식당주인이라고 가정하자.
>
> 어느 손님이 “오늘 저녁에 단체로 갈테니까 준비주세요”라고만 말한다면 당신은 몇명이 올 줄 몰라서 단체손님을 맞기 위해서 식당 전체를 비워둬야 되는 비효울적인 상황이 발생할 수도 있다.
>
> 하지만, “오늘 저녁에 30명 갈테니까 준비해주세요”라고 말한다면 당신은 5인 테이블 6개만 비워두고 나머지 공간은 다른 손님을 받는데 효율적으로 활용할 수 있다.

<br>

- `"변수의 참조 카운트가 0이 된다"`는 문장의 뜻은 더 이상 해당 변수를 필요로 하는 식별자가 없으므로 해당 변수에 할당된 메모리를 해제한다는 것.

  - Garbage Collector 알고리즘 Mark & Sweep 참조 ([MDN 링크](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management))
  - C언어 같은 저수준 언어에서는 메모리 관리를 위해 `malloc()`이나 `free()` 등을 사용함.

<br>

(사진 첨부)

- `원시값`의 메모리 영역 ⇒ `변수 영역` / `데이터 영역`
- `객체`의 메모리 영역 ⇒ `변수 영역` / `데이터 영역` / `객체의 변수 영역`

  - `변수 영역`에 할당된 메모리 주소가 바뀌는 게 아니라 `객체 변수 영역`에 할당된 메모리 주소가 바뀌는 것.
  - "기본형은 값을 복사하고, 참조형은 주소 값을 복사한다" (x)
  - **"기본형도 결국 주소 값을 참조한다. 단지 주소, 값을 복사하는 과정이 한 번인지 두 번인지의 차이인 것이다." (o)**

<br>

- `불변성`

  - 값에 변화가 없을 것임을 보장
  - 객체의 경우 값에 변화가 있었을 경우 새로운 객체를 만들어주는 방식으로 가변 데이터를 불변 데이터처럼 관리한다.

<br>

- `undefined` / `null`

  - 값이 없음을 개발자가 명시적으로 표현할 경우 `null`을 사용 ⇒ 단, `null`은 타입이 `object`로 나오니 주의
  - 자바스크립트 엔진 자체가 값이 없음을 표현한 경우 `undefined`를 사용

- `배열은 길이 만큼 빈 공간을 확보하고 index를 지정하는게 아니라 객체와 마찬가지로 특정 인덱스에 값이 할당될 때 빈 공간을 확보한다.`

연욱) 브라우저에 따라 메모리에 이미 할당된 적 있는 원시값이 꼭 하나의 주소에 고유하게 저장되지는 않는다더라.

- "a"가 @2004 주소에 저장되어 있을 때, 다른 식별자가 "a"를 찾을 경우 @2004를 참조 (코어 자바스크립트)
- "a"가 @2004 주소에 저장되어 있더라도, "a"가 @2005에 중복 저장되어 있을 수도 있음 (모던 자바스크립트 딥다이브)
- ECMAScript 표준에서 정의하는 부분은 문법적인 부분에 대한 것이지 메모리 할당 등의 구현 까지 정의해주지는 않기 때문 (copy on write 방식?)

<br>

- 래퍼 객체 원리

```js
// String { '0' : 'j', ..., length: 10, __proto__: { ... } }
const str1 = new String("javascript");

// "javascript"
const str2 = "javascript";
```

<br>

### 2. 면접 질문 & 답변

---

#### 도현

- `기본형 데이터`는 `불변값`의 특징을 가지고 `참조형 데이터`는 `가변값`이라는 특징을 가지는 이유는 뭔가요?
- 데이터를 복사할 때 `기본형은 값을 복사`하고, `참조형은 주소를 복사`한다라는 개념이 맞는 설명일까요?
- `얕은 복사`와 `깊은 복사`의 차이에 대해 설명해주세요.
- `undefined`라는 값을 직접 할당하는 경우와 자바스크립트 엔진을 통해 `undefined`가 할당되는 경우의 차이에 대해 설명해주세요.

#### 연욱

- 자바스크립트의 객체를 가변하게 만들어 놨다. 그런데 왜 최근에 불변 객체를 사용하는 것이 화두가 되었는지 설명해주세요.
  - 객체를 가변하도록 설계한 것은 한정된 메모리 공간을 효율적으로 사용하기 위함
  - 하지만, 최근 하드웨어의 성능이 올라가면서 상대적으로 메모리 공간의 효율성보다는 개발단계의 안정성에 집중할 수 있는 여건이 조성되었기에 불변객체 개념을 많은 프레임워크, 라이브러리 등에서 활용함

#### 성훈

- `불변 객체` 사용하는 이유와 리액트에서 불변성을 지키며 상태를 관리하는 이유는?
  - 불변객체를 사용하는 이유
     1.  객체의 내부 프로퍼티가 최초 선언 이후로 변경되지 않음 -> 안정성 확보
     2. 객체간의 비교를 내부 프로퍼티의 일치여부를 일일이 순회하면서 검사하는게 아닌, 참조값만을 비교해서 판단하기 위해서
     - 리액트에서 불변성을 지키며 상태를 관리하는 이유는 state, props가 변했는지 판단할 때 참조값만을 비교해서 판단하기 위해서
#### 종택

- 자바스크립트에서 0.1 + 0.2의 연산결과가 0.3이 아닌 이유를 설명하고, 이에 대한 해결 방안을 제시해주세요.
  - 자바스크립트의 number 타입이 64비트 부동소수점. 10진수를 2진수로 변환하다 보면 최대 표현할 수 있는 소수점 자리수 맨 끝에서 반올림할 경우 발생하는 오차
  - 해결 방법은 (0.1 + 0.2).toFixed(2)
