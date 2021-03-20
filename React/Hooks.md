# Hooks
Hooks는 리액트 16.8 버전에 새로 도입된 기능으로 함수형 컴포넌트에서도 상태 관리를 할 수 있는 useState, 렌더링 직후의 작업을 설정하는 useEffect등의 기능을 제공하여 기존의 함수형 컴포넌트에서 할 수 없었던 다양한 작업들을 할 수 있게 해준다.  


## useState
useState는 가장 기본적인 Hook이다. 함수형 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해준다. 사용법은 일단 import문으로 node_modules의 react를 불러와 useState를 디스트럭처링 할당으로 꺼내어 사용한다.  
```javascript
import React, { useState } from 'react';
```

그리고 useState를 호출하여 상태와 그 상태를 바꿔주는 함수를 가진 배열을 반환받는다. 이것 또한 디스트럭처링 할당으로 꺼내어 각각의 변수에 담아 사용한다.  
useState를 호출할때 인수로 전달한 값은 상태의 초기값이 된다.
```javascript
const [value, setValue] = useState(0);
```

### 함수 컴포넌트에서 관리해야 할 상태가 여러개일때
하나의 useState 함수는 하나의 상태 값만 관리 할 수 있다. 만약 함수 컴포넌트에서 관리해야 할 상태가 여러개 일때는 useState를 여러 번 사용하면 된다.
```javascript
// name과 age상태를 관리해야 하는 경우
const [name, setName] = useState('');
const [age, setAge] = useState(0);
```

혹은 하나의 상태를 객체로 관리해 줄 수도 있다.
```javascript
const [userInfo, setUserInfo] = useState({ name: '', age: 0 });
```

## useEffect
useEffect는 리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다. 클래스형 컴포넌트에서의 라이프사이클 메서드인 componentDidMount와 componentDidUpdate를 합친 형태와 유사하다. 즉, 컴포넌트가 브라우저에 처음 마운트 된 직후와 컴포넌트가 브라우저에 리렌더링 된 직후에 할 작업들을 정의할 수 있는 Hook이다.  

역시 사용법은 import문으로 node_modules의 react를 불러와 useEffect를 디스트럭처링 할당으로 꺼내어 사용할 수 있다.
```javascript
import React, { useState, useEffect } from 'react';
```  

그리고 useEffect를 호출할때 컴포넌트가 렌더링 된 후 실행 할 작업들을 정의해놓은 콜백함수를 인수로써 전달한다.  
```javascript
useEffect(() => {
  console.log('컴포넌트가 렌더링 되었습니다.');
  console.log({
    name, // useState에서 선언한 상태들
    age
  });
});
```

### 마운트 될 때만 실행하고 싶을 때
useEffect에서 설정한 함수를 컴포넌트가 화면에 맨 처음 렌러딩 될 때만 실행하고, 업데이트 될 떄는 실행하고 싶지 않다면 함수의 두 번째 인수로써 빈 배열을 전달해주면 된다.  
```javascript
useEffect(() => {
  console.log('컴포넌트가 마운트 될 때만 실행됩니다.');
}, []);
```

### 특정 값이 업데이트 될 때만 실행하고 싶을 때
특정 값이 변경될 때만 호출하고 싶은 경우 클래스 컴포넌트에서는 라이프사이클 메서드를 활용하여 아래와 같이 구현할것이다.  
```javascript
componentDidUpdate(prevProps, prevState) {
  if (prevProps.value !== this.props.value) {
    console.log('컴포넌트의 props가 변경되었습니다.');
  }
}
```

함수 컴포넌트에서는 useEffect의 두 번째 인수로 전달되는 배열 안에 검사하고 싶은 값을 넣어 주면 된다. 만약 name이라는 상태가 변했을때만 useEffect의 첫번째 인수로 전달 된 콜백함수를 실행하겠다고 한다면 아래와 같이 작성하면 된다.
```javascript
useEffect(() => {
    console.log(`${name}값이 변경 되었습니다.`);
  }, [name]);
```

