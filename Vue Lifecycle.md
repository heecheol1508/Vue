## Vue lifecycle

#### Create

`beforeCreate`

`created` 

- `data`, `computed`, `methods`, `watch` 등과 같은 옵션 설정이 완료된 시점
- 아직 DOM에 컴포넌트가 마운트 되지 않았기때문에 `el` 은 사용할 수 없음



#### Mount

컴포넌트가 DOM 에 추가될 때, 실행되는 라이프 사이클 훅으로 서버사이드 렌더링을 지원하지 않는다.

렌더링이 될 때 DOM 을 변경하고 싶다면 이 라이프 사이클 훅을 사용할 수 있음.

하지만 초기에 `data` 가 세팅되어야 한다면 `created` 라이프 사이클 훅을 사용하는 것이 좋습니다. 

`beforeMount` 와 `mounted`가 있음

