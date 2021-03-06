# figure

## 사용법 및 정의

- figure는 그림, 도표, 사진, 코드 목록 등 독립적인 콘텐츠를 표현하는 태그이다.

- figure는 문서의 주 흐름과 관련은 있지만 주 흐름에서 제거되어도 문서의 흐름에 영향을 주지 않는 콘텐츠를 나눠서 사용 및 설명해줄때 사용합니다.

- 태그의 내부요소로 콘텐츠를 넣고 `<figcation>`요소를 사용해 설명을 붙혀줄 수 있다.

- `<figcation>`요소의 순서는 `<figure>`태그의 내부에만 있다면 콘텐츠의 앞 뒤로 위치는 상관없지만 한번만 사용할 수 있다.

- 콘텐츠로는 img, code, p, video등 여러가지 태그를 사용할 수 있다.

## figure의 기본 CSS속성
```css
figure{
  display: block;
  margin-top: 1em;
  margin-bottom: 1em;
  margin-left: 40px;
  margin-right: 40px;
}
```

## 사용예시

1. img figure

```html
<figure>
  <img src="./images/tiger.png" alt="동물원의 호랑이">
  <figcaption>동물원의 호랑이, 이 동물원의 마스코트로 알려져있다.</figcaption>
</figure>
```

2. video figure

```html
<figure>
  <figcaption>CSS의 float속성이 웹에서 작동하는 원리를 표현하는 3D영상</figcaption>
  <video src="./images/float_3D.mp4" controls>flaot속성의 3D 영상</video>
</figure>
```