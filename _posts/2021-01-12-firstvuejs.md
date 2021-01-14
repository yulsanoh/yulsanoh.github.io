---
title:  "Vue.js의 기본적인 내용 정리"
excerpt: "Vue.js의 기본적인 내용을 정리하는 첫번째 포스트"

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - Vue.js
tags:
  - Vue.js
last_modified_at: 2021-01-12T08:06:00-05:00
---
<br>
# Vue.js에 대한 정리

## 기본적인 사용 방법

Vue.js 공식 사이트: https://kr.vuejs.org/v2/guide/index.html

위 사이트를 통해 Vue.js를 설치하거나 CDN을 불러와서 사용한다.

기본적으로 컴포넌트 형식으로 작성하게 되며, 기본 틀은 아래와 같다.

```html
<body>
  <div id="app">
  
  </div>

  <!-- Vue.js를 사용하기 위한 CDN 주소 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    let app = new Vue({
      el: "#app", // id값이 app인 태그가 부모가 되고 이 부분은 클래스도 가능하다, 자바스크립트로 따지자면 querySelector와 같은 기능
      data: {},
    });
  </script>
</body>
```

## 선언적 렌더링

Vue.js에서는 간단한 템플릿 구문으로 DOM에서 데이터를 바로 표시할 수 있다.

```html
<div id="app">
  { { message } }
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    message: "Hello Vue!",
  },
});
```

## v-bind

v-bind는 HTML의 엘리먼트 속성을 설정할 때 사용된다. 간단한 예제를 살펴보자

```html
<div id="app">
  <span v-bind:title="msg">마우스를 올려보세요</span>
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    msg: "title message",
  },
});
```

위와 같이 코드를 작성한 후 `<span>`태그 위에 마우스를 올리게 되면 `title`이 표시되는데, 내용은 Vue에서 관리하는 데이터 중 `msg`라는 변수의 내용이 출력되게 된다.

주의사항이 하나 있는데 추후에 나올 `v-model`과는 다르게 단방향으로만 연결되며 객체 내의 `data`에서 변경되는 것이 `v-bind`에 적용되고 `v-bind`에서 변경해도 `data`에는 접근할 수 없다.

## v-model

v-model은 `v-bind + 이벤트`와 같다고 보면 되며 사용자의 입력을 받는 UI 요소들에 사용한다 보통 Form 요소를 개발할 때 사용한다.

```html
<div id="app">
  { { msg } }
  <!-- msg 데이터가 변하는지 체크하기 위한 용도 -->
  <input type="text" v-model="msg" />
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    msg: "Hello Vue!",
  },
});
```

위와 같이 코드를 작성하고 확인해보면 입력 폼이 생겼을텐데 해당 입력 폼의 내용을 변경하게 된다면 data의 msg값도 변하게 된다. v-bind와 더불어 데이터를 연결하기에 `데이터 바인딩`이라고 칭함

## v-if

v-if는 보자마자 알 수 있듯이 if문과 동일한 효과를 지닌다 간단한 예제는 아래에

```html
<div id="app">
  <span v-if="seen">이제 나를 볼 수 있어요</span>
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    seen: true,
  },
});
```

`seen`이 `true`이기에 화면상에 출력되고 있는데, 콘솔창에서 `app.seen = false`를 입력하게 되면 `seen`이 `false`로 변하면서 출력되던 문구가 사라지게 된다.

## 객체

Vue의 data내에는 리터럴 값도 저장하지만, 당연하게도 객체도 저장이 가능하다.

```html
<div id="app">
  <div>human의 이름은 { { human.name } }이고, 나이는 { { human.age } }입니다.</div>
  <div>
    human1의 이름은 { { human2.name } }이고, 나이는 { { human2.age } }입니다.
  </div>
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    human: {
      name: 홍길동,
      age: 25,
    },
    human2: {
      name: 오율산,
      age: 28,
    },
  },
});
```

## v-for

v-for은 바닐라 자바스크립트의 for반복문과 동일하게 동작한다.

```html
<div id="app">
  <!-- item 이라는 변수에 list의 내용을 하나씩 주게된다. forEach와 동일하게 동작 -->
  <div v-for="item in list">{ { item } }</div>
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    list: ["홍길동", "오율산", "HTML", "CSS", "JS"],
  },
});
```

### 잠깐!

이 부분을 공부할 때 `v-for="(item, idx) in list"`와 같이 사용할 수 있다는 것을 알게되었는데 왜 배열을 매개변수로 받을 때 첫번째 매개변수가 `item`이고 두번째 매개변수가 `index` 인지 궁금해서 확인해서 매개변수를 두 개 더 추가해서 출력을 시도해봤는데 별 다른 내용은 출력되지 않았다.

`forEach`와 `v-for`을 둘 다 동일하게 `console.log`에 출력하기 위해서 Vue data에 메소드를 하나 추가했는데 다음과 같다.

```javascript
let app = new Vue({
  methods: {
    log(item, idx, c, d) {
      console.log(item, idx, c, d);
    },
  },
});
```

그리고 `v-for`가 쓰여진 `div`태그에 `v-bind:load="log(item, idx, c, d)"`로 콘솔에 출력을 시도해봤을 때, c와 d자리에 undefined가 출력되었다.

그리고 이 부분을 바닐라 자바스크립트에도 실행해 보았다 배열은 동일하게 출력하되 바닐라 자바스크립트에선 `forEach`를 사용해서 4개의 매개변수로 진행해 봤으며 콘솔창에 출력된 내용으로는 다음과 같다. (편의상 순서대로 a, b, c, d로 칭함)

a는 각 인덱스의 값, b는 각 인덱스의 번호, c는 해당 객체가 똑같이 복사되었고, d는 undefined가 출력되었다. `v-for과 forEach`는 살짝 다르게 동작하는 것 같다.

## v-on

v-on은 이벤트를 호출할 때 사용되며 사용되는 부분부터 먼저 확인해보자

```html
<div id="app">
  <input type="text" v-model="msg" />
  <button v-on:click="btnClick">확인</button>
</div>
```

```javascript
let app = new Vue({
  el: "#app",
  data: {
    msg: "",
  },
  methods: {
    btnClick() {
      alert(this.msg);
    },
  },
});
```

위의 코드에서 `input`창을 통해 사용자 입력을 받게 되며, 받은 데이터는 `v-model`에 의해 `msg` 데이터에 저장되게 된다. 그리고 확인 버튼을 클릭하게 되면 `v-on:click` 이벤트를 통해 입력한 `msg` 데이터의 내용이 알림창으로 뜨게 된다.
<br>
