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
*/

```