폴더구조 설계 (cdd, tdd, ddd 등)

 - **슬랙 참고** - 직접 만들어보기 - 본인이 사용하는 구조를 설명할 수 있어야



`npm install vue`

`npm run serve`



**모르는 것들**

```vue
export default {
	data () {
		return {
			msg: '12345'
		}
	}
}
```

axios

```vue
<template>
  <div v-if="types.length">
    <ul>
      <li v-for="item in types" :key="item.name">
        {{ item.name }} {{item.count}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      types: [{ 'name': 'pizza', 'count': 30 }, { 'name': 'korean', 'count': 100 }, { 'name': 'sushi', 'count': 50 }]
    }
  }
}
</script>

```





## 단일 파일 컴포넌트 Single File Components 

Vue.js 컴포넌트를 하나의 파일로 작성하는 기능

.vue 확장자를 갖는 파일에 정의한 컴포넌트

`<template>` `<script>`, `<style>` 세 블록으로 구성

