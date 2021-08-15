# Javascript란

## 자바스크립트의 탄생

1996년 3월, 넷스케이프 내비게이터라는 브라우저에  최초 탑재되었다. 개발자 브렌던 아이크(Brendan Eich)에 의해 탄생되었고 처음부터 이름이 Javascript였던것은 아니다. 최초 모카(Mocha)라는 이름으로 명명되었고 같은 해 9월에 라이브스크립트(Livescript)로 바뀐 후 또 같은 해 12월 우리가 알고 있는 자바스크립트(Javascript)로 최종 결정되었다.

자바스크립트는 사실 웹페이지에서 아주 가벼운 보조적인 가능을 수행하기 위해 나온 언어였지만 현재는 프론트, 백 등 아주 다양한 플랫폼에서 적극적으로 사용되고 있고 모든 브라우저의 표준 프로그래밍 언어로 자리잡았다.

## 자바스크립트의 표준화

1996년 8월 즉, 아직 Javascript가 Mocha라는 이름으로 사용되고 있을때 마이크로소프트에서 Mocha의 파생버전으로 JScript를 IE3.0버전에 탑재하였다. 하지만 당시에는 표준이란게 존재하지 않았고 두 개의 언어는 각각 호환되지 않는 기능들이 많았고 각각의 브라우저 서비스를 운영하고 있었기 때문에 경쟁으로 인하여 호환성 문제는 더욱 고도화 되어갔다.

예를 들어 현재 우리가 익숙하게 사용하고 있는 addEventListener 함수를 IE9 이전의 버전에서의 사용까지 고려한다면 방어코드를 따로 작성해줘야 한다.

```javascript
// IE9 이상 및 표준을 준수한 브라우저
if ($button.addEventListener) {
  $button.addEventListener('click', () => console.log('click!'))
}
// IE9 미만 및 오페라 브라우저에서 사용 가능
else if ($button.attachEvent) {
  $button.attachEvent('onclick', () => console.log('click!'));
}
```

이러한 현상은 일관성을 무너뜨리고 개발자를 혼란스럽게 한다. 그렇기 때문에 모든 브라우저에서 정상적으로 동작되는 표준화된 Javascript가 필요했다. 그래서 1997년 7월 Javascript의 표준화를 담당하게 된 ECMA 인터내셔널의 이름을 따서 자바스크립트의 첫 표준인 ECMAScript1의 사양이 완성되었다. 그리고 2년 뒤인 1999년에 ECMAScript3가 공개되고, 한참 뒤인 10년만에 2019년에 ECMAScript5가 공개되었다.

그리고 현재도 Javascript 최신사양이라고 하면 얘기가 되는 ECMAScript6(ECMAScript2015)가 2015년에 기존의 Javascript의 문제점을 보완하며 범용 프로그래밍 언어로써 갖춰야 할 기능들을 대거 도입하며 많은 관심을 받기 시작했고 이 시점을 시작으로 1년 주기로 비교적 작은 기능을 추가하는 수준으로 매년 업데이트를 할 것이라고 예고되었다.

이 글을 쓰는 시점인 2021년 8월의 최신 ECMAScript사양은 ES2021이다.

