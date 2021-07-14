# 제네릭(Generic)

Generic은 재사용을 목적으로 함수나 클래스의 선언 시점이 아닌, 사용 시점에 타입을 선언할 수 있는 방법을 제공한다. 즉, 사용 시점마다 타입이 바뀌어야하는 값을 위한것이다. 마치 함수에 인수를 전달하듯 제네릭 형태로 타입을 전달한다고 생각하면 된다.

제네릭을 사용하지 않는 아래의 예제를 살펴보자

```javascript
function toArray(a: number, b: number): number[] {
  return [a, b];
}
toArray(1, 2);
toArray('1', '2'); // Error - TS2345: Argument of type '"1"' is not assignable to parameter of type 'number'.
```

toArray는 전달받은 인수를 배열의 요소로 만들어 반환하는 함수이다. 하지만 매개변수의 타입을 지정해놨기 때문에 number타입의 요소들만을 가진 배열만 만들 수 있게 됐다. 그럼 Union을 사용하여 string타입까지 허용해보자.

```javascript
function toArray(a: number | string, b: number | string): (number | string)[] {
  return [a, b];
}
toArray(1, 2); // Only Number
toArray('1', '2'); // Only String
toArray(1, '2'); // Number & String
```

하지만 가독성이 떨어지고 만약 객체 또는 그 이외의 타입을 요소로 가지는 배열을 반환시키고 싶다면 매번 함수를 수정해주거나 새로운 함수를 정의해야 할 것이다. 이것을 제네릭을 통해 해결할 수 있다.

```typescript
function toArray<T>(a: T, b: T): T[] {
  return [a, b];
}

toArray<number>(1, 2);
toArray<string>('1', '2');
toArray<string | number>(1, '2');
toArray<number>(1, '2'); // Error
```

위처럼 함수를 사용(호출)할때 T부분에 타입을 지정해주면 내부에 미리 지정해놓은 T부분이 전달한 타입으로만 사용이 가능하게된다. 만약 내부적으로 타입추론이 가능한 구조라면 호출시 타입전달을 생략할 수도 있다.

```typescript
function toArray<T>(a: T, b: T): T[] {
  return [a, b];
}

toArray(1, 2);
toArray('1', '2');
toArray(1, '2'); // Error
```

### 제약 조건(Constraints)

인터페이스나 타입 별칭을 사용하는 제네릭을 작성할 수도 있다.
다음 예제에서는 제네릭에 별도의 제약 조건이 없기 때문에 사용시 모든 타입을 전달해줄 수 있다.

```typescript
interface MyType<T> {
  name: string;
  value: T;
}

const dataA: MyType<string> = {
  name: 'Data A',
  value: 'Hello world',
};
const dataB: MyType<number> = {
  name: 'Data B',
  value: 1234,
};
const dataC: MyType<boolean> = {
  name: 'Data C',
  value: true,
};
const dataD: MyType<number[]> = {
  name: 'Data D',
  value: [1, 2, 3, 4],
};
```

만약 타입 변수 T에 특정한 타입만 허용하고 싶은경우 `extends` 키워드를 사용하여 제약조건을 추가해줄 수 있다. string과 number만 허용하는 제약조건을 걸어보자.

```typescript
interface MyType<T extends string | number> {
  name: string;
  value: T;
}

const dataA: MyType<string> = {
  name: 'Data A',
  value: 'Hello world',
};
const dataB: MyType<number> = {
  name: 'Data B',
  value: 1234,
};
const dataC: MyType<boolean> = {
  // TS2344: Type 'boolean' does not satisfy the constraint 'string | number'.
  name: 'Data C',
  value: true,
};
const dataD: MyType<number[]> = {
  // TS2344: Type 'number[]' does not satisfy the constraint 'string | number'.
  name: 'Data D',
  value: [1, 2, 3, 4],
};
```

type과 interface는 식별자 영역과 타입구현 영역으로 구분할 수 있다. 아래에 예제처럼 type은 `=`의 왼쪽은 식별자 영역, 오른쪽은 타입구현 영역, interface는 첫 중괄호를 기준으로 한다.

```typescript
type U = string | number | boolean;

// type 식별자 영역 = 타입 구현 영역
type MyType<T extends U> = string | T;

// interface 식별자 영역 { 타입 구현 영역 }
interface IUser<T extends U> {
  name: string;
  age: T;
}
```

### 조건부 타입(Conditional Types)

제약 조건과 다르게 `타입 구현 영역`에서 사용하는 `extends`는 삼항 연산자를 사용할 수 있다. 이를 조건부 타입이라고 한다.

```typescript
type U = string | number | boolean;

// type 식별자 = 타입 구현
type MyType<T> = T extends U ? string : never;

// interface 식별자 { 타입 구현 }
interface IUser<T> {
  name: string;
  age: T extends U ? number : never;
}
```

```typescript
// `T`는 `boolean` 타입으로 제한.
interface IUser<T extends boolean> {
  name: string;
  age: T extends true ? string : number; // `T`의 타입이 `true`인 경우 `string` 반환, 아닌 경우 `number` 반환.
  isString: T;
}

const str: IUser<true> = {
  name: 'Neo',
  age: '12', // String
  isString: true,
};
const num: IUser<false> = {
  name: 'Lewis',
  age: 12, // Number
  isString: false,
};
```

원래 그렇듯 삼항 연산자를 연속해서 사용하여 여러가지 조건을 검사할 수 있다.

```typescript
type MyType<T> = T extends string
  ? 'Str'
  : T extends number
  ? 'Num'
  : T extends boolean
  ? 'Boo'
  : T extends undefined
  ? 'Und'
  : T extends null
  ? 'Nul'
  : 'Obj';
```

#### infer

`infer` 키워드를 사용해 타입 변수의 타입 추론(Inference) 여부를 확인할 수 있다.

아래 코드는 U가 추론 가능한 타입이라면 참, 아니면 거짓이라는걸 나타낸 의사코드이다.

```typescript
T extends infer U ? X : Y

type MyType<T> = T extends infer R ? R : null;

const a: MyType<number> = 123;
```

**잘 이해가 되지 않아 Type inference는 추후에 다시 정리 예정!**
