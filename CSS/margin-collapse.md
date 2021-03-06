# margin collapse(마진 병합, 상쇄)
- 특정 margin값들이 서로 만났을때 중복 되어지는 현상이다. 중복이라고 해서 서로의 값이 합쳐지는것이 아닌 하나의 margin으로 적용된다.

- margin collapse는 버그나 오류가 아니다. IE8 이전 브라우저에서는 호환성 문제로 잘 작동하지 않는 사례도 있다고 하지만 레이아웃 흐름상 배치에 유리하게 하려는 CSS모델 중 하나이다.

## margin collapse, why?

- margin collapse는 위에서 말했듯 레이아웃의 흐름상 배치에 유리하게 하려는 CSS모델인데 사실 엄청 대단한 이유는 없다.<br> 이 포스팅을 쓰려고 margin collapse에 대한 대단한 이유를 찾으려고 했지만... 어떻게 쓰는지에 대한 내용만 많이 나와있고 why에 대한 내용도 많이 없었을뿐더러 그냥 block요소가 일반 흐름에 따라 수직 방향으로 배치될 때 균일한 간격을 유지하기 위해 마진이 병합되도록 CSS박스모델을 설계한것이라고 이해했다.

- 위의 말이 와닿지 않을수 있다. 단적이지만 실제 한가지 예를 들어보자면 `*`이나 `div`태그 선택자로 margin을 준다고 가정해보자. 예시로는 div태그 전체에 margin을 주겠다.
```html
<style>
  div {
    margin: 20px;
  }
</style>
<header>
  <div>box1</div>
  <div>box2</div>
  <div>box3</div>
  <div>box4</div>
</header>
```
위와 같은 경우 margin-collapsing(마진 병합)이 일어나지 않는다면 맨 처음과 마지막 div의 위아래 여백은 20px이지만 box2와 box3의 위아래 여백은 40px이 될것이다. 이러한 경우에 수직으로 정렬될시에 일정한 간격을 유지하기 위해 margin collapsing(마진 병합)이 일어난다고 생각하면 되겠다.

## margin collapsing(마진병합)이 일어나는 경우

- maingin collapsing이 일어나는 경우는 3가지가 있다.
- 기본적으로 margin collapsing은 인접한 두 요소가 block요소일 경우에만 적용이된다. 그러므로 아래의 설명은 모두 block요소들을 기준으로 보면 되겠다. (inline-block에서는 발생하지 않는다.)
- 좌우 margin은 병합되지 않는다.
- 빈 요소의 margin도 병합이 된다.(border나 padding등 중간 요소가 없을때)

### 1. 인접 형제 요소들의 상하 마진이 겹칠 때

```html
<style>
  .box1{
    margin: 30px;
  }
  .box2{
    margin : 20px;
  }
</style>

<div class="container">
  <div class="box1">margin값이 더 큰 box</div>
  <div class="box2">margin값이 더 작은 box</div>
</div>
```
- 위와 같은 경우 두개의 margin이 합쳐져서 50px이 되는것이 아니라 더 큰 margin값으로 상쇄가 된다.
- 두 요소의 margin이 같아도 합쳐지는게 아닌 하나의 값만 margin으로 적용이 된다.

### 2. 부모 요소와 자식 요소의 margin-top이 겹칠 때

- 부모요소에게 border나 padding 값이 없을때만 가능하다.(중간에 가로막는 속성이 없어야함.)
- 마찬가지로 중간에 다른 inline 요소가 들어오면 안된다.(부모요소에 직접적으로 text를 적는다던지)
- 이번에도 더 큰값으로 상쇄되지만 상쇄된 margin값은 무조건 부모요소의 바깥여백으로 적용이된다.
```html
<style>
  .container{
    margin: 50px;
  }
  .box1{
    margin : 30px;
  }
</style>

<div class="container">
  <div class="box1">부모요소의 margin-top과 만날 수 있는 자식요소</div>
  <div class="box2">두번째 요소</div>
</div>
```
- 위와 같은 경우 부모요소의 margin-top(50px)과 자식요소의 상단margin-top(30px)이 만나며 더 큰값인 50px로 상쇄된다.

### 3. 부모 요소와 자식 요소의 margin-bottom이 겹칠때(margin-top과 반대의 개념)

