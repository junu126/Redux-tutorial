# Immutable.js

React에서 State를 직접적으로 변경 불가능 하다는 사실은 여러분 모두 아실겁니다. `Setstate()`를 통해서 우리는 State를 변경 할 수 있죠. 저희가 직접 state를 변경 할 수 없다는 점은 정말 불편합니다. 다음 예제를 봐주세요.

```js
state = {
  where: {
    are: {
      you: {
        now: 'faded',
        away: true // 요놈을 바꾸고 싶다!
      },
      so: 'lost'
    },
    under: {
      the: true,
      sea: false
    }
  }
}
```
만약 위의 예제와 같은 state가 있다고 할 때 저라면 가장 먼저 생각나는 것이 구조가 너무 복잡하다.. 일 것 같습니다.  
위 state의 불변함을 유지하면서 업데이트를 하려면 정말 귀찮을 것 입니다..   

그래서 사용하는 것이 바로 `Immutable.js`라이브러리 입니다!!

# 설치
프로젝트에서 Immutable.js를 사용하기 위해선 먼저 라이브러리를 불러와야 합니다.
> `yarn add immutable`  

다음과 같은 코드를 작성하셔서 페키지를 불러와 주세요.  

# 사용법
Immutable.js를 사용하는데에는 정말 어려움이 없습니다. 단지 몇개의 규칙들만 기억하시면 됩니다.

> 1. 객체는 Map
> 2. 배열은 List
> 3. 설정할 땐 set
> 4. 읽을 땐 get
> 5. 읽은 다음 설정할 땐 update
> 6. 내부에 있는걸 ~ 할 땐 뒤에 in을 붙인다. EX) setin, getin, updatein
> 7. 일반 자바스크립트 객체로 변환 할 때에는 toJS
> 8. List 엔 배열 내장함수와 비슷한 함수들이 있다 – push, slice, filter, sort, concat… 전부 불변함을 유지함
> 9. 특정 key 를 지울때 (혹은 List 에서 원소를 지울 때) delete 사용

간단한 사용 예시를 들어보겠습니다.  
```js
import React from 'react';
import { render } from 'react-dom';
import App from './App';
import { Map, List } from 'immutable';

// 1. 객체는 Map
const obj = Map({
  foo: 1,
  inner: Map({
    bar: 10
  })
});

console.log(obj.toJS());
// Object {foo: 1, inner: Object}
//  foo: 1
//   inner: Object
//    bar: 10

// 2. 배열은 List
const arr = List([
  Map({ foo: 1 }),
  Map({ bar: 2 }),
]);

console.log(arr.toJS());
// [Object, Object]
//   0: Object
//    foo: 1
//   1: Object
//    bar: 2

// 3. 설정할땐 set
let nextObj = obj.set('foo', 5);
console.log(nextObj.toJS());
// Object {foo: 5, inner: Object}
//  foo: 5
//   inner: Object
//    bar: 10
console.log(nextObj !== obj); // true

// 4. 값을 읽을땐 get
console.log(obj.get('foo')); // 1
console.log(arr.get(0)); // List 에는 index 를 설정하여 읽음

// 5. 읽은다음에 설정 할 때는 update
// 두번째 파라미터로는 updater 함수가 들어감 
nextObj = nextObj.update('foo', value => value + 1);
console.log(nextObj.toJS());
// Object {foo: 6, inner: Object}
//  foo: 6
//   inner: Object
//    bar: 10

// 6. 내부에 있는걸 ~ 할땐 In 을 붙인다
nextObj = obj.setIn(['inner', 'bar'], 20);
console.log(nextObj.getIn(['inner', 'bar'])); // 20

let nextArr = arr.setIn([0, 'foo'], 10);
console.log(nextArr.getIn([0, 'foo'])); // 10

// 8. List 내장함수는 배열이랑 비슷하다
nextArr = arr.push(Map({ qaz: 3 }));
console.log(nextArr.toJS());
// [Object, Object, Object]
//   0: Object
//    foo: 1
//   1: Object
//    bar: 2
//   2: Object
//    qaz: 3

nextArr = arr.filter(item => item.get('foo') === 1);
console.log(nextArr.toJS());
// [Object]
//  0: Object
//   foo: 1

// 9. delete 로 key 를 지울 수 있음
nextObj = nextObj.delete('foo');
console.log(nextObj.toJS());
// Object {inner: Object}
//  inner: Object
//   bar: 20

nextArr = nextArr.delete(0);
console.log(nextArr.toJS()); // []


render(<App />, document.getElementById('root'));
```
주석을 읽어보시면서 천천히 맞쳐보시기 바랍니다.

## Recode메소드
Recode라고 하는 Immutable.js의 **엄청난** 메소드가 있습니다. Recode를 사용하면 update, set, delete 등을 사용할 수 있으면서도, get, getin을 사용할 필요 없이 `data.input`과 같은 형식으로 조회를 할 수 있습니다.  

Record 는, Typescript 혹은 Flow 같은 타입시스템을 도입 할 때 굉장히 유용합니다.  

다음은 Recode의 예제파일 입니다.
```js
import React from 'react';
import { render } from 'react-dom';
import App from './App';
import { Record } from 'immutable';

const Person = Record({
  name: '홍길동',
  age: 1
});


let person = Person();

console.log(person); 
// ▶Object {name: "홍길동", age: 1 }

console.log(person.name, person.age);
// "홍길동" 1

person = person.set('name', '김민준');
console.log(person.name); // 김민준


// 이건 오류 납니다: person.name = '철수';

// Record 에서 사전 준비해주지 않은 값을 넣어도 오류납니다.
// person = person.set('job', 5);


// 값을 따로 지정해줄수도 있습니다.
person = Person({
  name: '영희',
  age: 10
});

const { name, age } = person; // 비구조화 할당도 문제없죠.
console.log(name, age); // "영희" 10

// 재생성 할 일이 없다면 이렇게 해도 됩니다.
const dog = Record({
  name: '멍멍이',
  age: 1
})()

console.log(dog.name); // 멍멍이

// 이런것도 가능하죠.
const nested = Record({
  foo: Record({
    bar: true
  })()
})();

console.log(nested.foo.bar); // true

// Map 다루듯이 똑같이 쓰면 됩니다.
const nextNested = nested.setIn(['foo', 'bar'], false);
console.log(nextNested);

render(<App />, document.getElementById('root'));
```

저는 Recode가 정말 훌륭한 메소드라고 생각합니다.