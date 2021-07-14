# 타입 별칭(Type Aliases)

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

### interface와의 차이

그렇다면 interface와 type 키워드의 차이는 무엇일까

Type Aliases, 말 그대로 타입 별칭이다. 새로운 타입을 생성하는것이 아닌 타입을 나타내는 이름(식별자)을 만드는것이다. 하지만 interface는 새로운 이름으로 새로운 타입(형식)을 만들어낸다.

그리고 이전버전의 Typescript에서는 Type Aliases의 확장을 허용하지 않았다. (정확히는 확장 기능이 없었다.) 물론 지금은 Intersection(`&`)을 사용하여 확장할 수 있지만 interface와 문법과 동작이 다르고 현재 객체를 intersection으로 합성한 경우 특정 타입이 Never타입으로 변환되는 이슈도 있다고 한다. 그렇기 때문에 기본적인 객체 형태의 타입은 interface로 정의하고 interface가 표한하지 못하는 형태에 한해(유니온이나 튜플을 사용해야하는 경우) Type Aliases를 사용하는것이 좋다고 공식문서가 안내하고 있다.