- 부모요소에게 border나 padding 값이 없을때만 가능하다.(중간에 가로막는 속성이 없어야함.)
- 마찬가지로 중간에 다른 inline 요소가 들어오면 안된다.(부모요소에 직접적으로 text를 적는다던지)
- 이번에도 더 큰값으로 상쇄되지만 상쇄된 margin값은 무조건 부모요소의 바깥여백으로 적용이된다.
```html
<style>
  .container{
    margin: 50px;
  }
  .box2{
    margin : 70px;
  }
</style>

<div class="container">
  <div class="box1">첫번째 요소</div>
  <div class="box2">부모요소의 margin-bottom과 만날 수 있는 자식요소</div>
</div>
```
- 위와 같은 경우 부모요소의 margin-bottom(50px)과 자식요소의 margin-bottom(70px)이 만나며 더 큰값인 70px로 상쇄된다. 
- 하지만 위에서 얘기한대로 자식요소의 margin값으로 상쇄돼도 실제로는 부모의 바깥여백으로 70px이 적용된것처럼 보인다.

- 2번과 3번같은 경우 부모요소에 border나 padding중 하나라도 값이 생기면 마진상쇄가 일어나지 않는다.

### 4. 빈 요소의 상하 margin이 인접요소의 margin과 겹칠 때

- 여기서 말하는 빈 요소란 높이(height)와 border, padding의 값이 0이고 내부에 inline콘텐츠 요소가 존재하지 않는 block요소를 말한다.

- 예를 들면 ::before나 ::after 가상요소 선택자로 `content: "";` 빈 요소를 만들어 준다거나 markup과정에서 나중에 요소안에 콘텐츠를 append 하려고 빈 요소를 만들어놓고 margin값이 있다면 그 margin-top과 bottom이 상쇄되는 개념이다.
```html
<style>
  .box1{
    margin: 80px;
  }
  .empty-box{
    margin-top: 30px;
    margin-bottom: 100px;
  }
  .box2{
    margin : 120px;
  }
</style>

<div class="container">
  <div class="box1">빈 요소의 위에 있는 요소</div>
  <div class="empty-box"></div>
  <div class="box2">빈 요소의 아래에 있는 요소</div>
</div>
```
- 위와 같은 경우 결국에는 3개의 margin값중 제일 큰 값으로 상쇄되어 적용이된다. 조금 복잡할 수 있으나 순서를 살펴보자.
1. 빈 요소의 margin-top과 margin-bottom의 값 중 더 큰값으로 상쇄된다. 위의 예제의 경우 100px로 상쇄
2. 상쇄된 100px을 위쪽으로 맞닿아있는 요소의 margin-bottom중 더 큰 값으로 다시 한번 상쇄한다. 위의 경우 80 < 100 이기 때문에 100px로 상쇄된다. 
3. 이번엔 100px과 아래쪽으로 맞닿아있는 요소의 margin-top의 값 중 더 큰 값으로 상쇄된다. 위의 예제같은 경우 100 <120 이기 때문에 결국 빈 요소를 사이에 두고 있는 box1 과 box2의 margin은 120px이 된다.

- 만약 빈 요소에 존재하는 margin값이 젤 클 경우는 빈 요소에 있는 margin으로 적용되니 결국은 이 경우엔 복잡할 수 있지만 빈 요소에 있는 margin값으로도 레이아웃이 결정될 수 있다는것이다.

## margin collapsing(마진 상쇄) 예외
- 특정 속성을 가진 요소들간의 마진 상쇄는 일어나지 않는다.
- `posion: absolute`속성이 적용된 요소
- `float: left/right`속성이 적용된 요소
- `display: flex`속성이 적용된 container내부의 items
- `display: grid`속성이 적용된 container내부의 items

## margin collapsing에 대해 포스팅하며 알아본 여러가지 이야기

- margin collapse는 까다로운 부분도 많고 이유에 대해 명확히 납득이 안되는 부분도 많아서 포스팅하는데 정말 많은 시간을 쓴 거 같은데... 어떤 외국인 블로그에서 본 글이다.

- CSS사양은 거대하니 모든것을 알려고하지 말라고 한다^^.. 대신 기본기를 탄탄하게 한 후 필요할때 빠르게 찾아서 쓸 수 있으면 스스로 쉽게 할 수 있을것이라고 하는데...정말 맞는말밖에 안해서 아무말도 못하겠...