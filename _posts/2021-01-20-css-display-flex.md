---
title:  "[CSS] display: flex; 요소에 대해서 1편"
excerpt: "CSS의 display: flex 요소의 정리, flex-container의 속성까지"

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - CSS
tags:
  - CSS
last_modified_at: 2021-01-20T08:06:00-05:00
---
<br>
# 📚 display: flex;
CSS의 속성 중 display는 요소를 어떻게 보여줄 지 결정하는 속성이다. 주로 사용되는 속성의 값은 `block`, `inline`, `inline-block`, `none`이 있는데 오늘 정리해볼 내용은 `flex`에 대한 내용이다.

레이아웃을 구성할 때 여러 속성들을 병합해서 사용하는 것들을 배웠는데 (예를들면 `float`과 `inline-block`)

`display: flex;`를 사용했을 때, 이건 진짜 너무 간편한 속성이라고 바로 깨달을 만큼 레이아웃을 쉽게 구성할 수 있게 해주는 속성이었다.

하지만 사용법을 드문드문 알고있어서 이 기회에 제대로 된 사용법을 정리하고자 포스트를 작성하게 되었다.

## flex 레이아웃 기본 구조
```html
<!-- html -->
<div class="container">
    <div class="container-item"></div>
    <div class="container-item"></div>
    <div class="container-item"></div>
</div>
```
flex 요소를 사용하기 위한 정말 기본적인 구성이다.

`부모 요소`인 `div.container`를 flex container(플렉스 컨테이너)라고 칭하고,
`자식 요소`인 `div.container-item`들을 flex item(플렉스 아이템)이라고 칭한다.