위에서는 상태라고 넣어줬지만 값을 넣어줘야 된다고 한 이유는 props로 전달받은 값을 넣어줘도 되기 때문이다. useEffect는 기본적으로 렌더링 된 직후마다 실행되며 두 번째 인수에 들어 갈 배열의 요소로 무엇을 넣는지에 따라 실행되는 조건이 달리지게 된다.  

### 뒷정리(clean up) 함수
컴포넌트가 언마운트 되기 전이나 업데이트 되기 직전에 어떠한 작업을 수행하고 싶다면 useEffect에서 뒷정리(cleanup) 함수를 반환해 주어야 한다.  
```javascript
useEffect(() => {
    console.log('effect');
    console.log(name);
    return () => {
      console.log('cleanup');
      console.log(name);
    };
  });
```

뒷정리 함수가 실행 될 때는 컴포넌트가 업데이트 되기 직전의 값을 보여준다. 코드의 순서로 보면 return문이 나중에 실행될 것 같지만 실제로는 return문의 뒷정리 함수가 먼저 호출되고 렌더링이 된 후 useEffect의 콜백함수의 몸체가 실행되는것이다.  

클래스 컴포넌트와 비교한다면 뒷정리 함수는 getSnapshotBeforeUpdate 또는 componentWillUnmount메서드라고 볼 수 있을것 같다.
뒷정리 함수가 아닌 useEffect의 콜백함수의 몸체는 componentDidMount 또는 componentDidUpdate메서드라고 볼 수 있을것 같다.

뒷정리 함수도 마찬가지로 언마운트(unmount)될때만 호출하고 싶다면 useEffect함수의 두 번째 인수값으로 비어 있는 배열을 전달해주면 된다.
```javascript
useEffect(() => {
    console.log('effect');
    console.log(name);
    return () => {
      console.log('cleanup');
      console.log(name);
    };
  }, []);
```

## useReducer
useReducer는 useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트 해주고 싶을 때 사용하는 Hook이다. reducer는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action)값을 전달받아 새로운 상태를 반환해주는 함수이다. reducer함수에서 새로운 상태를 만들 때는 항상 불변성을 지켜주어야 한다.

```javascript
function reducer(state, action) {
return { … }; // 불변성을 지키면서 업데이트한 새로운 상태를 반환한다.
}

// 액션 값은 주로 다음과 같은 형태로 이루어져 있다.
{
  type: ‘INCREMENT‘,
  // 다른 값들이 필요하다면 추가로 들어감
}
```

useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두 번째 파라미터에는 해당 리듀서의 기본값을 넣어준다. 이렇게 호출하게 되면 useReducer함수는 state값과 dispatch함수를 가지고 있는 배열을 반환하는데 보통 디스트럭처링 할당으로 받아서 사용한다.  
```javascript
// 위의 코드처럼 reducer함수를 정의 해놨다는 가정
const [state, dispatch] = useReducer(reducer, { value: 0 });
```  

state는 말 그대로 현재 관리하고 있는 상태(상태 객체)이고 dispatch함수는 액션을 발생시키는 함수이다. `dispatch(action);`과 같은 형태로 액션 값을 넣어주게 되면 리듀서 함수의 첫번째 파라미터로 상태가, 두번째 파라미터로 dispatch를 호출할때 전달했던 액션값이 전달되며 리듀서 함수가 호출된다.  
useReduce의 가장 큰 장점이 컴포넌트를 업데이트 하는 코드를 컴포넌트 바깥으로 분리할 수 있다는점이다. 함수 컴포넌트에서 관리해야 할 상태가 너무 많아졌을때 useState를 써봤다면 얼마나 함수 내부 코드가 길어지고 지저분해지는지 알 것이다.  

useReducer에서의 액션은 자바스크립트에서 사용하는 모든 표현식(값으로 평가 될 수 있는 식)을 사용할 수 있다. 


