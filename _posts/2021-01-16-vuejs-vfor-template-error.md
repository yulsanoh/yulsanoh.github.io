---
title:  "컴포넌트에 v-for 사용시 오류 해결법"
excerpt: "Vue.js의 컴포넌트에 v-for을 사용했을 때 발생하는 오류 해결법"

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - Vue.js
tags:
  - Vue.js
last_modified_at: 2021-01-16T08:06:00-05:00
---
<br>
# 컴포넌트 v-for 사용과 템플릿 범위에 따른 오류 정리

컴포넌트에서 v-for을 이용해서 데이터 바인딩을 하던 도중
<div>![에러화면 이미지](https://github.com/yulsanoh/yulsanoh.github.io/blob/main/_posts/images/2021-01-16-vuejs-vfor-template-error.png "에러화면 이미지")</div>

<div style="color: red;">'Error compiling template: Cannot use v-for on stateful component root element because it renders multiple elements.'</div>
<div style="color: red;">'Multiple root nodes returned from render function. Render function should return a single root node.'</div>
<br>
_위와 같은 오류가 발생해서 작성하는 포스트입니다. 만약 컴포넌트에 `v-for`을 사용했는데 해당 문구가 나타나면 아래의 포스트를 읽어주세요 :-)_

_해결 방법으로 바로 가고싶으시다면 [여기](# 데이터 바인딩과 v-for문)를 클릭해주세요_

Vue.js를 통해 컴포넌트를 만들어서 해당 컴포넌트 내에 데이터를 보내줄 때는 데이터를 어디에 받냐에 따라서 쓰는 `v-bind`를 바인딩 하는 방법이 달라지게 되는데 매우 기초적일 수 있으나 저처럼 프로그래밍에 발을 들인 지 얼마 되지 않은 입문 개발자들이 있을까 싶어서 정리하기 위해 작성하는 포스트입니다.

## 데이터 바인딩

지난 포스트에서 상위 컴포넌트의 데이터를 하위 컴포넌트로 전송하기 위해서는 하위 컴포넌트에는 `props`로 데이터를 받을 변수를 준비해야하고 해당 변수를 상위 컴포넌트에서 받아오기 위해 `v-bind`라는 키워드를 사용했었습니다.

이 말은 아래의 예제로 설명드리자면
```html
<!-- html -->
<div id="app">
    <comp-div></comp-div>
</div>
```
```javascript
// Vue

// 하위 컴포넌트
  Vue.component('comp-div', {
      template: `<div>{ { numList } }</div>`
  });

// 상위 컴포넌트
  let app = new Vue({
      el: '#app',
      data: {
          numList = [1, 2, 3];
      }
  });
```
위와 같이 코드를 작성하시면 하위 컴포넌트 내에 `numList`라는 것은 선언되어 있지 않기 때문에 컴포넌트로 인해 템플릿이 만들어 질 때 `div`태그 내에 `numList`는 선언되지 않았다는 오류가 발생하게 됩니다.

이 부분은 하위 컴포넌트에서는 직접적으로 데이터에 접근하지 못하고 상위 컴포넌트에서 받아와서 사용해야 합니다.

아래의 코드를 살펴보세요.
```html
<!-- html -->
<div id="app">
    <!-- 2. v-bind를 통해 상위 컴포넌트 데이터인 numList의 값을 하위 컴포넌트인 list에 복사 -->
    <comp-div v-bind:list="numList"></comp-div>
</div>
```
```javascript
// Vue

// 하위 컴포넌트
  Vue.component('comp-div', {
    // 1. props로 상위 컴포넌트의 데이터를 받을 변수 'list'를 선언
      props: ['list'],
      // 3. 복사된 값을 사용
      template: `<div>{ { list } }</div>`
  });

// 상위 컴포넌트
  let app = new Vue({
      el: '#app',
      data: {
          numList = [1, 2, 3];
      }
  });
```
이런 방식을 통해 상위 컴포넌트의 데이터를 불러와서 사용하게 됩니다.

하지만 배열의 내용이 리터럴 값이 아닌 객체가 되게 된다면 `v-for`문을 사용해서 각 인덱스에 저장된 객체 하나하나씩을 불러와야 하는 경우가 생기게 되는데 `v-bind`와 `v-for`을 어디에 쓰냐에 따라 오류 발생 여부가 결정되게 되는데 계속해서 말씀드리겠습니다.

## 데이터 바인딩과 v-for문

두가지 경우가 있기 때문에 첫번째, 그리고 두번째로 나누어서 정리하겠습니다.

### 첫번째 방식
첫번째 방식은 컴포넌트 템플릿이 최상위 요소 하나만 존재하고, 해당 요소에 자료를 받아오기 위해서 `v-for`을 사용한다면 `v-for`의 위치는 템플릿이 아닌 컴포넌트 태그입니다.
  
`v-for`을 통해 먼저 상위 컴포넌트 데이터에서 데이터를 받아온 뒤, 받아온 데이터를 `v-bind`를 통해 `props`의 변수에 저장해야 합니다.

```html
<!-- html -->
<div id="app">
    <comp-div v-for="item in humanList" v-bind:list="item"></comp-div>
</div>
```
```javascript
// Vue

// 하위 컴포넌트
  Vue.component('comp-div', {
      props: ['list'],
	  // 템플릿이 최상위 코드로만 구성되어 있는 경우
      template: `<div>{{ list }}</div>`
  });

// 상위 컴포넌트
  let app = new Vue({
      el: '#app',
      data: {
          humanList = [
            {
              name: '홍길동',
              age: 20
            },
            {
              name: '오율산',
              age: 28
            }
          ];
      }
  });
```
위와 같이 작성하게 되면 `v-for`문을 `<comp-div>`에서 동작하게 되면서 결론적으로 `<comp-div>`가 계속 생기게 된다고 보시면 됩니다. 그렇기 때문에 `template`의 최상단에는 하나의 요소만 존재해야 한다는 것이 유지되면서 오류가 나지 않습니다.

이해를 돕기위한 코드
```html
<!-- html -->
<div id="app">
    <!-- <comp-div v-for="item in humunList" v-bind:list="item"></comp-div> -->
    <comp-div v-bind:list="item[0]"></comp-div>
    <comp-div v-bind:list="item[1]"></comp-div>
    <!-- 아이템의 갯수만큼 <comp-div>가 생성되게 되고, <comp-div>는 현재 template에 의해 <div>{ { list } }</div>로 변환되게 됨 -->
</div>
```

### 두번째 방식

두번째 방식은 컴포넌트 템플릿 내에 최상위 템플릿이 아닌 하위 템플릿에 자료를 받아오기 위해 `v-for`을 사용한다면 `v-for`의 위치는 컴포넌트 태그가 아닌 자료를 받아올 하위 템플릿입니다.

이 때는 `v-bind`를 통해 `props`에 선언된 변수로 상위 컴포넌트의 데이터를 복사한 후 하위 템플릿에 `v-for`문을 이용해서 해당 자료의 값들을 불러오면 됩니다.

```html
<!-- html -->
<div id="app">
    <comp-div v-bind:list="humanList"></comp-div>
</div>
```
```javascript
// Vue

// 하위 컴포넌트
  Vue.component('comp-div', {
      props: ['list'],
	  // 템플릿이 최상위 코드와 여러 줄의 하위 코드들로 구성되어 있는 경우
      template: `<div>
                    <div v-for="item in list">{{ item }}</div>
                 </div>`
  });

// 상위 컴포넌트
  let app = new Vue({
      el: '#app',
      data: {
          humanList = [
            {
              name: '홍길동',
              age: 20
            },
            {
              name: '오율산',
              age: 28
            }
          ];
      }
  });
```
위와 같이 작성하게 되면 `<comp-div>`가 `template`에 의해 아래와 같이 변하게 되는데
```html
<!-- html -->
<!-- <comp-div></comp-div>가 템플릿에 의해 아래와 같이 변환 됨 -->
<div>
  <div v-for="item in list">{ { item } }</div>
</div>
```
이렇게 되면 최상위 루트에는 하나의 요소만 존재하게 되고 요소 내에서 `v-for`이 동작하면서 여러개의 `div` 요소를 생성해내면서 `item`들을 뿌려주게 됩니다.

## 결론
컴포넌트 내에 템플릿 요소가 존재하고 템플릿 요소에 자료를 받아오기 위해 `v-for`을 사용한다고 했을 때, 만약 템플릿이 최상위 요소 하나만을 가진다면 `v-for`은 컴포넌트를 사용하는 태그에 사용해야 하고, 템플릿이 최상위 요소가 아닌 하위 요소에서 데이터를 받기 위해 `v-for`을 사용해야 한다면 컴포넌트를 사용하는 태그에는 `v-bind`를 통해 데이터를 `props`에 선언된 변수로 받고, 해당 변수를 통해 하위 요소들에 `v-for`을 사용해서 자료를 받아오시면 됩니다.

이로써 Vue.js의 컴포넌트를 사용할 때 `v-for`의 위치에 따라 오류가 발생하는 부분을 정리해봤습니다.

지극히 초보 개발자 입장에서 쓴 포스트이며 잘못쓰여진 부분이 있거나 수정이 필요한 부분이 있다면 댓글이나 이메일로 연락주시면 감사하겠습니다 :-)
