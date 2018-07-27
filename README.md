[![Redux](/image/ReduxIcon.PNG)](https://github.com/reduxjs/redux)

---

Redux라이브러리를 정의해보면 '자바스크립트 앱을 위한 예측 가능한 `상태컨테이너`'라고 할 수 있습니다.  
(혹시 [WordPress](https://reduxframework.com/) 프레임워크를 찾아오셨으면 이 곳이 아니니 태그된 링크로 이동해주세요.)  

정의에서 말했던 상태컨테이너는 트리구조에서 자식들이 부모에게 값을 전달할 때 하나하나 props들을 거치면서 이동하는데 이러한 과정을 Skip할 수 있도록 도와주는 착한 아이입니다.  
방금 전 과정을 상태컨테이너를 이용하면, props들을 거치지 않고, 자식에서 컨테이너로, 컨테이너에서 부모로 바로 이동하는 과정을 거치게 됩니다.  

Redux는 우리가 일관적으로 동작하고, 서로 다른 환경에서 작동하고, 테스트하고 쉬운 앱을 만들 수 있도록 도와줍니다. 여기에 더해 [시간여행형 디버거와 결합된 실시간 코드 수정과 같은 개발 경험](https://github.com/reduxjs/redux-devtools)을 얻을 수 있습니다.

우리는 Redux를 여러 라이브러리와 함께 사용할 수 있습니다. 예를 들면 [React](https://reactjs.org/)라이브러리가 있죠, 또한 Redux는 매우 가벼운 라이브러리 입니다(2KB, including dependencies).

# Redux Curriculum
## 초보
리덕스를 처음 배울때에는 Redux홈페이지에서 제공하는 자료들을 공부하거나, 기본개념을 공부하는 것이 좋습니다.
- HTML, CSS, JS 등 [기초 자료](https://www.w3schools.com/)
- [React](https://reactjs.org/tutorial/tutorial.html)에 대한 기초적인이해
- Redux문서의 [자료](https://redux.js.org/basics)
- Redux문서의 [예제](https://redux.js.org/introduction/examples)
- Redux에 대한 [개발자 경험](https://medium.com/@Dev_Bono/%EB%8B%B9%EC%8B%A0%EC%97%90%EA%B2%8C-redux%EB%8A%94-%ED%95%84%EC%9A%94-%EC%97%86%EC%9D%84%EC%A7%80%EB%8F%84-%EB%AA%A8%EB%A6%85%EB%8B%88%EB%8B%A4-b88dcd175754)

## 중급
리덕스를 공부해서 action, reducers, store와 같은 개념들을 이해하였다면, AJAX와의 통신, React와의 연동을 통한 앱등을 만들어 보고, 공부하는 것이 좋습니다.
- [advanced](https://redux.js.org/advanced)에서는 비동기 로직이나, 라우팅 작업등을 다룹니다.
- 리덕스의 관리자 중 하나인 Mark Erikson의 [Redux 튜토리얼 시리즈](http://blog.isquaredsoftware.com/series/practical-redux/)

마지막으로 Mark Erikson은 주기적으로 [Redux워크샵](https://github.com/reduxjs/redux#redux-workshops)을 주최합니다. 기회가 되면 워크샵에 참여 할 수 있는 기회를 잡아보세요.



# 영향을 받은 것들
Redux는 [Flux](http://facebook.github.io/flux/)의 아이디어를 발전시키면서 [Elm](https://github.com/evancz/elm-architecture-tutorial/)의 큐들을 가져옴으로써 복잡성을 줄였습니다. Redux를 시작 하기 까지의 시간이 단지 몇분 안걸립니다.

# 설치
여러분들이 NPM을 패키지매니저로 사용하고 있을때 가장 안정적인 버전을 설치하는 방법입니다.  
(개인적이지만 NPM 패키지 매니저를 사용하기를 추천합니다.)
>`npm install --save redux`

## 보조패키지
React와 Redux를 연동시켜주는 React바인딩 기술을 설치합니다.
> `npm install --save react-redux`  

그리고 간편하게 사용하기 위한 개발자도구를 설치합니다.
> `npm install --save-dev redux-devtools`  

Redux와 달리 보조패키지들은 [UMD빌드](https://github.com/umdjs/umd)를 제공하지 않으므로, 편안한 개발환경을 위해 [Webpack](https://webpack.js.org/)이나 [Browserify](browserify.org/) 같은 CommonJS 모듈 번들러를 사용하시기 바랍니다.

# 팩트
앞으로 Redux를 공부하면서 가장 먼저 만나는 것이 `store`와 `action` 그리고 `reducers`.  
이 세 가지를 중심으로 사용하고, 만지고 놀겁니다!  

Redux의 간단한 구조를 설명드리면 여러분이 만드실 앱의 state는 전부 *store*안에 있는 객체트리에 저장되게 됩니다. state트리를 변경하는 유일한 방법은 객체트리로 *action*을 보내는 것 입니다. 그리고 어떻게 변경할지 명시하기 위하여 여러분은 *reducers*를 사용하게 될 것 입니다!

이 과정을 이해하고 알 수 있다면 Redux는 끝입니다! 
~~**팩트에요!**~~

# 예제
다음은 Redux를 이용한 간단한 예제 파일입니다.

```js
import { createStore } from 'redux'
/* createStore에 괄호를 씌운 이유는 
redux모듈내에 있는 createStore를 가져오기 위함 입니다. */

/**
    * 이것이 (state, action) => state 형태의 순수 함수인 reducer 입니다.
    * 리듀서는 action이 어떻게 state를 다음 state로 변경하는지 서술합니다.
    * 
    * state의 모양은 저희가 정합니다. 배열 일수도 있고, 객체일 수도 있고, 기본형일 수도 있습니다.
    * 심지어 Immutable.js 자료구조 일 수도 있습니다. 중요한 점은 state를 직접 건들이면 안되며 
    * state가 변경되면 객체를 새로 반환해야 합니다.
    * 이 예시에서는 switch문을 사용했지만
    * 기호에 따라서 map()함수와 같은 다른 컨벤션을 사용하셔도 좋습니다.
*/
function counter(state = 0, action) {
    switch(action.type) {
        case 'INCREMENT' :
            return state + 1
        case 'DECREMENT' :
            return state - 1
        default : 
            return state
    }
}

// 앱의 상태를 보관하는 Redux 스토어를 생성합니다.
//  API의 종류로는 { getstate, dispatch, subscribe, replaceReducer }가 있습니다.

let store = createStore(counter)

// 여러분은 subscribe()를 통해서 state를 변경해서 UI를 업데이트 해 달라고 요청을 보낼 수 있습니다.
// 보통은 subscribe()를 직접 사용하기 보단 뷰 바인딩 라이브러리(React, Redux)를 사용합니다.
// 하지만 현재 상태를 localStorage에 지속적으로 편리하게 넣기 위해서는 subscribe을 사용하는 것이 좋습니다.

store.subscribe(() => 
    console.log(store.getstate())
)

// 내부의 state 값을 변경하는 방법은 action뿐 입니다.
// action은 연속해서 나열 할 수도 있고, 기록할 수도잇고 또는 저장하거나 재사용 할 수도 있습니다.

store.dispatch({ type : 'INCREMENT' })
// 1
store.dispatch({ type : 'INCREMENT' })
// 2
store.dispatch({ type : 'DRCREMENT' })
// 1
```

State를 직접적으로 바로 변경하는 대신, Action이라고 불리는 평범한 객체를 통해 일어날 일을 명시 합니다. 그리고 각각의 action이 전체 APP의 State를 어떻게 변경할지 결정하는 특별한 함수인 Reducer를 작성합니다.

그리고 Redux를 사용하기 전에 Flux를 사용하던 분들은 알아둬야 할 것이 있습니다.
Redux에선 Dispatcher가 없습니다. 또한 store를 여러게 지원하지 않고 단 하나만 지원합니다. 그래서 처음 공부할 때 조오금 했갈리는 부분이 있을 수 있습니다. 나중에 여러분의 APP이 커졌을 때, store를 추가하는 대신 root reducer를 쪼개서 state tree에 각기 다른 부분을 독립적으로 다루는 reducer를 만들면 됩니다. 마치 React에서 하나의 root component가 있고, 이것을 여러개의 작은 component로 나누는 것 처럼 말이죠.

이러한 기능이 숫자 세는 앱과 같은 작은 앱에는 과도해 보일 수 있지만, 이 패턴의 어썸한면이 크고 복잡한 앱으로 확장하기 좋다는 점이 있습니다. 또한 이것은 action이 일으키는 모든 변경을 추적함으로 써, 강력한 개발자 도구를 가능하게 합니다. 마지막으로 여러분은 action을 재생하는 것 만으로 사용자의 세션을 기록하고, 재생할 수 있습니다.

# Documentation

* [소개](내 깃헙 업로드 할 주소.))
* [기초](내 깃헙 업로드 할 주소.)
* [심화](내 깃헙 업로드 할 주소.)
* [레시피](내 깃헙 업로드 할 주소.)
* [문제해결](내 깃헙 업로드 할 주소.)
* [용어사전](내 깃헙 업로드 할 주소.)
* [API 레퍼런스](내 깃헙 업로드 할 주소.)

# 번역
영어로 작성된 레포를 직접 번역헀습니다.