### useReducer를 사용한 Counter 컴포넌트 구현
생략됐지만 Button이란 컴포넌트를 이용해 -와 +버튼을 만들었고 Number라는 컴포넌트를 이용해 상태를 props로 전달하여 화면에 렌더링 하고있다.  
```javascript
import Button from 'components/Button/Button';
import Number from 'components/Number/Number';
import React, { useReducer } from 'react';

function reducer(state, action) {
  // action.type의 값을 참조하여 각기 다른 작업 수행
  switch (action.type) {
    case 'INCREMENT':
      return { number: state.number + 1 };
    case 'DECREMENT':
      return { number: state.number - 1 };
    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, { number: 0 });

  const increase = () => {
    dispatch({ type: 'INCREMENT' });
  };
  const decrease = () => {
    dispatch({ type: 'DECREMENT' });
  };
  return (
    <>
      <Button onClick={decrease}>-</Button>
      <Number>{state.number}</Number>
      <Button onClick={increase}>+</Button>
    </>
  );
}
```

### useReducer를 사용한 Input 컴포넌트 구현
이번엔 각각의 요소들을 컴포넌트로 나누지 않고 하나의 컨테이너 컴포넌트로써 구현  
```javascript
import React, { useReducer } from 'react';

// dispatch함수가 reducer를 호출할때 현재 상태값과 인수로 전달 한 action 값이 전달됨
function reducer(state, action) {
  return {
    // 기존의 state를 merge해주며 새로운 객체를 반환해 불변성 유지
    ...state,
    // changeEvent가 일어난 target요소의 name 어트리뷰트 값을 기준으로 상태를 변경
    [action.name]: action.value
  };
}

const Input = () => {
  // state에는 두번째 인수로 전달한 초기 상태가 담김
  // dispatch는 action값을 가지고 reducer를 호출해 줄 함수
  const [state, dispatch] = useReducer(reducer, {
    name: '',
    nickname: ''
  });

  const { name, nickname } = state;

// input에 onChange이벤트를 달아주어 변화가 될때마다 dispatch함수 호출
  const onChange = e => {
    dispatch(e.target);
  };

  return (
    <>
      /* 접근성을 고려한 aria-label */
      <label aria-label='이름'>
        <input name='name' value={name} onChange={onChange} />
      </label>
      <label aria-label='닉네임'>
        <input name='nickname' value={nickname} onChange={onChange} />
      </label>
      <div>이름: {name}</div>
      <div>닉네임: {nickname}</div>
    </>
  );
};

export default Input;
```

## useMemo
useMemo를 사용하면 함수 컴포넌트 내부에서 발생하는 연산을 최적화 시킬 수 있다. 클래스 컴포넌트에서는 shouldComponentUpdate메서드와 같은 용도로 쓴다고 볼 수 있다. useMemo는 boolean값을 리턴해서 판단하지는 않고 useMemo의 첫 번째 인수로는 작업 할 내용을 담은 콜백함수, 두번째 인수로는 배열을 전달하는데 배열의 요소에 있는 값이 바뀌었을때만 연산을 실행하게끔 해주는 용도이다.

아래 코드는 각각의 number값이 담긴 배열인 list가 변화되었을때만 useMemo에 전달된 콜백함수가 호출되는 예제이다. useMemo의 두번째 인수 값으로 list가 담긴 배열을 전달해주었다.

```javascript
import React, { useState, useMemo, useRef } from 'react';

const getAverage = list => {
  const sum = list.reduce((cur, num) => cur + +num, 0);
  console.log(sum);
  return list.length ? sum / list.length : 0;
};

export default function Average() {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  const numberHandler = ({ target }) => {
    setNumber(target.value);
  };

  const addList = () => {
    if (isNaN(number) || !number) {
      setNumber('');
      return;
    }
    setList([...list, number]);
    setNumber('');
  };

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={numberHandler} placeholder='숫자만 입력' />
      <button onClick={addList}>추가</button>
      <ul>
        {list.map(number => {
          return <li>{number}</li>;
        })}
      </ul>
      <span>{avg}</span>
    </div>
  );
}
```

## useCallback
useCallback은 useMemo와 아주 비슷하게 렌더링 성능을 최적화 해야하는 상황에서 사용한다. useCallback을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성할 수 있게 된다.  

