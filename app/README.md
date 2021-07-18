# app

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

# VueJsCrashCourse2021
For study

NOTION URL : https://www.notion.so/Vuex-c8578e7e5cf345f2b42fff3e1c803306

## 1. Watch 속성
- watch 속성은 감시하는 역할로 데이터가 변경되었을 때, 실행되는 속성이다.
- watch 속성은 체크하고싶은 데이터를 명시하고 해당 데이터를 함수로하여 (value, oldValue) 와 같은 형태로 실행하고 싶은 내용을 명시하면 된다.
- oldValue 의 경우, 해당 체크하고 있던 데이터의 변경 전 값을 나타나게 된다.
- value 나 oldValue 의 경우 객체나 배열의 경우, 이전 값과 현재 값이 같은 값이 나올 수 있기 때문에 primitive 타입만 써주는 것이 좋다.
- watch 속성의 경우, 잘못 거는 경우 무한 루프를 탈 수 있으며 속성 간 watch 관계가 얽히는 경우 관리가 힘들기 때문에 가능하면 쓰지 않는 것이 좋다.
- computed 는 새로운 값을 리턴하고, watch 는 특정 동작을 수행한다.

## 2. this.$root, this.$parent
- 일반적인 Vue 에서는 3,3 의 2차원 배열을 Vue 로 표현하기 위해 Vue 를 계층적 구조로 만들어야 한다.
- 하위의 컴포넌트까지 전달하기 위해 자식으로 계속해서 props 를 흘려주고 최하층에서 컨트롤 해야하는 식이다.
- 이러한 번거로움을 해결하기 위해서 this.$root.$data 와 같은 형태로 최상층 데이터에 접근할 수 있다.
- 또한, this.$parent.$data 와 같은 방법으로 부모 Vue 의 데이터에도 접근 가능하다.
- 만약 해당 데이터가 배여링라면 최하층의 index 를 props 로 받아서 해당 루트 데이터의 배열에 있는 본인의 데이터를 건드릴 수 있다.
- 그러나 Vue 에서 배열의 인덱스를 지정하여 값을 바꾸는 경우에 화면이 갱신되지 않는다.
  ### Why ? Vue 의 근본인 Javascript 의 한계 때문이다.
        - 배열의 메소드를 이용하여 (push 등) 데이터를 조작하는 경우에는 화면이 갱신된다.
        - 해결방법은 ? 1) import Vue 를 하여서 Vue.set 을 통해 바꾸고싶은 값의 key 와 value 를 전달하여 조작하는 방법이 있다.
                      2) Vue 를 불러오지 않고, this.$set 을 이용하여 위처럼 key 와 value 를 전달해주면 된다.

## 3. Event Bus
- 일종의 이벤트 중앙 매개체로, 이벤트를 한번에 몰아서 관리하기 위한 시스템이다.
- import Vue 키워드를 통해 가져와서 export new Vue 만 하는 빈 js 파일을 만든다.
- 사용하려는 Vue 에서 import 하여 가져온 후, created 부분 등에서 $on 을 통해 커스텀 이벤트로 등록한다.
- 그 후 통신하려는 Vue 에서 $emit 을 사용하여 이벤트를 발생시킨다.

## 4. Vuex
- Vue 의 상태를 관리하기 위한 중앙통제 관리 시스템이다. Vuex.store() 에 하기 상태값들을 넣는다.
- state 는 vue 의 data 와 비슷하다.
- getter 는 vue 의 computed 와 비슷하다.
- mutations 는 state 를 동기적으로 수정할 때 사용한다.
- actions 는 비동기를 사용하거나 여러 뮤테이션을 연달아 실행할 때 사용한다.

  ### mutations
  - mutations 는 대문자의 함수로 만든다.
  - 변수로 이름을 선언하여 export [변수명] 의 형태로 사용하는 것이 편하다.

- Vuex 에서도 배열을 인덱스로 접근하는 경우에 Vue.set 을 사용해야 화면이 갱신된다.
- this.$store.commit(변수명) 을 이용하여 뮤테이션을 실행할 수 있다.
- 두번째 인수로 인자를 넘겨주어서 사용할 수 있다.

참고) export default 와 달리 export 와 같은 형태로 하면 {} 와 같은 형태로 가져와야 한다.

- computed 속성을 사용하여 vuex 의 상태값을 가져올 수 있다.
- 예를 들면 this.$store.state.tableData 와 같은 형태로 가져오는 것이다.

  ### Vue 와 Vuex 의 연결
  - Vue.use(Vuex) 를 통해 Vue 와 Vuex 를 연결해주어야 한다.
  - 최상위 컴포넌트에서 import 한 후 export store 와 같은 형태로 연결해주어야 사용할 수 있다.

  ### mapState
  - Vuex 의 상태값을 가져오기 위해서는 computed 속성에 하나하나 다 작성해야 하는 번거로움이 있다.
  - 따라서, mapState 를 사용하여 state 값들을 가져올 수 있다.
  - import { mapState } from 'vuex' 와 같은 형태로 import 해온다.
  - computed 속성에 ...mapState(['상태값', ...]) 과 같은 형태로 가져올 수 있다.
  - 혹은 상태값 : state => state.변수명 과 같은 방법으로도 가져올 수 있다.
  - 일반함수와 키, '변수명' 과 같은 형태로도 가져올 수 있다.

  ### getters
  - computed 와 비슷하게 사용하며, 기존의 state 에 추가적인 작업을 할때 사용한다.
  - 캐싱 효과를 이용할 수 있다.
  - getters 도 computed 에서 가져와서 this.$store.getters.turnMessage 와 같은 형태로 사용할 수 있다.
  - Vue.use(Vuex) 를 한 후에 Vuex.Store 를 하여야 한다.


### 참고사항
- props 말고 Vue 컴포넌트 태그 사이에 속성을 넣어주는 방법도 있다.
- 해당 상위 컴포넌트에 slot 태그를 넣음으로써 해당 태그 사이 속성을 사용할 수 있다.
-
