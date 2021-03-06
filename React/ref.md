# ref

## ref란?
ref란 DOM에 이름을 다는것이라고 생각하면 된다.  
일반적으로 HTML에서 고유한 이름을 부여할때는 id 어트리뷰트를 사용한다.  
```HTML
<div id="first-container"></div>
```

이렇게 HTML에서 id 어트리뷰트를 사용하여 이름을 부여하는 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법이 있는데 이것이 reference의 줄임말인 ref이다.  

왜 굳이 id를 쓰지 않고 ref를 쓰는 이유는 리액트는 컴포넌트를 기반으로 개발을 하게되고 같은 컴포넌트를 여러 번 사용한다고 가정했을때 id 어트리뷰트는 unique해야하는데 이럴때 id가 중복되어 문제가 생기게 된다.  
물론 props로 컴포넌트를 사용할때마다 id를 다르게 전달해주는 방법도 있지만 실수를 할 수도 있고 상당히 불편하다.  

## ref를 사용해야하는 상황
ref는 DOM을 꼭 직접적으로 건드려야 할 때 사용한다.  
예를 들면 특정 DOM요소에 포커스(포커스가 되는 요소라면) 주기 라던가, Canvas요소에 그림을 그리는 등의 상황이 있다.  

### 클래스 컴포넌트에서의 ref

#### 콜백 함수
ref를 만드는 가장 기본적인 방법은 콜백 함수를 사용하는것이다.  
ref를 달고자 하는 요소에 ref라는 props로 콜백함수를 전달해 주면 된다.  
이 콜백 함수는 ref 값을 파라미터로 전달 받는다.  
그리고 함수 내부에서 파라미터로 전달받은 ref를 컴포넌트의 클래스(멤버) 변수로 설정해준다.
```JSX
// 컴포넌트 내부 JSX
<input ref={(ref) => {this.input = ref}} />
```

이렇게 되면 ref를 할당해준 this.input은 input 요소의 DOM을 가리키게 된다.  
ref의 이름은 this.input이 아닌 원하는것으로 자유롭게 지정할 수 있다.  
DOM타입과 관계없이 이름을 지을 수 있다는 말이다.  

#### createRef
리액트에 내장되어 있는 createRef라는 함수를 사용해서 ref를 만들어 줄 수도 있다.  
createRef는 리액트 16.3 버전부터 도입되었기 때문에 사용할때는 리액트의 버전을 꼭 확인해 주어야한다.  

```JSX
import React, { Component } from ‘react‘;

export default class RefPratice extends Component {
  input = React.createRef();

  handleFocus = () => {
    this.input.current.focus();
  }

  render() {
    return (
      <div>
        <input ref={this.input} />
      </div>
    );
  }
}
```

createRef는 콜백함수 방식과 다르게 명시적으로 클래스(멤버) 변수로 ref를 선언한 후 createRef()를 호출하여 할당해주어야 한다.  
그리고 해당 변수를 ref를 달고자 하는 요소의 ref라는 props에 넣어주어야 한다.  

그리고 ref를 설정해 준 DOM요소를 참조하려면 this.변수명.current 프로퍼티로 참조하면 된다.  
콜백 함수와 다른점은 ref변수 뒤에 current를 꼭 붙혀줘야 한다는 점이다.

#### 컴포넌트에 ref 달기
앞에서는 DOM요소에 ref를 달았는데 컴포넌트에도 ref를 달 수 있다.  
주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓰인다.  
컴포넌트에 ref를 다는 방법은 DOM에 ref를 다는 방법과 같다. 

```JSX
<MyComponent
  ref={(ref) => {this.myComponent = ref}}
/>
```

컴포넌트에 ref를 달아주게 되면 컴포넌트 내부의 메서드 및 클래스(멤버) 변수에 접근 할 수 있다.  
컴포넌트 내부의 DOM요소의 ref는 클래스(멤버)변수로 등록되어 있을것이고 그 값에 접근하면 컴포넌트 외부에서도 해당 DOM을 조작할 수 있게 된다.  

위에서 myComponent라는 클래스 변수에 컴포넌트의 ref를 할당했으니 아래와 같이 사용할 수 있다. (컴포넌트 내부에는 input이라는 ref가 있다는 가정)
```JSX
myComponent.input
```

### 잘못된 사용법
ref를 사용할떄는 ref를 사용하지 않고 해결할 수 있는 기능인지 충분히 고려한 후에 활용해야 한다.  

그리고 서로 다른 컴포넌트끼리 데이터를 교류 할 때 ref를 사용하는것은 잘못된 사용법이다.  
구현은 가능은 하지만 이것은 리액트의 콘셉트(컨셉), 즉 라이브러리를 만든 의도와 다르게 사용하는 방법이다.   
리액트는 앱 규모가 커지면 마치 데이터가 스파게티처럼 구조가 꼬여 유지보수가 힘들어지는점을 해결하기 위해 만들어졌다.  
그래서 처음에 리액트를 발표할때 강조한 부분이 단방향 데이터 통신인것이다.  
그래서 컴포넌트끼리 데이터를 교류할 때는 항상 컴포넌트 구조상 부모와 자식 사이에서만 데이터가 교류되어야 한다.