위의 useMemo를 이용한 list의 평균값을 구하는 예제에서 numberHandler와 addList함수는 컴포넌트가 리렌더링 될 때마다 함수가 새로 생성된다. 만약 컴포넌트의 렌더링이 자주 발생하거나 렌더링 해야 할 컴포넌트의 갯수가 많아진다면 애플리케이션의 성능에 안좋은 영향을 주게 될 것이다.

아래 코드는 위의 useMemo에서 작성한 코드와 동일하게 동작한다.
```javascript
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = list => {
  const sum = list.reduce((cur, num) => cur + +num, 0);
  return list.length ? sum / list.length : 0;
};

export default function Average() {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

// useCallback은 첫번째 인수로 전달 된 콜백함수 자체를 반환한다.
  const numberHandler = useCallback(({ target }) => {
    setNumber(target.value);
  }, []);

// 단 두번째 인수로 전달 된 배열내의 요소(상태)가 변할때만 반환해준다.
  const addList = useCallback(() => {
    if (isNaN(number) || !number) {
      setNumber('');
      return;
    }
    setList([...list, number]);
    setNumber('');
  }, [number, list]);

// useMemo는 첫번째 인수로 전달 된 콜백함수의 리턴값, 즉 여기선 getAverage함수를 반환한다.
  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input value={number} onChange={numberHandler} placeholder='숫자만 입력' />
      <button onClick={addList}>추가</button>
      <ul>
        {list.map(number => {
          return <li>{number}</li>;
        })}
      </ul>
      <span>{avg}</span>
    </div>
  );
}
```

하지만 useMemo와 useCallback의 차이점은 useMemo는 첫번째 인수로 전달된 콜백함수의 return한 값을 반환하고 useCallback은 첫번째 인수로 전달된 콜백함수 자체를 반환하게 된다.  
그래서 숫자, 문자열, 객체처럼 일반 값을 return값을 통해 재사용하려면 useMemo를, 함수 자체를 재활용하고 싶다면 useCallback은 사용하면 될 것 같다.

### 정리
- useMemo : 첫번째 인수로 전달 된 콜백함수의 return값을 반환함
- useCallback : 첫번째 인수로 전달 된 함수(콜백함수) 자체를 반환함

## useRef
useRef는 함수 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해준다. useMemo와 useCallback예제에서 사용했던 Average컴포넌트에서 추가 버튼을 눌렀을때 useRef를 이용하여 input에 focus가 되게끔 작성해보자.

```javascript
import React, { useState, useMemo, useCallback, useRef } from 'react';

const getAverage = list => {
  const sum = list.reduce((cur, num) => cur + +num, 0);
  return list.length ? sum / list.length : 0;
};

export default function Average() {
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');
  const $input = useRef(null);

  const numberHandler = useCallback(({ target }) => {
    setNumber(target.value);
  }, []);

  const addList = useCallback(() => {
    $input.current.focus();
    if (isNaN(number) || !number) {
      setNumber('');
      return;
    }
    setList([...list, number]);
    setNumber('');
  }, [number, list]);

  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input ref={$input} value={number} onChange={numberHandler} placeholder='숫자만 입력' />
      <button onClick={addList}>추가</button>
      <ul>
        {list.map(number => {
          return <li>{number}</li>;
        })}
      </ul>
      <span>{avg}</span>
    </div>
  );
}
```

클래스 컴포넌트와 다른점은 바로 `ref.focus()` 메서드를 사용할 수 있는것이 아닌 `current`라는 프로퍼티에 접근해야지만 해당 ref의 실제 Element에 접근할 수 있다.    
```javascript
ref.current.focus();
```

### ref를 활용한 지역 변수 사용
컴포넌트 지역 변수를 사용해야 할 때도 useRef를 활용할 수 있다. 여기서 지역 변수는 렌더링과 상관없이 바뀔 수 있는 값을 의미한다. 클래스 형태로 작성 된 컴포넌트의 경우에는 지역 변수를 사용해야 할 때 아래와 같이 클래스 멤버(static멤버) 형태로 작성할 수 있다.  
```javascript
import React, { Component } from 'react';

class MyComponent extends Component {
  id = 1
  setId = (n) => {
    this.id = n;
  }
  printId = () => {
    console.log(this.id);
  }
  render() {
    return (
      <div></div>
    );
  }
}

export default MyComponent;
```

