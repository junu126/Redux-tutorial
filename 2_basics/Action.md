# Action

리듀서가 취급하는 Action을 다른 리듀서에 위임하는 방법과 Redux를 이용해서 프레젠테이션 구성요소에서 컨테이너 구성요소를 생성하는 방법을 보여주는 예제를 보여드리겠습니다.

이 예제는 Redux 공식 홈페이지에서 받아오는 예제 입니다.
```
git clone https://github.com/reactjs/redux.git

cd redux/examples/todos
npm install
npm start
```
위의 명령어를 실행하시면 예제를 불러올 수 있습니다.

---

먼저 액션을 정의해 봅시다.

**액션**은 애플리케이션에서 스토어로 보내는 데이터 묶음입니다. 이들이 스토어의 유일한 정보원이 됩니다. 여러분은 `store.dispatch()`를 통해 이들을 보낼 수 있습니다.

새 할일의 추가를 나타내는 Action의 예시입니다.

```javascript
const ADD_TODO = 'ADD_TODO'
```
```javascript
{
    type: ADD_TODO,
    text : 'Build my first Redux app'
}
```
액션은 평범한 자바스크립트 객체입니다. 액션은 반드시 어떤 형태의 액션이 실행될지 나타내는 `type`속성을 가져야 합니다. type은 일반적으로 `문자열 상수`로 정의됩니다. 여러분의 앱이 충분히 커지면 타입들을 별도의 모듈로 분리할수도 있습니다.

```javascript
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```
> **보일러플레이트에 대한 설명**  
<br>
액션 타입 상수를 반드시 별도의 파일에 정의할 필요는 없습니다. 심지어 정의하지 않아도 됩니다. 작은 프로젝트에서는 액션 타입으로 그냥 문자열을 쓰는게 쉬울겁니다. 하지만 코드 베이스가 커지면 상수를 정의해서 얻을 수 있는 장점이 있습니다.

사용자가 할일을 완료했다고 체크하는 액션 하나를 더 추가합시다. 할일은 배열 안에 저장되기 때문에 우리는 특정한 할일을 `index`를 통해 참조할 수 있습니다. 진짜 앱에서는 새 할일이 만들어질 때 마다 유일한 ID를 부여하는게 더 좋겠죠.

```javascript
{
    type : COMPLETE_TODO,
    inex : 5
}
```
각 액션에는 가능한 적은 데이터를 전달하는 것이 좋습니다. 예를 들어, 할 일 객체 전체를 전달하는 것 보다는 index를 전달하는 것이 낫습니다. 마지막으로, 지금 보이는 할일들을 바꾸는 액션을 추가하겠습니다.

```javascript
{
    type : SET_VISIBILITY_FILTER,
    filter : SHOW_COMPLETED
}
```
## 액션 생산자

**액션생산자는 액션을 만드은 함수입니다.** "액션과" "액션 생산자"는 혼용하기 쉬운 용어이니 적절하게 사용하도록 신경써야 합니다.

전통적인 [Flux](https://taegon.kim/archives/5288) 구현에서 액션 생산자는 보통 불러와졌을때 액션을 보냅니다.

```javascript
function addTodoWithDispatch(text) {
    const action = {
        type : ADD_TODO,
        text
    }
    dispatch(action)
}
```
이와는 대비되게 Redux의 액션 생산자는 단지 액션을 반환합니다.