![css-display-flex-basic](https://user-images.githubusercontent.com/58783926/105153061-5617b080-5b4b-11eb-862c-eeae819954c7.jpg)

_~~뭔가 간격이 맞지 않는 것 같지만 넘어가자~~_

위의 그림에서는 하늘색의 큰 박스가 `div.container`이 되고 빨간색 박스가 `div.container-item`이 되는 것이다!

그리고 `flex` 속성에는 `부모 요소`인 `div.container`에 적용되는 속성과 `자식 요소`인 `div.container-item`에 적용되는 속성이 각각 다르다.

## 부모 요소인 flex container에 적용되는 속성
flex container, 위의 코드에선 `div.container`에 해당하는 요소에 적용되는 속성들을 살펴보자

### display: flex; (flex container 정의)
flex 속성을 사용하기 위한 시작을 알리는 코드라고 할 수 있다. 해당 코드를 적용하게 되면 요소의 display는 flex로 변하게 되고 정렬이 시작되게 되는데 각 flex item(자식요소)들은 내부에 작성된 내용 만큼 width와 height가 정해지고 서로의 컨텐츠 크기를 기준으로 영역을 차지하게 된다.

![css-display-flex-scope](https://user-images.githubusercontent.com/58783926/105153084-5ca62800-5b4b-11eb-8f43-fd2072425d52.jpg)

여기서 height의 경우 각각의 flex item에 영향을 받는게 아니라 flex item들 중 가장 높은 요소의 영향을 받게 되는데 아래의 그림을 살펴보자

![css-display-flex-height](https://user-images.githubusercontent.com/58783926/105153117-63cd3600-5b4b-11eb-9e57-0041f4c95edb.jpg)

### display: inline-flex; (flex container 정의)
`display: flex;`가 `display: block;` 요소와 같다면, `display: inline-flex`는 `display: inline-block`과 같다.

![css-display-flex](https://user-images.githubusercontent.com/58783926/105153144-6c257100-5b4b-11eb-9d4a-a638bf88be2b.jpg)

* `display: flex;` 요소

![css-display-inline-flex](https://user-images.githubusercontent.com/58783926/105153166-75aed900-5b4b-11eb-823e-f76cf5cc8467.jpg)

* `display: inline-flex;` 요소

정리하자면 `display: flex;`는 `display: block;`과 같이 수직 쌓음을, `display: inline-flex`는 `display: inline-block`과 같이 수평 쌓음을 하게 된다.

<div class="notice--danger">flex와 inline-flex 모두 내부 내용(flex item)이 아닌 컨테이너(flex container)끼리의 수평, 수직 쌓음을 설정하는 것임을 유의하자</div>

### flex-direction (배치 방향 설정)
`flex-direction`은 주 축(main-axis)의 방향을 설정하는 것이다. `가로`, `세로`, `가로-역순`, `세로-역순`의 값이 존재한다.

<div class="notice--danger">'-reverse'가 붙게되면 flex-item들의 순서도 변하게 됩니다.</div>

![css-display-flex-direction-row](https://user-images.githubusercontent.com/58783926/105153187-7e9faa80-5b4b-11eb-87e9-11600ad5ca2a.jpg)

* `flex-direction: row;` 가로인 상태(기본 값)

![css-display-flex-direction-row-reverse](https://user-images.githubusercontent.com/58783926/105153217-85c6b880-5b4b-11eb-887a-0704c72b0175.jpg)

* `flex-direction: row-reverse;` 가로-역순인 상태

![css-display-flex-direction-column](https://user-images.githubusercontent.com/58783926/105153534-e6ee8c00-5b4b-11eb-952a-544a920a6928.jpg)

* `flex-direction: column;` 세로인 상태

![css-display-flex-direction-column-reverse](https://user-images.githubusercontent.com/58783926/105153831-3cc33400-5b4c-11eb-9a1a-05b0db90445d.jpg)

* `flex-direction-column-reverse;` 세로-역순인 상태

row와 column이 많이 헷갈리는데 저는 row를 가-row로 외웠습니다.......... 죄송

#### 주 축(main-axis), 교차 축(cross-axis)
flex-direction을 기준으로 주 축(main-axis)과 교차 축(cross-axis)이 변하게 되는데 그림과 같이 알아보면 다음과 같다.

![css-display-flex-direction-axis-1](https://user-images.githubusercontent.com/58783926/105157298-2dde8080-5b50-11eb-8d1f-207ef534d811.jpg)

* `flex-direction`이 `row`일 때

![css-display-flex-direction-axis-2](https://user-images.githubusercontent.com/58783926/105157301-2e771700-5b50-11eb-9aaf-b5236a644df0.jpg)

* `flex-direction`이 `column`일 때

쉽게 생각하면 주 축(main-axis)은 `flex-direction`의 방향이고 교차 축(cross-axis)은 주 축을 가로지르는 축이다 :-)

### flex-wrap (줄 바꿈)
`display: flex;`가 적용된 flex-container 안의 자식요소인 flex-item들은 기본적으로 한 줄에서만 표시가 되고 줄 바꿈이 일어나지 않는다.

이 부분은 각 flex-item들의 크기를 무시하고 한 줄에 맞춰서 크기를 맞추기 때문에 레이아웃이 찌그러지게 되는데 flex-item들의 크기를 맞추면서 레이아웃을 구성하기 위해 사용하는 속성이다.

![css-display-flex-nowrap](https://user-images.githubusercontent.com/58783926/105159040-fd97e180-5b51-11eb-958a-46713155fafa.jpg)

* `flex-wrap`이 `nowrap`인 상태

![css-display-flex-wrap](https://user-images.githubusercontent.com/58783926/105159287-418ae680-5b52-11eb-984c-106202319df0.jpg)

* `flex-wrap`이 `wrap`인 상태

![css-display-flex-wrap-reverse](https://user-images.githubusercontent.com/58783926/105159527-8878dc00-5b52-11eb-8523-a8def048aa50.jpg)

* `flex-wrap`이 `wrap-reverse`인 상태

### justify-content
`justify-content`는 주 축(main-axis)를 기준으로 flex-item들을 정렬하는 것을 정의하는 속성이다.

_(flex-wrap 이미지의 모든 요소들의 `margin`과 `padding` 요소를 0으로 맞추었습니다.)_

![css-display-flex-justify-content-flex-start](https://user-images.githubusercontent.com/58783926/105167001-810a0080-5b5b-11eb-8496-9a1df40069c3.jpg)

* `justify-content`가 `flex-start`인 상태

![css-display-flex-justify-content-flex-end](https://user-images.githubusercontent.com/58783926/105167004-81a29700-5b5b-11eb-9a81-fcb60d2f82ee.jpg)

* `justify-content`가 `flex-end`인 상태

![css-display-flex-justify-content-center](https://user-images.githubusercontent.com/58783926/105167009-823b2d80-5b5b-11eb-8696-d63b4cdaf734.jpg)

* `justify-content`가 `center`인 상태

![css-display-flex-justify-content-space-around](https://user-images.githubusercontent.com/58783926/105167011-823b2d80-5b5b-11eb-936a-b5d1e215e87d.jpg)

* `justify-content`가 `space-around`인 상태

![css-display-flex-justify-content-space-between](https://user-images.githubusercontent.com/58783926/105167017-849d8780-5b5b-11eb-9f3b-9cf0db496d91.jpg)

* `justify-content`가 `space-between`인 상태

`justify-content: flex-end;`와 `flex-direction: row-reverse;`의 차이점은 컨텐츠의 순서까지 변경되는지 여부이다. 전자는 컨텐츠의 순서가 변경되지 않지만, 후자는 컨텐츠의 순서까지 변경된다.

그리고 `space-around`와 `space-between` 값인 이미지들의 각각의 간격들은 모두 동일하다.

### align-content
21일 추가 예정