위와 같은 코드를 함수 컴포넌트로 작성할때 useRed를 활용할 수 있다.  
```javascript
import React, { useRef } from 'react';
 
const Localvariable = () => {
  const id = useRef(1);
  const setId = (newId) => {
    id.current = newId;
  }
  const printId = () => {
    console.log(id.current);
  }
  return (
    <div></div>
  );
};
 
export default Localvariable;
```

ref의 안의 값이 바뀌더라도 컴포넌트는 다시 렌더링 되지 않는다. 컴포넌트는 props, state의 변경 또는 부모 요소가 렌더링(리렌더링) 됐을때와 강제로 렌더링 시켯을때만 다시 렌더링되게 된다. 그렇기 때문에 렌더링과 관련되지 않은 값을 관리할 때는 이런식으로 ref를 활용해 줄 수도 있다.

## 커스텀 Hooks 만들기
여러 컴포넌트에서 비슷한 기능을 반복해서 사용한다면 Hook은 어차피 함수이기 떄문에 커스텀 Hook을 만들어주어 재사용 해주면 될 것이다.  

useReducer에서 작성 했던 여러가지 input을 관리하는 코드를 useInputs라는 Hooks으로 분리해보자.

```javascript
import { useReducer } from 'react';

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value
  };
}

export default function useInputs(initialForm) {
  const [state, dispatch] = useReducer(reducer, initialForm);
  const onChange = e => {
    dispatch(e.target);
  };
  return [state, onChange];
}
```

그리고 Info 컴포넌트를 생성하여 직접 만든 커스텀 Hooks을 사용해보자.
```javascript
import React from 'react';
import useInputs from 'Hooks/useInputs';

const Info = () => {
  const [state, setState] = useInputs({
    name: '',
    nickname: ''
  });

  const { name, nickname } = state;

  return (
    <>
      {/* 접근성을 고려한 aria-label */}
      <label aria-label='이름'>
        <input name='name' value={name} onChange={setState} />
      </label>
      <label aria-label='닉네임'>
        <input name='nickname' value={nickname} onChange={setState} />
      </label>
      <div>이름: {name}</div>
      <div>닉네임: {nickname}</div>
    </>
  );
};

export default Info;
```

input을 사용해야 하는 상황에서 상태까지 관리하며 범용적으로 사용할 수 있는 Hooks를 만들어 재사용할 수 있게 된 것이다.

## Hooks 라이브러리
빌트인 및 커스텀 Hooks말고도 다른 개발자들이 만들어 라이브러리로 배포해놓은 것들을 적절히 잘 사용한다면 프로젝트의 생산성이 올라갈 것이다.
- [Collection of React Hooks](https://nikgraf.github.io/react-hooks/)  
- [rehooks](https://github.com/rehooks/awesome-react-hooks)

## 정리
이렇게 React에서 Hooks를 사용하면 클래스 컴포넌트에서 사용하는 기능을 대부분 구현할 수 있다. 물론 Hooks가 등장했다고 해서 클래스 컴포넌트를 사용되는것이 잘못된 방식은 아니라고 생각한다. 오히려 클래스 컴포넌트에서 더욱 명확하고 확실하게 처리되는 기능들이 있고 함수의 한계때문에 구현하지 못하는 기능들도 몇가지 있다.  
React 공식문서에서도 기존의 함수 컴포넌트가 React개발의 새로운 패러다임이 될 것이라고 하지만 클래스 컴포넌트도 앞으로 계속 지원될 것이라고 한다. 그렇기 때문에 이미 작성되어 있는 클래스 컴포넌트를 함수 컴포넌트로 바꾼다거나 하는 일은 하지 않아도 된다. 하지만 클래스 컴포넌트로 구현된 코드를 유지보수 해야 할 일도 있을수 있으니 클래스 컴포넌트로 개발하는 방식을 아는것도 필수사항이라고 생각한다.