# 3가지 원칙

## 애플리케이션의 모든 state는 하나의 store안에 하나의 객체 트리 구조로 저장됩니다.
---
이를 통해 범용적인 애플리케이션을 만들기 쉽게 할 수 있습니다. 서버로부터 가져온 state는 시리얼라이즈되거나 수화되어 전달되며 클라이언트에서 추가적인 코딩 없이도 사용할 수 있습니다. 또한 하나의 state tree만을 가지고 있기 때문에 디버깅에도 용이할 것 입니다. 빠른 개발 사이클을 위해 개발중인 애플리케이션의 state를 저장해놓을 수도 있습니다. 하나의 state tree만을 가지고 있기 떄문에 이전에는 굉장히 구현하기 어려웠던 기능인 실행취소 / 다시실행을 손쉽게 구현할 수 있습니다.

```javascript
console.log(store.getState());

{
  visibilityFilter: 'SHOW_ALL',
  todos: [{
    text: 'Consider using Redux',
    completed: true,
  }, {
    text: 'Keep all state in a single tree',
    completed: false
  }]
}
```
## Redux에서 state를 변화시키는 유일한 방법은 무슨 일이 벌어지는 지를 묘사하는 action객체를 전달하는 방법뿐입니다.
---
이를 통해서 뷰나 네트워크 콜백에서 결코 state를 직접 바꾸지 못 한다는 것을 보장할 수 있습니다. 모든 state 변화는 중앙에서 관리되며 모든 액션은 엄격한 순서에 의해 하나하나 실행되기 때문에, 신경써서 관리해야할 미묘한 경쟁 상태는 없습니다. action은 그저 평범한 객체입니다. 따라서 기록을 남길 수 있고, 시리얼라이즈 할 수 있으며, 저장할 수 있고, 이후에 테스트나 디버깅을 위해서 재현하는 것도 가능합니다.

```javascript
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
});

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
});
```
# action에 의해 state tree가 어떻게 변화하는 지를 지정하기 위해 프로그래머는 순수 Reducer를 작성해야합니다.
---
Reducer는 그저 이전 state와 action을 받아 다음 state를 반환하는 순수 함수입니다. 이전 state를 변경하는 대신 새로운 state 객체를 생성해서 반환해야한다는 사실을 기억해야 합니다. 처음에는 하나의 리듀서만으로 충분하지만, 애플리케이션이 성장해 나가면 state tree의 특정한 부분들을 조작하는 더 작은 리듀서들로 나누는 것도 가능합니다. 리듀서는 그저 함수이기 때문에 호출되는 순서를 정하거나 추가적인 데이터를 넘길 수도 있습니다. 심지어 페이지네이션과 같이 일반적인 재사용 가능항 Reducer를 작성하는 것도 가능합니다.
```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
  case 'SET_VISIBILITY_FILTER':
    return action.filter;
  default:
    return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return [...state, {
      text: action.text,
      completed: false
    }];
  case 'COMPLETE_TODO':
    return [
      ...state.slice(0, action.index),
      Object.assign({}, state[action.index], {
        completed: true
      }),
      ...state.slice(action.index + 1)
    ];
  default:
    return state;
  }
}

import { combineReducers, createStore } from 'redux';
let reducer = combineReducers({ visibilityFilter, todos });
let store = createStore(reducer);
```
