# 컴포넌트(Component)
React는 컴포넌트를 통해 UI를 재사용 가능한 개별적인 여러 조각으로 나누고, 각 조각을 필요할때 조합하여 UI를 구성할 수 있게 만들어졌다.  

컴포넌트의 기능은 단순한 템플릿 이상이다.  
데이터가 주어졌을때 이에 맞추어 UI를 만들어 주고, 라이프 사이클 API를 이용하여 컴포넌트가 화면에서 나타날 때, 사라질 때, 변화가 일어날 때 정의해놓은 작업들을 처리할 수 있으며,  
메서드를 만들어 특별한 기능을 붙혀 줄 수도 있다.

## props(속성)
props는 properties의 약어로 컴포넌트의 속성을 정의할 때 사용하는 요소이다.  
props 값은 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 정의 할 수 있다.

```jsx
const App = () => {
  return <MyComponent name="Hajun" />;
};
```

부모 컴포넌트에서 정의 한 props를 컴포넌트에서 사용할때는 함수의 파라미터로 받아와 사용할 수 있다.
```jsx
const MyComponent = props => {
return <div>Hi! my name is {props.name}</div>;
};
```

### defaultProps
만약 부모요소에서 정의되지 않았을때 props의 기본값을 정해주는 방법이 있다. props를 사용 할 컴포넌트로 가서 아래와 같이 정의해준다.  

```jsx
MyComponent.defaultProps = {
  name: 'default name'
};
```

### children
children은 컴포넌트를 사용할 때 컴포넌트 태그 사이의 내용을 보여주는 props이다.

```jsx
const App = () => {
  return <MyComponent>React</MyComponent>;
};
```

위 코드에서 MyComponent 컴포넌트 태그 사이에 작성한 React라는 문자열은 MyComponent함수의 파라미터인 props.children으로 들어간다.

```jsx
const MyComponent = props => {
  return (
    <div>
      children value is {props.children}
    </div>
  );
};
// 결과 : children 값은 React 입니다.
```

### 디스트럭처링 할당
props를 사용할때 굳이 props.key를 사용하는것보다 디스트럭처링 할당을 통해 원하는 프로퍼티만을 추출하여 사용 할 수 있다.

```jsx
const MyComponent = ({ name, children }) => {
  return (
    <div>
      Hi! my name is {name}<br />
      children value is {children}
    </div>
  );
};
```

### propTypes
컴포넌트의 필수 props를 지정하거나 props의 타입을 지정할 때는 propTypes를 사용한다.  
컴포넌트의 propTypes를 지정하는 방법은 defaultProps를 설정하는 것과 비슷하다.  

propTypes를 사용하려면 컴포넌트의 상단에 import 구문을 사용하여 불러와야 한다.

```jsx
// MyComponent.jsx 컴포넌트
import React from ‘react‘;
import PropTypes from ‘prop-types‘;


const MyComponent = ({ name, children }) => {
(…)
```

그리고 아래와 같이 타입을 지정해 줄 수 있다.
```jsx
MyComponent.propTypes = {
  name: PropTypes.string
};
```

name이라는 props에는 무조건 문자열(string) 값을 전달해댜 한다.  
만약 문자열이 전달이 안됐을 경우에는 어떻게 동작하는지 살펴보자. 
```jsx
function App() {
  return (
    <>
      <MyComponent name={1}>칠드런</MyComponent>
    </>
  );
}
```

![propsTypes를 지키지 않았을때 정상 렌더링 됨](images/proptypes-rendering.png)

위와 같이 렌더링은 정상적으로 된다.

하지만 브라우저의 Console 창을 살펴보면 경고를 띄워주고 있다.

![propsTypes를 지키지 않았을때 경고를 띄워줌](images/proptypes-Warning.png)

TypeScript처럼 실제 타입을 검사하여 에러를 내진 않지만 Console창에 Type이 맞지 않는다고 알려주기 때문에 만약 예상치 못하게 데이터에 변화가 되어 props에 들어가도 쉽게 개발자가 정한 Type에 맞춰 수정할 수 있다.