- ES2021 추가 된 사양
    - String.prototype.replaceAll

        기존 String.prototype.replace는 특정 문자열을 모두 변경하려면 정규표현식을 사용했어야 했지만 replaceAll을 사용하면 기존 replace를 사용하는것처럼 특정 문자열을 모두 변경 할 수 있다.

        ```javascript
        const str = 'Hello World! Hello Javascript!';
        // 첫번째 Hello에만 영향을 줌
        str.replace('Hello', 'Hi'); // Hi World! Hello Javascript!
        // 기존 방식
        str.replace(/Hello/g, 'Hi'); // Hi World! Hi Javascript!
        // ES2021 추가된 replaceAll
        str.replaceAll('Hello', 'Hi'); // Hi World! Hi Javascript!
        ```

        더욱 더 명시적으로 함수명을 표현한것 같고 정규표현식을 사용하지 않아 더욱 간편해진것 같다.

    - Promise.any() + AggregateError

        Promise.any()는 프로미스를 요소로 가진 배열을 인자로 받고 배열의 요소중 가장 먼저 fulfilled되는 Promise를 resolve한다. 만약 모든 프로미스가 rejected됐을 경우에는 새로운 에러 유형으로 AggregateError를 일으킨다.

        예) 

        ```javascript
        const pErr = new Promise((resolve, reject) => {
          reject('Always fails');
        });

        Promise.any([pErr]).catch((err) => {
          console.log(err);
        })
        // expected output: "AggregateError: No Promise in Promise.any was resolved"
        ```

        Promise.race()는 가장 먼저 fulfilled되는것이 아니라 settled되는 요소를 반환하고 만약 하나라도 rejected된다면 rejected Promise를 반환한다는 차이점이 있다.

        정리하자면 Promise.any()는 fulfilled된 Promise만, Promise.race()는 settled된 모든 Promise를 취급한다는것이다.

    - WeakRef

        메모리에 요소에 대한 참조가 남아있는지 판단하기 위해 사용된다. 사용은 new 연산자와 함께 WeakRef를 호출하며 요소의 참조값을 넘겨주며 인스턴스를 생성하고 현재 구현되어있는 WeakRef.prototype으로는 WeakRef.prototype.deref()가 있다. 

        ```javascript
        class Counter {
          constructor(element) {
            // Remember a weak reference to the DOM element
            this.ref = new WeakRef(element);
            this.start();
          }

          start() {
            if (this.timer) {
              return;
            }

            this.count = 0;

            const tick = () => {
              // Get the element from the weak reference, if it still exists
              const element = this.ref.deref();
              if (element) {
                element.textContent = ++this.count;
              } else {
                // The element doesn't exist anymore
                console.log("The element is gone.");
                this.stop();
                this.ref = null;
              }
            };

            tick();
            this.timer = setInterval(tick, 1000);
          }

          stop() {
            if (this.timer) {
              clearInterval(this.timer);
              this.timer = 0;
            }
          }
        }

        const counter = new Counter(document.getElementById("counter"));
        setTimeout(() => {
          document.getElementById("counter").remove();
        }, 5000);
        ```

        mdn에서 제시한 예제를 가져와봤다. 간단히 보면 DOM에서 #counter의 참조값을 가져와 카운터를 start하고 5초뒤에는 #counter를 삭제시켜 가비지컬렉팅 되게끔 하여 start하고 5초 이내에는 tick함수 내부에서 첫번째 if문이 실행되게끔, 5초 뒤에는 else문이 실행되게끔 하는 코드다. 이렇게 가비지 컬렉팅이 된지 WeakRef.prototype.deref()를 통해서 알 수 있는 기능이다. (참조값이 존재한다면 그 참조값을 그대로 반환하고 가비지 컬렉팅 됐다면 undefined를 반환한다.)

        이것과 관련해서 요소가 가비지컬렉팅 되기전에 실행되는 콜백함수를 전달 할 수 있는 FinalizationRegistry도 존재한다.

        ```javascript
        const obj = { key: 'value' }
        const registry = new FinalizationRegistry((value) => {
          console.log(`${value} has been garbage collected`)
        })
        registry.register(obj, 'registered object')
        ```

    - Logical assignment operators (??=, &&=, ||=)

        +=, =+, /=. *= 처럼 비교 연산자도 사용할 수 있다. (아직 정식 출시는 아니라 web과 node에서 사용이 안되는것으로 확인)

        ```javascript
        const truthy = true;
        const falsy = false;

        let bool = false;

        bool &&= truthy; // bool = bool && truthy
        bool ||= falsy; // bool = bool || falsy
        bool ??= truthy; // bool = bool ?? truthy
        ```

    - Separators for numeric literals (1_000)

        number값을 사용할때 길이가 길어지면 구분하기 힘든 문제를 _를 쓰면 개발자가 구분하기 위한 용도로만 사용되게끔 하는 기능이다.

        ```javascript
        const number = 1_000_000_000;
        ```

    - + Array.prototype.sort의 개선

        내부 알고리즘이 더 효율적으로 변경되었다고 하는데 이 부분은 자세히는 모르겠다.

    참고 문서

    - [New Features in ECMAScript 2021](https://dev.to/faithfulojebiyi/new-features-in-ecmascript-2021-with-code-examples-302h)
    - [ECMAScript 2021 spec for JavaScript crosses the finish line](https://www.infoworld.com/article/3613948/ecmascript-2021-spec-for-javascript-crosses-the-finish-line.html)

## 자바스크립트 성장의 역사

앞에서 소개한대로 자바스크립트는 사실 웹페이지에서 가벼운 보조기능을 수행하기 위해 나왔었다. 그래서 초기 버전에서는 정말 서버로부터 전달받은 HTML과 CSS를 렌더링하는 수준이었다.

### Ajax

자바스크립트가 탄생하고 3년뒤인 1999년에 비동기적인 방식으로 데이터를 교환할 수 있는 통신 기능인 Ajax(Asynchronous Javascript And XML)가 XMLHttpRequest라는 이름으로 등장했다.

Ajax의 등장 이전에는 페이지에서 사소한 변경이 일어나더라도 <html>에서 </html> 태그로 구성된 완전한 HTML파일을 서버로부터 전송받아 웹페이지 전체를 다시 렌더링 해주는 방식으로 동작했다. 만약 페이스북 사용한다고 가정한다면 Ajax이전에는 좋아요 하나를 누르는것만으로 전체페이지가 다시 렌더링이 되어야했다는 소리다. 이는 불필요한 데이터의 통신 발생 및 성능, 사용자 경험 등 여러가지로 비효율적인 동작을 방식이었다.

Ajax는 위와같은 동작을 획기적으로 해결했다. 서버로부터 필요한 데이터만 전송받아 변경해야 하는 부분만 Javascript로 렌더링하는 방식을 가능하게 한 것이다.

이 Ajax를 증명한 계기는 아주 유명하다. 2005년 Google Maps에서 Ajax를 활용하여 웹 애플리케이션을 만들었고 불필요한 동작을 최소화하여 데스크톱 애플리케이션과 비교해도 손색이 없는 성능과 사용자 경험을 보여준것이다.

### jQuery

현재는 잘 사용하지 않는 아주 긴 시간동안 Javascript를 아주 편하게 쓸 수 있도록 만들어줬던 자바스크립트 라이브러리이다. 2006년에 등장했고 DOM을 아주 쉽게 제어하며 크로스 브라우징 이슈에 대한 해결 코드도 추상화하여 제공했기 때문에 아예 jQuery만을 배워 웹 개발을 하는 개발자도 있었다.

### V8 자바스크립트 엔진

각 브라우저별로 다른 자바스크립트이 탑재되어 있다. 위에 설명했던 Ajax방식으로 구현된 Google Maps를 계기로 더욱 빠른 자바스크립트 엔진으로 강력한 애플리케이션을 만드려는 시도가 있었고 그에 대한 결과물이 구글의 V8엔진이다. C++로 개발되었고 현재 대표적으로 Google브라우저와 Node.js에서 사용되고 있다.

더욱 빠르게 작동할 수 있는 자바스크립트 엔진을 기반으로 과거 웹 서버에서 수행되던 로직들이 대거 클라이언트로 이동했고 프론트엔드 영역의 영향력을 넓혀주며 웹 애플리케이션 개발을 빠르게 성장시키게 해주었다.

- 각 브라우저의 자바스크립트 엔진
    - Chrome: V8
    - FireFox: SpiderMonkey
    - Microsoft Edge: Chakra
    - Safari: JavaScript Core

### Node.js

Chrome에 사용됐던 V8 엔진을 이용해 2009년 라이언 달(Ryan Dahl)이 Javascript를 브라우저 이외의 독립적인 환경에서도 실행 가능하게 하는 Node.js를 발표한다.

Node.js는 다양한 플랫폼에 적용할 수 있지만 현재는 보통은 서버 사이드 애플리케이션 개발에 주로 사용되고 있다. 그렇기 때문에 Javascript는 하나의 언어를 배움으로써 프론트엔드, 백엔드 영역에서 사용할 수 있어 두 가지 모두 학습하려고 할때 새로운 언어에 대한 학습시간을 아낄 수 있다는 장점이 있다.

그래서 이제는 자바스크립트가 크로스 플랫폼을 위한 언어로도 주목받고 있다. 웹은 모두가 아는 사실이고 흔히 우리가 평소에 쓰는 데스크톱 애플리케이션인 Discord, Postman도 Electron.js로 개발한 네이티브 데스크톱 애플리케이션이다.

### SPA

웹 애플리케이션의 발전이 지속되면서 더 좋은 성능과 사용자 경험을 개발하고자 하는 움직임이 일어났고 당연하게 개발 규모와 복잡도가 상승했다.

이전의 개발 방식으로는 웹 애플리케이션의 규모가 올라가는것과 비례해서 복잡도가 리니어하게 상승하는것이 아닌 규모가 커질수록 복잡도는 겉잡을 수 없이 급상승하여 조금만 애플리케이션의 규모가 커져도 개발과 유지보수가 매우 힘들었다. 이러한 문제를 해결하기 위해 많은 패턴과 라이브러리가 만들어졌고 복잡도가 리니어하게 상승하는 경험을 할 수 있게 해주었다.

현재 컴포넌트 기반으로 SPA를 개발하는 프레임워크, 라이브러리로는 React, Vue, Angular가 대표적이고 그 이외에도 수 많은 프레임워크, 라이브러리가 생겨나고 사라지며 빠른 기술변화를 겪고 있다.