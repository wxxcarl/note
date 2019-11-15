# Vue小技巧

### 一、 监听子组件的生命周期

#### 1. 手动$emit
````
// Parent.vue
<Child @mounted="doSomething"/>

// Child.vue
mounted() {
  this.$emit("mounted");
}
````

2. 自动监听
````
//  Parent.vue
<Child @hook:mounted="doSomething" ></Child>
````
源码中，当`$on`监听的事件以`hook:`开头时，会自动触发`vm.$emit('hook:', hook)`

### 二、提高大行数据列表渲染性能
````
export default {
  data: () => ({
    users: {}
  }),
  async created() {
    const users = await axios.get("/api/users")
    this.users = users
  }
}
````
Vue 在默认情况下，会将数组 this.users 中的，所有对象的第一层属性设置为响应式数据，如果只需要展示数据的话，对性能小号较大，可以在赋值之前先冻结：
````
- this.users = users
+ this.users = Object.freeze(users)
````
该方法只会冻结值，而对引用不会有影响，仍然可以对`this.users`重新赋值


### 三、传递所有的props和events
````
<AppList v-bind="$props" v-on="$listeners"></AppList>

<script>
  import AppList from "./AppList";

  export default {
    props: AppList.props,
    components: {
      AppList
    }
  };
</script>
````
一个快速创建响应式组件的方法就是使用基础组件的 props 来定义它们。就像我在代码中所写的 `props: AppList.props`。

### 四、Vue.observable
在简单场景下不需要vuex

`store.js`示例
````
import Vue from "vue";

export const store = Vue.observable({
  count: 0
});

export const mutations = {
  setCount(count) {
    store.count = count;
  }
};
````
使用示例
```
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="setCount(count + 1);">+ 1</button>
    <button @click="setCount(count - 1);">- 1</button>
  </div>
</template>

<script>
  import { store, mutations } from "./store";

  export default {
    computed: {
      count() {
        return store.count;
      }
    },
    methods: {
      setCount: mutations.setCount
    }
  };
</script>
````

### 五、vuex的两个冷门但有用API
````
const store = new Vuex.Store({
  state: {
    count: 0
  }，
  // ...
}）
````
- `watch` 监听state变化

````
const unsubscribe = store.watch((state, getters) => {
    return [state.count, getters.getCountPlusOne];
  },
  watched => {
    console.log("Count is:", watched[0]);
    console.log("Count plus one is:", watched[1]);
  },
  {}
);

unsubscribe();
````

- `subscribeAction` 调用监听函数，在每一个 action 分发的时候调用指定的回调函数，并在其中调用自定义代码

````
const unsubscribe = store.subscribeAction({
  before: (action, state) => {
    startBigLoadingSpinner();
  },
  after: (action, state) => {
    stoptBigLoadingSpinner();
  }
});

// To unsubscribe:
unsubscribe();
````

### 六、函数式组件
````
<template functional>
  <div>
    <p v-for="item in props.items" @click="props.itemClick(item);">
      {{ item }}
    </p>
  </div>
</template>
````
更高效、更轻量，但是获取不到自身的实例

### 七、nuxt重定向404页面
当用户访问一个并不存在的页面时，我们想要将其重定向到网站主页

创建 pages/*.vue 组件

````
<!-- pages/*.vue -->
<script>
export default {
  asyncData ({ redirect }) {
    return redirect('/')
  }
}

// or 

export default {
  fetch ({ redirect }) {
    return redirect('/')
  }
}
</script>
````


<style>
    .page-header {
        display: none;
    }
</style>