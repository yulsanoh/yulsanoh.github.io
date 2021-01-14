---
title:  "Vue.js의 컴포넌트"
excerpt: "Vue.js의 컴포넌트와 관련된 내용을 정리한 포스트"

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - Vue.js
tags:
  - Vue.js
last_modified_at: 2021-01-13T08:06:00-05:00
---
<br>
# Vue.js에 대한 정리

## 컴포넌트
> 컴포넌트는 Vue의 강력한 기능 중 하나입니다. _Vue.js 공식사이트_

컴포넌트는 HTML 요소를 캡슐화시켜서 재사용하기 좋게 만들어주는 것이라고 생각하면 되는데, 가령 Youtube와 같이 똑같은 모양의 양식을 여러개 만들어야 할 때 하나씩 태그들로 만들지 않고 양식을 만들어 두어서 필요할 때 꺼내서 쓰는 것과 같다.

```javascript
  Vue.component('name', {
      // 옵션
  })
```
위와 같이 사용하게 되며 간단한 예제는 아래와 같다.

```html
<div id="app">
    <my-component></my-component>
</div>
```
```javascript
  Vue.component('my-component', {
      template: '<div>사용자 정의의 컴포넌트</div>'
  })

  let app = new Vue({
      el: '#app',
      data: {

      }
  })
```

작성하게 되면 `div#app` 아래에 `Vue.component`의 `template`에 지정한 내용이 들어가게 된다.

```html
<div id="app">
    <div>사용자 정의의 컴포넌트</div>
</div>
```
이렇게 자동으로 입력한 양식으로 변환되게 된다.

여기서 __한가지 알아둬야 할 부분__ 은 `div#app`이 부모태그고 컴포넌트는 최상단 구조에 무조건 하나의 태그만 가능하다. 그 아래 후손 태그들의 갯수는 상관없다. 이게 무슨 말이냐면

```javascript
  Vue.component('my-component', {
      template: `<div>사용자 정의의 컴포넌트</div><div>최상단 태그의 형제 태그</div>`
  })
```
이렇게 작성하면 오류가 발생하며
```javascript
  Vue.component('my-component', {
      template: `<div>사용자 정의의 컴포넌트
            <div>최상단 태그의 후손 태그</div>
      </div>`
  })
```
위와 같이 최상단 태그는 하나만 작성하고 아래 후손 태그들을 두어야 한다.

그리고 부모 태그인 `div#app`은 자료를 주는 역할이고 하단 컴포넌트들은 자료를 받는 역할을 하게 된다. (단방향 방식)

## Props
컴포넌트를 사용해서 템플릿을 구현했을 때, 원하는 데이터를 불러오고 싶은 경우가 생길 것이다. 이럴때는 `Props`를 사용해서 불러오게 되는데 알아두어야 할 것이 하나 있다.

모든 컴포넌트 인스턴스는 자체 스코프가 존재하는데, Vue.js 공식사이트에서는 하위 컴포넌트 템플릿에서 상위 데이터를 직접 참조할 수 없고 그렇게 해서도 안된다고 적혀있다.

데이터는 `props` 옵션을 이용해서 상위 데이터를 하위 컴포넌트로 전달할 수 있다.

`props`를 주게되면 `child`라는 컴포넌트 내에 `message`라는 변수가 생겼다고 생각하면 된다.

```javascript
  Vue.component('child', {
    // props 정의
    props: ['message'],
    // 데이터와 마찬가지로 prop은 템플릿 내부에서 사용할 수 있음
    template: `<span>{ { message } }</span>`
  })
```
그리고 이렇게 `props`를 정의한 후에 데이터를 전달하려면 데이터를 전달해주면 된다.

```html
  <child message="Hello Vue!"></child>
```

### 동적으로 데이터 보내기
정규 속성을 표현식에 바인딩하는 것과 비슷하게 `v-bind`를 사용해서 부모의 데이터를 불러올 수 있는데 부모의 데이터가 변하게 되면 실시간으로 하위 데이터로 전송된다.

```html
<div id="app">
  <input type="text" v-model="msg">
  <!--
    v-bind를 통해 my-message 안에 상위 데이터인 msg를 담게되고 템플릿이 생길 때
    템플릿 내의 { { my-message } }에 들어가게 됨
  -->
  <my-child v-bind:my-message="msg"></my-child>
</div>
```
```javascript
  Vue.component('my-child', {
    props: ['my-message'],  // 'my-child' 컴포넌트 내에 'my-message'라는 변수가 생김
    template: `<span>{ { my-message } }</span>`
  })

  let app = new Vue({
    el: '#app',
    data: {
      msg: 'Hello Vue!'
    }
  })
```

### 정리
`my-child`라는 컴포넌트를 정의하고 구조를 짠 다음에 컴포넌트 내에 `props`로 변수를 생성하고 생성된 변수에 값을 부여하는 방법은, HTML에서 `v-bind`를 통해 동일한 이름의 속성을 부여하고 거기에 상위 데이터의 `key`값을 부여하면 된다.

여기서 중요한 점은 `컴포넌트 템플릿`에 값을 불러오기 위해서는 `props` 변수가 컴포넌트 내에 선언되어 있어야하며, 생성자 함수(상위 데이터)의 데이터에 접근하기 위해서는 `v-bind:컴포넌트 변수명=값`을 사용하면 된다.

예시)
상위 데이터에 `msg: 'hello'`가 선언되어 있고 `'hello'`를 출력하기 위해 상위 데이터에 접근해야 한다면
```javascript
<my-child v-bind:컴포넌트 내의 props 변수명="msg"></my-child>
```
위와 같이 사용한다.

## 배열 데이터 호출하기
상위 데이터에 배열로 된 데이터가 존재하고 배열 내의 데이터를 하위 컴포넌트에서 호출하기 위해서는 일반적으로 인덱스 번호로 호출하는 방법과 동일하게 호출하면 된다.

```html
<my-child v-bind:data="list[0]"></my-child>
<my-child v-bind:data="list[1]"></my-child>
```
```javascript
  Vue.component('my-child', {
    props: ['data'],
    template: `<div>{ { data } }</div>`
    // 상위 데이터의 객체 값 중 name 값만 불러오고 싶다면 data.name과 같이 사용한다.
  })

  let app = new Vue({
    // 부모 태그(el: ) 생략
    data: {
      list: [
        {
          name: '홍길동',
          age: 20
        },
        {
          name: '오율산',
          age: 28
        }
      ]
    }
  })
```

## 간단한 연습문제
여기까지 배운 내용으로 간단하게 사용자 입력 폼을 생성하고 해당 폼에 작성되는 내용들을 키보드의 ENTER 키, 혹은 입력 버튼을 이용해서 상위 데이터에 접근해서 저장하고 저장된 데이터들을 하단에 출력하는 간단한 웹을 제작해보았다.

해당 자료의 링크는 다음과 같다.

[해당 자료의 링크 :-)](https://github.com/yulsanoh/yulsanoh/blob/main/exercise/vuejs/pushName.html)