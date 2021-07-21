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

- NOTION URL : https://polyester-trip-9ae.notion.site/Do-it-Vue-js-64576ef5b1674c3683df2e7ef1efd3f8

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

# Vue.js 심화
# 목차
- slot을 이용한 상품 컴포넌트 활용
- Mixin을 이용한 살품 필터 구현
- Provide-Injection
- toast
- axios
- vuex
- Jest
- etc

## 프로젝트 구조
- node_modules : 패키지.js 에 의해서 설치되는 라이브러리 폴더
- package.js 에 scripts 를 통해 run 커맨드 관련 cli 등을 지정할 수 있음
- 참고 ) vue-cli-service server

## slot
- 외부로부터 tamplate 을 전달받아 컴포넌트의 view 를 유연하게 구성할 수 있게 하는 기능
- 부모 컴포넌트를 레이아웃 용도로만 사용하고 기능을 캡슐화한 컴포넌트 에 유용하다
- 자식 컴포넌트에 slot 태그를 선언하면 부모 컴포넌트의 Dom 요소가 주입된다
- props 의 경우, 부모에서 자식으로 data 속성을 전달하는 반면에, slot 은 부모 컴포넌트에 의해 렌더링 된 Dom 이 자식으로 주입된다.
- 특정 자식 컴포넌트에 특정 다른 태그를 도입하고자 할때, 자식 컴포넌트에 slot 내 기본 태그를, 부모 컴포넌트에 특정 태그에 특정 Dom 을 넣어서 활용할 수 있다

## Named Slot
- 하나의 컴포넌트 내에 여러 개의 slot 이 필요한 경우 사용
- 부모 컴포넌트에 template v-slot:슬롯명 으로 구현
- 자식 컴포넌트 slot 태그에 name='슬롯명' 으로 구현
- 단축표기 : #슬롯명

## Slot Props
- 범위가 있는 슬롯
- v-bind 를 이용하여 자식 컴포넌트에서 부모 컴포넌트로 객체를 전달
- :props이름="" 으로 전달, v-slot:슬롯이름="slotProps"

## Mixin
- Vue 컴포넌트에 재사용 가능한 기능을 배포하는 방법
- 여러 개의 모듈을 mixin 가능
- 컴포넌트 자체에 중첩된 옵션이 있으면 컴포넌트에 우선순위가 있음
- 라이프 사이클 훅은 Mixin 이 먼저 실행됨
- 같은 data 이름이 있으면 라이프사이클 훅에 따라 mixin1 이 먼저 생성되고 이후 2가 데이터를 덮어쓰고 인스턴스 생성 시 같은 데이터명이 있으면 덮어쓴다고 보면 된다.

## Provide/Inject
- 구조 관계가 매우 깊은 컴포넌트 간에 데이터를 전달할 수 있는 방법
- Provide (부모) - Inject (자식) 이 쌍으로 선언된 경우, 데이터 전송 가능
- 장거리 props
- Props 와 달리 반응형이 아니며 일반적인 Component 에서는 사용을 권고하지 않음
- 보내줄 컴포넌트에서는 provide() 함수에 데이터를 보내주고, 받는 컴포넌트에서는 inject: ['데이터명'] 과 같은 방식으로 사용

## Vue examples
- Vue.js 를 사용하여 만든 컴포넌트들을 소개
- 사용자에게 toast 알림기능을 적용

## Axios
- 설정옵션에서 timeout 시간과 cross-site Access-Control 옵션을 줄 수 있다.
- 기존 자바스크립트에서 비동기 처리를 위해 주로 콜백을 사용했는데 콜백 hell 이 생성되어서 Promise 객체가 만들어졌다
- url 뒤에 객체로 싸서 파라미터를 전달가능하다
- Json-server : npm 설치 만으로 rest api server 를 간단하게 구성해주는 라이브러리이다
- GET 메소드 사용 시, ? 대신 / 을 사용하는 다이나믹 방식도 가능하고 특정 속성에 대한 추출도 가능하다 ( & 사용하여 복합 필터링도 가능 )
- Interceptor : HTTP 통신의 request/response 를 가로채어 특정한 처리를 할 수 있게 해주는 함수, 권한 체크 등에 쓰임
- request/response 를 가로채어 로딩바를 나타내는 등의 작업을 한다.

## Vuex
- Vuex 는 전역변수를 잘못 관리할 경우, 생기는 문제점을 해결하고자 단방향 데이터 흐름으로 만든 것이라 할 수 있다.
- Vue Components 가 Action 으로 Dispatch 하고 Actions 는 Mutations 로 Commit 을 하고 State 로 Mutate 되어 Vue Componenst 에서 다시 렌더링 되는 것으로 보면 된다.
- 컴포넌트가 액션을 일으키면 액션에서는 외부 API 를 호출한 뒤 그 결과를 이용해 변이를 일으키고 변이에서는 액션의 결과를 받아 상태를 변경한다.
- 따라서 Mutations 만 watch 하면 State 의 변화를 알 수 있다.
- 상태관리 패턴을 지원하는 라이브러리로 중앙 집중화된 상태 정보 저장소 역할을 수행한다.
- 속성이나 이벤트의 발신, 전달이 불필요해진다.