#### propTypes의 종류
- array : 배열
- arrayOf(PropType) : 특정 PropType으로 이루어진 배열을 의미한다. 예를 들어 arrayOf(PropTypes.number)는 숫자로 이루어진 배열이다.
- bool : true혹은 false값 (boolean)
- func : 함수
- number : 숫자
- object : 객체
- string : 문자열
- symbol : ES6의 Symbol
- node : 렌더링할 수 있는 모든 것(숫자, 문자열, JSX코드)
- instanceOf(클래스) : 특정 클래스의 인스턴스  
예를 들어 instanceOf(Person)이라고 하면 Person클래스의 인스턴스 Type이다.
- oneOf(array) : 주어진 배열 요소의 값 중 하나의 Type  
- oneOfType(array) : 주어진 배열 안의 Type중 하나
- objectOf(propType) : 객체의 모든 키 값이 인수로 주어진 PropType인 객체
- shape(object) : 인수로 주어진 객체의 스키마를 가진 객체
- any : 모든 propType (아무거나)

#### isRequired를 사용하여 필수 propTypes 설정
propTypes를 지정하지 않았을때 경고 메시지를 띄워 주는 방법이 있다.  
propTypes를 지정할 때 뒤에 isRequired를 붙혀주면 된다. 

```jsx
MyComponent.propTypes = {
  name: PropTypes.string.isRequired
};
```

위와 같이 작성하게 되면 name이라는 props를 정의하지 않고 MyComponent를 렌더링하게 되면 브라우저의 Console에서 경고 메시지를 띄워준다.


## 클래스형 컴포넌트와 함수형 컴포넌트
React에서 컴포넌트를 선언하는 방식은 두 가지이다.  
하나는 클래스형 컴포넌트이고, 다른 하나는 함수형 컴포넌트이다.  
기본적으로 몇가지 추가기능을 제외하고는 두 가지 유형의 컴포넌트는 동일하다고 볼 수 있다.

## 클래스형 컴포넌트
클래스형 컴포넌트는 ES6의 class문법을 사용하여 작성한다.  

클래스형 컴포넌트에서는 꼭 render 함수가 있어야 하고, render함수 내부에서 컴포넌트가 보여주어야 할 JSX를 반환해야한다.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### 클래스형 컴포넌트에서 props 사용하기
클래스형 컴포넌트에서 props를 사용할 때는 render 함수에서 this.props를 참조하면 된다.  
그리고 defaultProps와 propsTypes 또한 class의 밖에서 동일하게 사용하면 된다.

```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
  render() {
    const { name, children } = this.props;

    return (
      <div>
      Hi! my name is {name}<br />
      children value is {children}
    </div>
    );
  }
}

MyComponent.defaultProps = {
  name: 'default name'
};

MyComponent.propTypes = {
  name: PropTypes.string.isRequire
};
```

class의 경우에는 defaultProps와 propTypes를 class의 정적 메서드로 등록해주는것이니 아래의 코드처럼 static메서드로 등록해줄 수 있다.

```jsx
class MyComponent extends Component {
  static defaultProps = {
    name: 'default name'
  }

  static propTypes = {
    name: PropTypes.string.isRequire
  }

  render() {
    const { name, children } = this.props;

    return (
      <div>
      Hi! my name is {name}<br />
      children value is {children}
    </div>
    );
  }
}
```


## 함수형 컴포넌트
함수형 컴포넌트는 간단하게 JavaScript함수를 작성하고 내부에 필요한 코드들을 작성 후 JSX를 반환하면 된다.
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### 함수형 컴포넌트의 장점
- 클래스형 컴포넌트보다 선언하기가 편하다.
- 메모리 자원도 클래스형 컴포넌트보다 덜 사용한다.
- 프로젝트를 빌드한 후 배포할때도 클래스형 컴포넌트를 사용할때보다 결과물 파일의 크기가 더 작다. (크게 차이가 나진 않는다고 함)

### 함수형 컴포넌트의 단점
- state사용이 불가능 함
- 라이프사이클 API의 사용이 불가능하다.

하지만 위의 단점은 리액트 v16.8 업데이트 이후 Hooks가 도입되면서 해결되었다.  
완벽하게 클래스형 컴포넌트와 동일하게 사용할 수 있는것은 아니지만 조금 다른 방식으로 비슷한 작업을 할 수 있게 되었다.

## state(상태)
리액트에서 state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다.  
props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이고, 컴포넌트는 자산의 props를 읽기 전용으로만 사용할 수 있다.  
props를 바꾸려면 부모 컴포넌트에서 바꾸어 주어야 한다.

