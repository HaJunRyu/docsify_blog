# 타입 별칭(Type Aliases)과 인터페이스와의 차이

`type` 키워드를 사용하여 새로운 타입 조합을 만들 수 있다.
하나 이상의 타입을 조합해 별칭(이름)을 부여하며, 정확히는 조합한 각 타입들을 참조하는 별칭을 만드는 것이다.
일반적으로 둘 이상의 조합으로 구성하기 위해 유니온(Union)을 많이 사용한다.

```javascript
// 이렇게 특정 타입의 별칭을 지정해줄 수도 있다.
type MyType = string;
type YourType = string | number | boolean;
type TUser =
  | {
      name: string,
      age: number,
      isValid: boolean,
    }
  | [string, number, boolean];

const userA: TUser = {
  name: 'Neo',
  age: 85,
  isValid: true,
};
const userB: TUser = ['Evan', 36, false];

function someFunc(arg: MyType): YourType {
  switch (arg) {
    case 's':
      return arg.toString(); // string
    case 'n':
      return parseInt(arg); // number
    default:
      return true; // boolean
  }
}
```

## 인터페이스와 타입별칭의 차이

인터페이스와 타입 별칭은 같은 역할을 할 수 있지만 사용법이나 동작에서 차이를 보인다.

### 선언 방법

```tsx
interface PersonInterface {
  name: string;
  age: number;
}

type PersonType = {
  name: string;
  age: number;
};
```

### 확장하는 방법

```tsx
interface StudentInterface extends PersonInterface {
  school: string;
}

type StudentType = PersonType & {
  school: string;
};
```

### 선언적 확장

인터페이스는 선언적 확장이 가능하지만 타입 별칭은 불가능하다.

```tsx
interface PersonInterface {
  name: string;
}

// 이때 결국 PersonInterface는 name과 age프로퍼티를 가져야하는 객체타입으로 확장됨
interface PersonInterface {
  age: number;
}

// 그래서 이와 같이 name과 age를 모두 프로퍼티로써 가지고 있어야 에러가 발생하지 않음
const Hajun: PersonInterface = {
  name: 'Hajun',
  age: 25,
};

// 타입 별칭은 선언적 확장이 지원되지 않음
type PersonType = {
  name: string;
};

// 식별자 중복 에러 발생
type PersonType = {
  age: number;
};
```

### 사용할 수 있는 값

인터페이스는 당연히 객체형식으로만 정의할 수 있다.

하지만 타입 별칭은 그렇지 않다. 그렇기 때문에 타입 별칭은 리터럴 타입도 사용할 수 있다.

```tsx
interface Foo {
  value: string;
}

type Bar = string;
type Literal = 'foo' | 'bar';
```

### 컴파일시의 성능

아래 링크는 Microsoft/TypeScript wiki에 작성된 인터페이스와 타입 별칭의 확장시의 차이점에 대해 다룬 글이다. 아래 내용을 읽어보고 관심이 생긴다면 짧은글이니 직접가서 읽어보면 좋을것 같다.

[https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections)

내용은 타입을 확장할때 interface의 extends키워드를 사용하는게 좋다는 내용인데 이유는 interface는 객체 합성 패턴을 사용하지만 타입 별칭은 intersection(&)을 이용해서 재귀적으로 속성을 합성하기 때문이라고 얘기했다.

그리고 일부의 경우 타입 별칭을 intersection을 이용해서 확장했을시에 의도치않게 never타입이 나오는 경우도 있다고한다.

아래의 예시를 에디터에 작성해보면 객체끼리 확장을 했지만 같은 프로퍼티 키를 가진 객체를 확장했을때 never타입이 만들어지는것을 볼 수 있다.

```tsx
type Type1 = { a: 1 } & { b: 2 }; // 정상적으로 확장됨
type Type2 = { a: 1; b: 2 } & { b: 3 }; // Type2는 never타입이다.

const t1: Type1 = { a: 1, b: 2 }; // 잘 할당 된다.
const t2: Type2 = { a: 1, b: 3 }; // Type 'number' is not assignable to type 'never'.(2322)
const t3: Type2 = { a: 1, b: 2 }; // Type 'number' is not assignable to type 'never'.(2322)
```

그리고 성능적인 측면에서 또 하나의 차이점은 interface를 이용하여 확장하면 캐싱이 되지만 타입별칭은 그렇지 않다고 한다.

```
TypeScript wiki 본문 중의 내용

Type relationships between interfaces are also cached, as opposed to intersection types as a whole.
```

성능과 번외로 에러에 관한 이슈가 있었는데 이전 타입스크립트 버전에서는 타입별칭을 사용한 구문에서 에러가 났을때 컴파일러가 어디에서 에러가 났는지 알려주지 못했었다고 하는데 이제는 버전이 업데이트가 되면서 해결되었다고 한다.