### 저장소(Store)
- 애플리케이션 상태를 중앙 집중화하여 관리하는 컨테이너로 상태를 직접 변경하지 않으며, 변이를 통해서만 변경 가능하다.

### 상태 (State)
- 저장소에 의해 관리되는 애플리케이션의 데이터

### 변이 (Mutations)
- 기존 상태를 유지하고 새로운 상태 값을 생성한다.
- 상태의 변화를 추적할 수 있으며, 상태의 변경과 관련 없는 작업은 변이로 처리하지 않는다.
- 동기적인 작업에 쓰이며 비동기 처리는 Action 에서 처리한다.

### 액션 (Action)
- 상태 변화와 관련이 없는 작업, 비동기 처리를 한다.

### helper 함수
- mapState, mapMutations, mapActions
- 객체를 리턴하므로 로컬 영역에 다른 요소와 함께 사용할 때는 전개 연산자(...Object) 사용
- 뷰 컴포넌트에서는 $store.dispatch 를 통해 Context 와 item 을 보내주고 해당 actions 에서는 context.commit 을 통해 뮤테이션을 호출하여 state 를 변경하는 방식이다.
- Vuex 의 state 에 직접 접근하여 push 할수는 있으나, console error 가 나기 때문에 mutations 를 통해 접근하자.
- 새로고침 시 삭제되는 것을 방지하기 위해 localStorage 를 사용할 수 있다.
- Action dispatch 를 하거나 Mutation commit 으로 데이터를 변경할 수 있는데, Action 단에서 백엔드 API 를 주로 호출한다고 보면 된다.

## Test
- 어플리케이션 검증을 위해서 테스트를 하는데, 작은 모듈 단위로 떼어내 분리된 환경에서 테스트하는 것을 단위 테스트라 한다.
- 대표도구 : Jest, Mocha, Testing Library

### Jest
- Decribe 란 테스트 대상에 대한 범위를 지정하고 설명하는 것이며 중첩하여 선언 가능하다.
- test 에는 테스트 명과 테스트할 함수를 넣어서 수행한다.
- Expect - Matcher 는 기대 값과 실제 수행 결과를 비교하여 테스트의 성공여부를 판단하는 도구이다.
- 하나 matcher 가 실패하는 경우 해당 테스트는 fail 이다.
- vue-cli-service test:unit 과 같은 형태로 해당 테스트코드 파일을 실행해볼 수 있다.
- Mock : 외부와의 연결을 끊기 위해 주입하는 모의 객체
- Mock 호출 여부는 toHaveBeenCalled 로 확인
- jest.fn() 으로 Mock Function 생성
- spyOn : 함수호출로 인해 변경되는 값을 확인할 때 사용
- Vue 테스트를 위해서 Vue Test Util 에서는 필요한 함수를 제공한다.
- mount 는 single file component를 테스트를 위해 mount 시킨다.
- shallowMount : child component 를 제외하고 mount 시킨다.
- createLocalVue : plugin, mixin 등을 별도로 구성할 수 있는 vue class 를 제공한다.
- Javascript 의 비동기 처리 방식 때문에, async/await/$nextTick() 과 같은 형태로 작성한다.

## etc
### keep-alive : 성능상의 이유 등으로 컴포넌트 상태를 유지하거나 다시 렌더링하지 않을 경우 사용한다
- keep-alive 태그 사이에 컴포넌트를 넣고 해당 컴포넌트 의 :is 에 렌딩하지않고 캐싱해서 사용하고 싶은 컴포넌트 이름을 넣는다.
- 컴포넌트를 v-for 로 사용하고 있는 경우는 동작하지 않음
- 라이프 사이클 훅에 activated, deactivated 가 추가됨
- 특정 컴포넌트를 캐싱해서 보여줄 때 activated 되고, 그 외에는 deactivated 되며 재생성하지 않는다.

### Lazy Loading : SPA 의 경우 렌더링이 빠르지만, 규모가 커짐에 따라 최초 로딩 시간이 길어지는 단점이 있다.
- 해당 문제를 해결하기 위해 하나가 아닌 여러개로 화면을 번들링되어 지연 다운로드하는 기능을 Lazy Loading 이라고 한다.
- router.js 에 import 문 대신에 const 로 선언하여 webpackChunkName 주석으로 import 하면 해당 그룹 별로 번들링 된다.

### Webpack Bundle Analyzer
- Webpack bundling 결과를 알려주어 해당 프로젝트에서 어떤 번들링이 차지하는 크기가 큰지 파악할 수 있다.
- config.js 파일같은 부분 configureWebpack - plugins 에 new 하여 넣어주게 되면 시각화하여 보여준다.
- 해당 번들링 파일 사이즈를 확인하여 특정 라이브러리가 무겁다면 사용하는 모듈만 import 하는 방법 등이 있다.
