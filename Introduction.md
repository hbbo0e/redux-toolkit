
# Get Started
## 내용

- `confidugreStore()` : createStore 의 단순화된 구성 옵션과 적절한 기본값을 제공하기 위해 래핑된다. **자동으로** Slice Reducer 를 결합하고, 제공하는 Redux 미들웨어를 추가하고, redux devtools extension 을 사용할 수 있다.
- `createReducer()` : switch 문을 작성하는 대신 action type 에 대한 case Reducer 함수에 작업 유형의 조회 테이블을 제공할 수 있다. 자동으로 라이브러리 `immer` 를 사용하여 일반적인 변이 코드로 간단한 불변 업데이트를 작성할 수 있도록 한다. 예를 들어, `state.todos[3].completed=true` 와 같은 코드를 사용한다.
	[[immer]]
- `createAction()` : 주어진 액션 유형 문자열에 대한 액션 생성기 함수를 생성한다.
- `createSlices()` : 리듀서 함수의 객체, 슬라이스 이름, 초기 상태 값을 받아들이고 해당 액션 생성자와 액션 유형을 사용하여 슬라이스 리듀서를 자동으로 생성한다.
	- 리덕스에서 슬라이스를 관리하는데 필요한 여러 가지 요소들을 자동으로 생성할 수 있다. 
	- 리덕스에서 **슬라이스별로 필요한 리듀서 함수와 관련된 액션 생성자 및 액션 타입을 생성하기 위해 사용된다**.
		- 슬라이스 ?
			: 상태 트리의 일부를 나타내는 작은 조각이다. 리덕스는 애플리케이션 전체 상태를 하나의 큰 객체로 관리하는 데 이를 여러 개의 슬라이스로 나눌 수 있다. 
			=> 애플리케이션의 특정 부분에 대응하며, 해당 부분을 담당하는 리듀서 함수와 액션 생성자를 포함한다.
- `combineSlices()`  : 여러 슬라이스를 하나의 리듀서로 결합하고 초기화 후 슬라이스의 지연 로딩을 허용한다.
	- 하지만 이 함수는 모든 리듀서를 한 번에 로드하므로, 큰 규모의 애플리케이션에서는 초기 로딩 시간이 길어질 수 있다.
	- 초기화 할 때는 필요한 슬라이스만 로드되고 필요에 따라 추가적인 슬라이스가 나중에 로드될 수 있다는 것이다. -> 초기 로딩 시간을 줄이고, 애플리케이션의 성능을 향상시킬 수 있도록 한다.
- `createAsyncThunk` : 액션 유형 문자열과 프로미스를 반환하는 함수를 받아들여서 그 프로미스를 기반으로 보류 중 (pending), 완료됨(fulfilled), 실패함(rejected) 액션 유형을 dispatch 하는 thunk 를 생성한다.
	- 일반적으로 비동기 작업을 수행할 때 프로미스를 사용하는데 이를 사용하면 프로미스를 처리하고 액션을 디스패치하는 로직을 간단하게 작성할 수 있다.
- `createEntityAdapter` : 저장소에서 정규화된 데이터를 관리하기 위해 재사용 가능한 리듀서 및 셀렉터 집합을 생성한다.
	- 정규화된 데이터는 중복을 최소화하고 데이터를 구조화하는 기술로, 관계형 데이터베이스에서의 정규화와 유사한 개념이다. 중복된 데이터를 하나의 엔티티로 표현하고 해당 엔티티에 대한 참조를 사용하여 데이터를 연결하는 방식이다. -> 상태 일관성 유지, 메모리 사용 최적화

👇🏻 제대로 안 함 RTK
## RTK
: @reduxjs/toolkit 패키지 내에서 선택적으로 제공되는 추가 기능이다. 데이터 가져오기 및 캐싱 사용 사례를 해결하기 위해 특별히 만들어진 구조로, 앱의 API 인터페이스 계층을 정의하는데 사용되는 도구 세트를 제공한다.
이것은 웹 애플리케이션에서 데이터를 로딩하는 일반적인 경우를 단순화하고, 데이터 가져오기 및 캐싱 로직을 직업 작성할 필요가 없도록 하기 위한 것이다.

내부적으로 redux 를 사용하여 아키텍처를 구성한다. 전역 상태 관리 능력을 제공하는 추가 기능을 탐색하고 Redux DevTools 브라우저 확장 프로그램을 설치하는 것이 좋다.

RTK 쿼리는 핵심 Redux Toolkit 패키지 설치에 포함되어 있습니다. 아래 두 진입점 중 하나를 통해 사용할 수 있습니다.

```
import { createApi } from '@reduxjs/toolkit/query'/* React-specific entry point that automatically generates   hooks corresponding to the defined endpoints */import { createApi } from '@reduxjs/toolkit/query/react'
```

## 내용

RTK 쿼리에는 다음 API가 포함되어 있습니다.

- `createApi` : RTK 쿼리 기능의 핵심입니다. 이를 통해 엔드포인트 집합을 정의하고 해당 데이터를 가져오고 변환하는 방법에 대한 구성을 포함하여 일련의 엔드포인트에서 데이터를 검색하는 방법을 설명할 수 있습니다. 대부분의 경우 경험상 "기본 URL당 하나의 API 슬라이스"를 사용하여 앱당 한 번만 사용해야 합니다.
- `fetchBaseQuery()``fetch`: 요청을 단순화하는 것을 목표로 하는 작은 래퍼입니다 . 대부분의 사용자에게 `baseQuery`사용하도록 권장됩니다 .`createApi`
- `<ApuProvider />` : 아직 Redux 스토어가 없는 `Provider` 경우 로 사용할 수 있습니다 .
- `setupListeners()``refetchOnMount`: 및 동작을 활성화하는 데 사용되는 유틸리티입니다 `refetchOnReconnect`.

RTK 쿼리가 무엇인지, 어떤 문제를 해결하는지, 사용 방법에 대한 자세한 내용은 [**RTK 쿼리 개요**](https://redux-toolkit.js.org/rtk-query/overview) 페이지를 참조하세요 .



# Why Redux Toolkit is How To Use Redux Today
= 리덕스를 사용하는 게 Redux-Toolkit 이 된 이유

- Redux-Toolkit (RTK)
: @reduxjs/toolkit 패키지는 핵심 redux 패키지를 둘러싸며 redux 앱을 구축하는데 필수적이라고 생각되는 API 메서드와 공통 종속성을 포함한다. 
: store 설정, reducer 설정, 불변 업데이트 로직 (immer) 작성, slice 생성 등 다양한 일반적인 사용 사례를 단순화하는데 도움이 되는 유틸리티가 포함되어 있다.

## 기능/todos/todosSlice.js
```js
import { createSlice } from '@reduxjs/toolkit'

const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      state.push({
        id: action.payload.id,
        text: action.payload.text,
        completed: false,
      })
    },
    todoToggled(state, action) {
      const todo = state.find((todo) => todo.id === action.payload)
      todo.completed = !todo.completed
    },
  },
})

export const { todoAdded, todoToggled } = todosSlice.actions
export default todosSlice.reducer
```

- `todoToggled` 
	: todo.id 랑 action.payload 가 동일하면 할 일에 체크 (할일 완료) 이므로 완료 상태를 바꿔준다.