리액트에는 두 가지 종류의 state가 있다. 하나는 클래스형 컴포넌트가 가지고 있는 state이고, 다른 하나는 함수형 컴포넌트에서 useState라는 함수를 통해 사용하는 state이다.

### 클래스 컴포넌트에서의 state

#### this.state, this.setState
클래스 컴포넌트에서는 아래 코드와 같이 클래스 내부에서 this.state로 상태를 초기화, 참조할 수 있다.  
클래스 컴포넌트의 state는 객체 형식이어야 한다.  
this.setState로 상태를 변경해 줄 수 있다.
```jsx
class Counter extends Component {
  constructor(props) {
    super(props);
    // state의 초깃값 설정하기
    this.state = {
      number: 0
    };
  }
  render() {
    const { number } = this.state; // state를 조회할 때는 this.state로 조회한다.
    return (
      <div>
        <h1>{number}</h1>
        <button
          // onClick props를 통해 버튼이 클릭 되었을 때 호출할 함수를 지정한다.
          onClick={() => {
            // this.setState를 사용하여 state를 변경해 줄 수 있다.
            this.setState({ number: number + 1 });
          }}
        >
          +1
        </button>
      </div>
    );
  }
}

위에서 사용한 setState는 인수로 넣어준 객체 안에 들어있는 값만 바꿔준다. 만약 state에 name이란 프로퍼티가 있었다고 가정했을때 `setState({ number: number + 1 })`이렇게 작성 해줬다고 해서 덮혀쒸워져 name프로퍼티가 사라지는것이 아닌 인수로 작성해준 객체 내부의 number값만 업데이트 되게 된다.
```

그리고 위의 코드에선 state의 초기값 설정을 위해 constructor를 호출하여 그 안에서 this.state를 참조하여 할당해주었는데 그냥 클래스변수를 사용하는 방법도 있다.

```jsx
class Counter extends Component {
  state = {
    number: 0,
    name: 'Hajun'
  };
  render() {
    const { number, name } = this.state; // state를 조회할 때는 클래스 내부에서 this.state로 조회한다.
    return (...);
  }
}
```

#### this.setState에 객체 대신 함수를 전달
만약 하나의 이벤트에서 setState를 2번 호출한다고 하면 정상적으로 작동하지 않는다.  
왜냐하면 setState는 state를 비동기적으로 업데이트 하기 때문이다.  
비동기적으로 업데이트 한다는뜻은 state값이 즉시 바뀌는것이 아니라는 것이다.

```jsx
(...)
render() {
  const { number } = this.state; // state를 조회할 때는 this.state로 조회한다.
  return (
    <div>
      <h1>{number}</h1>
      <button
        // onClick props를 통해 버튼이 클릭 되었을 때 호출할 함수를 지정한다.
        onClick={() => {
          // this.setState를 사용하여 state를 변경해 줄 수 있다.
          this.setState({ number: number + 1 });
          this.setState({ number: this.state.number + 1 });
        }}
      >
        +1
      </button>
    </div>
  );
}
(...)
```

하지만 하나의 이벤트에서 state를 2번 연속으로 변경하고 싶은 경우 setState의 인수로 객체 대신 함수를 전달 해주는 것이다.

아래와 같은 형식으로 사용할 수 있다.

```jsx
this.setState((prevState, props) => {
  return {
    // 업데이트하고 싶은 내용
  };
})
```
인수로 전달하는 함수의 첫번째 파라미터에는 기존 상태객체가 들어오고 두번째 인수로는 현재 지니고 있는 props가 들어온다.  
만약 상태를 업데이트 하는 과정에서 props가 필요없다면 두번째 파라미터는 생략해도 된다.  

위와 같이 setState에 함수를 전달하여 하나의 이벤트에서 2번 호출한다면 객체를 전달했을때와는 달리 정상적으로 상태가 2번 바뀌게 작동할 것이다.

Counter 컴포넌트에서는 2씩 증가하는 버튼을 아래 코드처럼 작성할 수 있다.

```jsx
(...)
render() {
  const { number } = this.state; // state를 조회할 때는 this.state로 조회한다.
  return (
    <div>
      <h1>{number}</h1>
      <button
        // onClick props를 통해 버튼이 클릭 되었을 때 호출할 함수를 지정한다.
        onClick={() => {
          // this.setState를 사용하여 state를 변경해 줄 수 있다.
          this.setState(prevState => ({
            number: prevState.number + 1
          }));
          this.setState(prevState => ({
            number: prevState.number + 1
          }));
        }}
      >
        +2
      </button>
    </div>
  );
}
(...)
```

