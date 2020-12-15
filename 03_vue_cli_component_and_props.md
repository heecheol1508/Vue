# vue-cli 3.0



프로젝트 폴더구조

```
node_modules
public
	-index.html // 기본 html
src
	-assets		// 이미지 저장 폴더
	-components	//
	-views		//
	-App.vue	//
	-main.js	// 이 프로젝트에서 어플리케이션을 구동시킴
	-router.js	// 라우터 관리
	-store.js	// 전역 상태 값 관리, vuex
```



node_modules

- vue-cli를 설치하면 같이 다운됨
- 필요할 때 가져다 쓸 수 있는 모듈들이 들어있음
- package.json 내역을 바탕으로 모듈들이 설치됨



vue-cli-plugin

프로젝트를 시작할 때 프로젝트의 개발환경을 구축하기 위해 몇가지 필요한 사항을 선택했음

원래는 추후에 수정해야하는 부분이 생기면 매우 복잡했음

plugin 기능이 생긴 이후에는 쉽게 추가할 수 있게 됨

vue에서 공식적으로 제공하는 플러그인 @vue/cli-plugin

또는 vue-cli-plugin 검색해서 하면 됨. ex) vuetify



plugin의 역할

프로젝트를 개발하면서 개발환경을 수정해야 하는 경우가 있음.

그럴 때 프로젝트에 추가된 파일들을 수정하고 설정들을 직접 바꿔주기가 힘든데,

plugin이 그걸 대신 해줌.

다시 말해서, plugin으로 개발이 진행되는 중에도 간단하게 개발환경을 셋팅할 수 있음

vuetify 등



https://youtu.be/C-AAkSRsMTk



**App.vue**

```vue
<template>	// template 속성 안에는 html 코드가 들어감
	<div>
        ...
    </div>
</template>

<script>	// script 코드    
    export default {	// export default는 ES6에서 모듈을 추출하는 방식 중에 하나
        name: 'App',
        data () {	// 컴포넌트를 만들어 사용할 때는 data를 함수 형태로 선언해줘야 함
            return {
                title: "안녕하세요",
                count: 1
            }
        },
        methods: {
            // 여기에 함수 선언해서 사용할 수 있음
        },
        computed: {
            // 등등 속성 설정 가능
        }
    }
</script>
```



**main.js**

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

new Vue({	// vue를 새로 선언, 선언된 vue를 통해서 전체 어플리케이션이 돌아감
    router,
    store,
    render: h => h(App)
}).$mount('#app')
```



**export, import 비교**

규격...이라고 생각하기? 규격들을 맞춰서 vue 파일이 구성되어야만 나중에 배포될 때의 최종결과물인 js파일을 완성할 수 있음



```vue
# App.vue
<template>
	<div>
        <h1>{{ title }}</h1>
        <p>{{ count }}</p>
        <button @click="count++">추가</button>
    </div>
	
</template>
<script>
    export default {
        data () {
            return {
                title: "안녕하세요",
                count: 1
            }
        }
    }
</script>
```

이렇게 App.vue 파일안에 모든 기능을 다 쓰는 것은 

cdn으로 vue를 import 시켜서 한 파일 내에서 모든 기능을 작성하는 것과 크게 다르지 않음

그렇다면 cli로 만든 이 개발환경에서는 각각의 vue파일들을 어떻게 관리하고 어떻게 사용하면 좋을까

-> 컴포넌트로 등록하고 사용하기





#### 컴포넌트를 등록하고 사용하는 두가지 방법



1.첫번째 방법 - 하나의 파일을 하나의 파일에서 불러오기

```vue
# Home.vue
<template>
	<div>
        <h1>{{ homeTitle }}</h1>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                homeTitle: "홈홈홈"
            }
        }
    }
</script>
```

```vue
# App.vue
<template>
	<div>
        <h1>{{ title }}</h1>
        <p>{{ count }}</p>
        <button @click="count++">추가</button>
        <HomeComponent></HomeComponent>
    </div>
</template>
<script>
    // 외부의 vue 파일을 가져오기 위한 약속, 규격
	import HomeComponent from './Home.vue'	// .vue 생략가능
    
    export default {
        // 홈 컴포넌트를 탬플릿 안에서 사용하기 위해서는
        // export default 내에서 '이건 컴포넌트야'라고 선언해줘야함
        components: {
            HomeComponent
        },        
        data () {
            return {
                title: "안녕하세요",
                count: 1
            }
        }
    }
</script>
```



2.두번째 방법 - 전체에서 하나의 파일 불러오기 (전역에서 그 컴포넌트를 사용할 수 있도록 선언)

ex) footer, navbar 등 이 방법으로 사용 가능

```vue
# Status.vue
<template>
	<div>
        <h1>{{ status }}</h1>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                status: "good"
            }
        }
    }
</script>
```

```javascript
// main.js
import StatusComponent from './Status.vue'

Vue.component('AppStatus', StatusComponent)	// Vue.component('컴포넌트명', 옵션)

```

main.js 파일은 vue 어플리케이션을 관장하는 역할을 하기 때문에

main.js에 선언해준 Appstatus 라는 컴포넌트는 어떤 파일에서나 다 사용가능





#### 컴포넌트들 간에 데이터를 주고받는 방법



### Props

부모 컴포넌트에서 자식 컴포넌트로 데이터를 넘겨주는 방법(property)



`v-bind`를 통해서 전달

```
부모(User), 자식(UserDetail)일때 

User.vue 안에서
<UserDetail :nameOfChild="name"></UserDetail> 이렇게 넘겨줄 수 있음

// nameOfChild 자식에서 선언되는 변수명
// name는 어떤값을 넘겨줄건지를 넣음. 숫자, 객체 등 여러 형태로 넣을 수 있음

UserDetail.vue에서는
props: ['nameOfChild'], 이렇게 리스트 형식으로 받아서 써줘야 함, 리스트 안에 string

```



데이터가 복잡해지고 여러 개의 props를 쓰다보면 데이터의 형태 등이 헷갈릴 수 있음

데이터 타입을 기록하기 위해서는 props를 받을 때 array가 아니라 object로 받아오면 됨

```
props: {
	nameOfChild: String
}
```

속성도 추가할 수 있음

```
props: {
	nameOfChild: {
		type: String,
		required: true
	}
}
```

```
props: {
	nameOfChild: {
		type: String,
		default: 'HEE'
	}
}
```





Props를 만약에 내가 설명한다면???







