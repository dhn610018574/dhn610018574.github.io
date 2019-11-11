---
title: Vue Function-based API RFC
---
### 基本例子
```
<!-- 此处指已发布3.0版本的写法 如果还没有升级为vue3.0 那么应该这样引入 -->
<!-- 安装 npm install @vue/composition-api --save -->
<!-- import { value, computed, watch, onMounted } from '@vue/composition-api' -->

import { value, computed, watch, onMounted } from 'vue'

const App = {
  template: `
    <div>
      <span>count is {{ count }}</span>
      <span>plusOne is {{ plusOne }}</span>
      <button @click="increment">count++</button>
    </div>
  `,
  setup() {
    // reactive state
    const count = value(0)
    // computed state
    const plusOne = computed(() => count.value + 1)
    // method
    const increment = () => { count.value++ }
    // watch
    watch(() => count.value * 2, val => {
      console.log(`count * 2 is ${val}`)
    })
    // lifecycle
    onMounted(() => {
      console.log(`mounted`)
    })
    // expose bindings on render context
    return {
      count,
      plusOne,
      increment
    }
  }
}

```