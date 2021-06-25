# 2021년 06월 25일 금요일 TIL(TypeScript 고급타입, frameworkless routing)

## 오늘 한 일과 느낀점

### TypeScript 고급타입

#### intersection
intersection은 `&`를 사용하여 2가지의 타입을 합쳐서 표현할 수 있다. 

#### union
union은 `|`를 사용하여 여러가지 타입 중 하나라는 표현으로 사용할 수 있다. 

만약 union을 사용하여 함수의 파라미터로 원시값이 아닌 object를 선택적으로 받았다고 가정했을때 typeof연산자로 판단할 수 없다. 그럴땐 제네릭을 이용할 수 있다.

```javascript
// User와 Action이라는 인터페이스가 있다는 가정
function precess(v: User | Action) {
  if ((<Action>v).do) return (<Action>v).do();
  if ((<User>v).name) return (<Action>v).name;
}
```
또는 타입가드를 정의해주고 사용한다.
```javascript
function isAction(v: User | Action): v is Action {
  return (<Action>v).do !== undefined;
}

// User와 Action이라는 인터페이스가 있다는 가정
function precess(v: User | Action) {
  if (isAction(v)) return v.do();
  else return v.name;
}
```

#### typeAlias
type을 정의하는것이며 Interface와 비슷하지만 특정 type에 이름을 붙힐수도 있다.
```javascript
type TextContent = string;
type Person = {
  name: string;
  age: number
}
```

보통 객체의 형태(내부 프로퍼티와 그 값들의 타입)를 정의할때 인터페이스와 더불어 많이 사용된다. 하지만 문법의 차이 이외에 어떤것들이 다른지 궁금해서 찾아보았다.

1. 확장하는 방법이 다르다. 비교하기 쉽게 의사코드로 같은 식별자를 사용했다.

```javascript
// interface
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  bark(): string;
}

// typeAlias
type Animal = {
  name: string;
  age: number;
};

type Dog = Animal & { bark(): string };
```

그리고 typeAlias는 불가능한 선언전 확장이 interface는 가능하다. type은 변수처럼 같은 이름으로 하나의 type만 선언할 수 있지만 interface는 중복으로 선언하면 자동으로 병합이 된다.
```javascript
// Dogs타입은 bark와 run메서드 모두 구현해야한다.
interface Dogs extends Animals {
  bark(): string;
}

interface Dogs {
  run(): void;
}
```

2. interface는 객체만 type은 일반 type에 대한 별칭도 가능

```javascript
interface TextContent {
  value: string;
}

type TextContent = string;
```

3. 성능
[yceffort님의 포스팅](https://yceffort.kr/2021/03/typescript-interface-vs-type)을 보고 참고하던중 마이크로소프트의 타입스크립트 github저장소의 내용을 참고하여 정리하신글이 있었다. 여러개의 type또는 interface를 확장할때 interface는 객체 합성패턴을 사용하지만 type의 경우 재귀적으로 순회하며 속성들을 병합한다는데 이 경우 일부 never타입이 되어 제대로 병합이 안될 수 있다고 한다. 그래서 만약 객체의 타입만을 정의할때는 그냥 interface를 쓰는편이 더욱 안정적이고 빠르다고한다. 물론 빠르다는것은 js로 변환되는 컴파일과정의 속도를 말한다.

#### 인덱스 타입
인덱스타입은 속성의 이름이 정해져있지 않고 동적으로 처리해야할때 사용한다. 인덱스타입에는 string또는 number값만 올 수 있다. 왜냐하면 객체의 프로퍼티가 그러하기 때문이다. 하지만 string으로 정의를 해도 0, 1, 2같은 숫자값은 넣을 수 있다. 왜냐하면 숫자값은 실제로는 프로퍼티키에서 문자열로 적용되기 때문이다. 만대로 number로 정의를 하게되면 string값의 프로퍼티키는 사용할 수 없다. 객체에 각각의 프로퍼티가 값의 타입은 같게끔 설정할때 유용하다는 생각이 들었다.

### frameworkless routing
프레임워크 없이 SPA로 여러개의 라우트를 관리하는법에 대해 다루었다. 사실 그냥 명령형으로 그냥 간략하게 혼자 짜본적은 있지만 그닥... 마음에 드는 코드가 나오지는 않았었다. 일단 `/`로 구분된 후의 라우트는 구분하지 못했었기 때문이다. 그리고 책에 나오는 예제를 읽어봤는데.. 지금은 글로 정리를 할만큼 이해가 완벽히 되지는 않았다. 정규표현식을 이용하여 `/`이후의 파라미터 값들을 읽어들였는데 regExp에 약해서 더 어려웠던것도 있던것 같다. diff알고리즘을 직접 따라쳐본 후 다시 한번 라우트 파트를 읽어봐야할것 같다.

## 내일 할 일
- diff 알고리즘을 적용하여 TodoList만들어보기 (오늘 하려고했으나 시간 부족...)
- 친구가 보내준 알고리즘 문제 10개 이상 풀어보기
