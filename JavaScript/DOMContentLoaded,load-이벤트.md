# DOMContentLoaded, load 이벤트

HTML 문서의 생명주기엔 3가지의 주요 이벤트가 관여한다. 그 중 DOMContentLoaded와 load 이벤트에 대해 알아보자.

## DOMContentLoaded
- 브라우저가 초기 HTML 문서를 완전히 불러온 후 파싱하여 DOM tree가 완성됐을때 발생한다.
- stylesheet, image, 하위 프레임 등의 기타 자원의 로딩은 기다리지 않는다.

주로 DOM tree가 완성된 것을 확인 한 후 원하는 DOM 노드를 찾아야할때 사용한다.

DOMContentLoaded를 사용하지 않고 script태그가 찾으려는 DOM 노드보다 상위에 있을경우 아직 DOMtree에 찾으려는 노드가 생성되지 않았기 때문에 null이 할당되게 되고 예상치 못한 에러를 발생시킬 가능성이 있다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<script>
  const $headerHeading = document.querySelector('.header-heading');
  console.log($headerHeading); // null
</script>
<body>
  <header>
    <h1 class="header-heading">DOMContentLoaded를 사용하지 않았을때</h1>
  </header>
</body>
</html>
```

DOMContentLoaded 이벤트의 이벤트 핸들러에 위의 코드를 작성해주면 해결된다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const $headerHeading = document.querySelector('.header-heading');
    console.log($headerHeading); // <h1 class="header-heading">DOMContentLoaded를 사용했을때</h1>
  });
</script>
<body>
  <header>
    <h1 class="header-heading">DOMContentLoaded를 사용했을때</h1>
  </header>
</body>
</html>
```

위의 예제같은 경우에는 js파일을 따로 만든 후 script태그의 src속성으로 로드하며 defer속성을 적어주면 해결이 되긴한다.
defer 속성이 없을때는 script태그를 닫히는 body태그의 바로 위 또는 아래에 위치시켜서 해결하기도 했다.

## load
- 브라우저가 초기 HTML 문서를 완전히 불러온 후 파싱하여 DOM tree가 완성된것 뿐 아니라 stylesheet, image, 하위 프레임 등의 기타 자원(리소스)이 모두 로드 됐을때 발생한다.

외부 자원(리소스)이 모두 로드된 이후에 발생하는 이벤트이기 때문에 주로 이미지 사이즈를 확인하거나 화면에 뿌려지는 요소들의 스타일링을 가져오는 등의 목적으로 사용된다.

load이벤트를 사용하지 않고 DOMContentLoaded를 사용했을때 DOM요소는 정상적으로 불러올 수 있지만 아직 로드되지 않는 리소스에 대한 정보는 가져올 수 없다. 로드해야 하는 파일의 용량이 적거나 인터넷 환경이 원활하다면 이 경우에도 정상적으로 동작하겠지만 로드해야 하는 파일의 용량이 크고 사용자의 인터넷 환경이 원활하지 않다면 아래 예제와 같이 0이라는 결과가 나온다. 이 예제의 이미지는 로딩이 상당히 오래걸리는 리소스를 사용하여 대부분 정상적으로 작동하지 않을것이지만 이것도 사용자에 따라 정상적으로 작동할 수도 있을것이다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const $img = document.querySelector('img');
    console.log($img.offsetWidth); // 0
  });
</script>
<body>
  <main>
    <img src="https://en.js.cx/clipart/train.gif?speed=1&cache=0" alt="JS icon">
  </main>
</body>
</html>
```

load 이벤트를 사용하면 위 예제의 문제점을 해결할 수 있다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<script>
  window.onload = () => {
    const $img = document.querySelector('img');
    console.log($img); // <img src="https://en.js.cx/clipart/train.gif?speed=1&amp;cache=0" alt="JS icon">
    console.log($img.offsetWidth); // 177
  };
</script>
<body>
  <main>
    <img src="https://en.js.cx/clipart/train.gif?speed=1&cache=0" alt="JS icon">
  </main>
</body>
</html>
```