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

### 1、setup

setup() 函数是 vue3 中专门为组件提供的新属性，它为我们的使用 vue3 的 Composition API 新特性提供了统一的入口。

## 1.1 执行时机

setup 函数会在 beforeCreate 之后、created 之前执行。

## 1.2 接收 props 数据

    1、在props中定义当前组件允许外界传递过来的参数名称：
    ```
    props: {
      p1: String
    }
    ```
    2、通过setup 函数的第一个形参，接收props数据
    ```
    setup(props){
      console.log(props.p1)
    }
    ```

## 1.3 context

setup 函数的第二个形参是一个上下文对象，这个上下文对象中包含了一些有用的属性，这些属性在 vue2.x 中需要通过 this 才能访问，在 vue3.x 中，它们的访问方式如下：

```
const MyComponent = {
  setup(props,context){
    context.attrs
    context.slots
    context.parent
    context.root
    context.emit
    context.refs
  }
}
```

注意：在 setup() 函数中无法访问到 this

### 2 reactive

reactive() 函数接收一个普通对象，返回一个响应式的数据对象

## 2.1 基本语法

等价于 vue2.x 中的 Vue.observable() 函数，vue3.x 中提供了 reactive() 函数，用来创建响应式的数据对象，基本代码示例如下：

```
import {reactive} from '@vue/composition-api'
<!-- 创建响应式数据对象，得到state类似于vue2.x中 data() 返回的响应式对象 -->
const state = reactive({count:0})
```
## 2.2 定义响应式数据供template使用
1、按需导入reactive
```
import {reactive} from '@vue/compositon-api'

```
2、在setup() 函数中调用 reactive() 函数，创建响应式数据对象
```
setup(){
  const state = reactive({count:0})
  return state
}
```
3、在template中访问响应式数据
```
<p>当前的count值为：{{count}}</p>
```
### 3.ref
## 3.1 基本语法
ref() 函数用来根据给定的值创建一个 `响应式的数据对象`, ref()函数调用的返回值是一个对象，这个对象上只包含一个`.value`属性：