#### this.setState가 끝난 후 특정 작업 실행
setState를 사용하여 값을 업데이트 한 후 특정 작업을 하고 싶을때는 setState의 두번째 인수로 콜백 함수를 등록하여 처리할 수 있다.

```jsx
<button
  // onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정합니다.
  onClick={() => {
    this.setState({ number: number + 1 },
      () => {
        console.log('setState가 호출되었습니다.');
        console.log(this.state);
      }
    );
  }}
>
  +1
</button>
```

### 함수 컴포넌트에서의 useState
React v16.8 이전에서는 함수형 컴포넌트에서 state를 사용할 수 없었다.  
하지만 v16.8 이후에 Hooks의 useState를 이용하여 함수형 컴포넌트에서도 state를 사용할 수 있게 되었다.

일단 useState를 사용하려면 컴포넌트 파일의 최상단에서 useState를 불러와야한다.
```jsx
import React, { useState } from 'react';
```

useState는 배열을 반환하고 이 반환된 배열의 요소들을 디스트럭처링 할당으로 각각 변수에 할당해보자.

```jsx
const [age, setAge] = useState(25);
// 다음년도로 가는 버튼의 이벤트 핸들러로 사용한다는 가정
const nextYear = () => setAge(age + 1);
```

useState 함수의 인수로는 상태의 초깃값을 넣어준다.  
클래스형 컴포넌트에서는 state의 초기값은 객체 형태를 넣어줘야했지만 useState에서의 초기값은 항상 객체는 아니어도 된다.  
원시값과 객체, 값으로 평가될 수 있는 모든것이 들어올 수 있다.  

useState는 배열을 반환한다고 했는데 반환되는 배열의 첫 번째 요소는 현재 상태(처음에는 useState의 인수로 넣어준 초기값이 들어가있음)이고, 두번째 요소는 상태를 바꾸어 주는 함수이다.  
이 함수를 Setter함수라고 부른다.  

#### 하나의 컴포넌트에서 useState 여러 번 사용하기
한 컴포넌트에서 useState를 여러 번 사용할 수 있다.

```jsx
const [age, setAge] = useState(25);
const [name, setName] = useState('Hajun');
```

하지만 위의 코드 같은 경우는 그냥 객체를 state로 쓴다면 하나만 사용해도 될 것이다.

```jsx
const [person, setPerson] = useState({ age: 25, name: 'Hajun' });
```

### state를 사용할 때 주의사항
상태를 바꿀때 클래스형 컴포넌트에서는 setState를, 함수형 컴포넌트에서는 useState를 호출하여 반환받은 Setter 함수를 사용해야한다.

만약 state객체의 참조값을 바꿔주지 않고 mutating하는것은 잘못 된 코드이다.  

클래스 컴포넌트의 잘못 된 상태 변경
```jsx
this.state.number = this.state.number + 1;
this.state.array = this.array.push(2);
this.state.object.value = 3;
```  

함수 컴포넌트의 잘못 된 상태 변경
```jsx
const [object, setObject] = useState({ a: 1, b: 2 });
object.b = 3;
```

React는 불변성(Immutable)을 유지하는 방식을 사용하고 있다.  
그렇기 때문에 배열이나 객체를 업데이트 할 때 배열이나 객체의 사본을 만들고(새로운 참조값을 만들어줌) 그 사본에 값을 업데이트 한 후 그 사본의 상태를 setState나 Setter함수를 통해 업데이트 한다.  

객체
```jsx
const object = { a: 1, b: 2, c: 3 };
setObject({ ...object, b: 2 }); // 사본을 만들어서 b 값만 덮어 쓰기
```  

배열
```jsx
const array = [
{ id: 1, content: 'HTML' },
{ id: 2, content: 'CSS' },
{ id: 3, content: 'JavaScript' }
];

setData(array.concat({ id: 4, content: 'React' })); // 새 항목 추가 (Array.prototype.concat)
setData([...array, { id: 5, content: 'Angular' }]); // 새 항목 추가 (스프레드 문법)
setData(nextArray.filter(item => item.id !== 2)); // id가 2인 항목 제거
setData(nextArray.map(item => (item.id === 5 ? { ...item, value: 'Vue' } : item))); // id가 5인 항목의 value를 'Vue'로 설정
```  
