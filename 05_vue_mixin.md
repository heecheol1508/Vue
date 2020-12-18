# Mixin

여러 컴포넌트에서 불러와서 재사용할 수 있도록 하는 기능? 방법?



Mixin을 통해 컴포넌트 내에 들어간 함수나 데이터는

vue instance내에서 this를 통해 불러올 수 있다!

```javascript
// src/mixins/dateFormat.js

export const dateFormat = {
    data() {
        return {
            mixinData: '믹스인데이터'
        }
    },
    methods: {
        getDateAndTime(date) {
            if (date !== null) {
                let hour = date.getHours();
                let minutes = date.getMinutes();
                let fullDate = `${date.getFullYear()}/${date.getMonth() + 1
                    }/${date.getDate()}`;
                return `${fullDate} ${hour}:${minutes}`;
            } else {
                return null;
            }
        },
    }
}
```

```vue
// User.vue

<template>
  <div class="blue lighten-3 pa-3">
    <p>{{ getDateAndTime(createdAt) }}</p>
    <p>{{ helloToMixin }}</p>
  </div>
</template>

<script>
import { dateFormat } from "../mixins/dateFormat";	// 객체로 가져오기

export default {
  data() {
    return {
      createdAt: null,
    };
  },
  created() {
    this.createdAt = new Date();
  },
  mixins: [dateFormat],
  computed: {
    helloToMixin() {
      return this.mixinData + " <- this.mixinData로 사용가능...";
    },
  },
};
</script>
```



만약에 User 컴포넌트 내에 mixin에 있는 methods 내 함수와 같은 이름의 함수가 있다면??

mixin에 있는 methods 내 함수보다 User 컴포넌트(현재 컴포넌트) 내의 함수가 우선시된다.

즉 mixin이 실행되고 user컴포넌트가 실행 -> **오버라이딩**