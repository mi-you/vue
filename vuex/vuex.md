# vuex3.x

## 核心概念

```javascript
import Vuex from 'vuex';
const store = new Vuex.store({
  state: {
    count: 0,
  },
  mutations: {
    add(state, ...params) {
      state.count++;
    },
  },
  actions: {
      asyncAdd(context, ...params){
          setTimeout(() => {
              context.commit('add)
          },100)
      }
  },
  getters: {
      showNewCount(state){
          return `类似计算属性的count:${state.count}`
      }
  },
  modules: {
      a: moduleA,
      b: moduleB
  }
});
```

- `state`: 所有共享数据都放到 state 里

```javascript
// 组件访问state数据的第一种方式
this.$store.state.count;
// 组件访问state数据的第二种方式
import { mapState } from "vuex";
export default {
  computed: {
    ...mapState(["count"]),
  },
};
```

- `mutations`: 原则是只能通过 mutation 修改数据,不要这这里执行异步操作。

```javascript
// 组件调用mutation
import { mapMutations } from "vuex";
export default {
  methods: {
    add(param) {
      // 组件触发mutation的第一种方式
      this.$store.commit("add", param);
    },
    // 组件触发mutation的第二种方式
    ...mapMutations(["add"]),
    add2(param) {
      this.add(param);
    },
  },
};
```

- `actions`: 用于处理异步任务，在这里调用 mutation 函数

```javascript
import { mapActions } from "vuex";
export default {
  methods: {
    asyncAdd(param) {
      // 组件触发action的第一种方式
      this.$store.dispatch("asyncAdd", param);
    },
    // 组件触发action的第二种方式
    ...mapActions(["asyncAdd"]),
    add2(param) {
      this.asyncAdd(param);
    },
  },
};
```

- `getters`: 类似计算属性，对 state 数据进行按需处理;

```javascript
// 组件访问state数据的第一种方式
this.$store.getters.showNewCount;
// 组件访问state数据的第二种方式
import { mapGetters } from "vuex";
export default {
  computed: {
    ...mapGetters(["showNewCount"]),
  },
};
```

# VUEX4.x

## 创建使用 store 与 3.x 不同

- 创建

```javascript
import { createStore } from "vuex";
export default createStore({
  state: {},
  mutations: {},
  actions: {},
  getters: {},
});
```

- 使用

```javascript
import { useStore } from "vuex";
export default {
  // composition api vue3
  setup() {
    let store = useStore();
  },
};
```
