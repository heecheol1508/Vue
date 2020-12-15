

props에 대해 다시 적어보면

자식 컴포넌트에서 부모 컴포넌트의 값을 전달받는 것

그런데 자식 컴포넌트에서 props 값을 직접 변경하게되면 자식 컴포넌트 내에서만 바뀌게 된다

그리고 부모 컴포넌트에서 값을 변경하면 자식 컴포넌트에서도 오버라이딩이 될 수 있다

그래서 자식 컴포넌트에서 props의 값을 직접 변경하지 않는게 좋다



emit

자식 컴포넌트에서 부모 컴포넌트로 값을 바꿔달라고 보내는 신호





# eventBus

컴포넌트 간 데이터 공유

형제 컴포넌트 간 데이터 공유 가능



받는 부분에서 eventBus를 듣기 위한 별도의 장치

-> created 훅을 사용해서 이벤트리스너 역할(eventBus.$on())을 하도록 함

```javascript
import { eventBus } from "../main"
export default {
    data() {
        ...
    },
    created() {
        eventBus.$on('userWasEdited', *콜백함수)
    }
}
```

*vue instance 내에서 콜백함수를 쓸 때는 화살표 함수를 작성해야 this가 그대로 vue model을 가리킴.

```javascript
import { eventBus } from "../main"
export default {
    data() {
        ...
    },
    created() {
        eventBus.$on('userWasEdited', () => {
        
        })
    }
}
```

```javascript
import { eventBus } from "../main"
export default {
    data() {
        ...
    },
    created() {
        eventBus.$on('userWasEdited', (date) => {
        	this.editedDate = date
        })
    }
}
```



```javascript
// main.js

export const eventBus = new Vue()
// eventBus라는 새로운 vue 인스턴스를 만들고 export
```

```javascript
// UserEdit.vue - script

eventBus.$emit("userWasEdited", new Date())
// eventBus에 'userWasEdited'라는 신호를 보냄
// emit은 원래 자식 컴포넌트에서 부모 컴포넌트로 신호를 보내는 거니까
// eventBus라는 vue 인스턴스가 부모 컴포넌트 역할을 한다고 추론 가능
// 즉, 형제 컴포넌트 사이를 이어주는 가상의 부모 컴포넌트 역할

```



만약에 만들어놓은 eventBus가 여러 군데서 사용이 되고 있다면, eventBus에서 사용되는 어떤 함수나 기능들을 한 군데에 만들어 놓고, 그 함수를 불러와서 사용하면 좋을 것이다. 그리고 eventBus는 이 기능을 제공하는데,

eventBus는 새로운 vue 인스턴스이기 때문에 

```javascript
// main.js

export const eventBus = new Vue({
  methods: {
    userWasEdited(date) {
      this.$emit('userWasEdited', date)
    }
  }
})
```

```javascript
// UserEdit.vue - script

// eventBus.$emit("userWasEdited", new Date()); 를 아래처럼 바꿀 수 있음
eventBus.userWasEdited(new Date());
```

