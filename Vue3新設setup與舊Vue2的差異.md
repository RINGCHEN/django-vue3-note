# 先瞭解Vue2 里的内容，怎么用 Vue3 的方式写出来
## data——唯一需要注意的地方
整个 data 这一部分的内容，你只需要记住下面这一点。
以前在 data 中创建的属性，现在全都用 ref() 声明。
在 template 中直接用，在 script 中记得加 .value 。

## method的差異
### Vue2
<script>
export default {
  methods: {
    onClick() {
      console.log('clicked')
    },
  },
}
</script>

### Vue3
<script setup>

// 注意这部分
const onClick = () => {
  console.log('clicked')
}
</script>

## props
Vue3 声明 props 我们可以用 defineProps()
### Vue2
<script>
export default {
  props: {
    foo: String,
  },
  created() {
    console.log(this.foo);
  },
}
</script>
### Vue3
<script setup>

// 注意这里  直接將props變成一個常數變數，以物件模式呈現
const props = defineProps({
  foo: String
})

// 在 script 标签里使用
console.log(props.foo)
</script>

## emits 事件
与 props 相同，声明 emits 我们可以用 defineEmits()
### Vue2
<script>
export default {

  emits: ['click'], // 注意这里
  methods: {
    onClick() {
      this.$emit('click'); // 注意这里
    },
  },
 
}
</script>
### Vue3
<script setup>
// 注意这里
const emit = defineEmits(['click']);  //用[]括起來，並用''單引號列為字串

const onClick = () => {
  emit('click') // 注意这里  用()加上''單引號
}
</script>

## computed
### Vue2
<script>
export default {
  data() {
    return {
      value: 'this is a value',
    };
  },
  computed: {
    reversedValue() {
      return value
        .split('').reverse().join('');
    },
  },
}
</script>

### Vue3
<script setup>
import {ref, computed} from 'vue'
const value = ref('this is a value')

// 注意这里  也是用const來宣稱變數，用箭頭函式來運行
const reversedValue = computed(() => {
  // 使用 ref 需要 .value
  return value.value
    .split('').reverse().join('');
})

</script>

## watch
两种写法的区别是：
## watch 
需要你明确指定依赖的变量，才能做到监听效果。

## watchEffect 
会根据你使用的变量，自动的实现监听效果。

## watch
### Vue2
<template>
  <div>{{ count }}</div>
  <div>{{ anotherCount }}</div>
  <button @click="onClick">
    增加 1
  </button>
</template>

<script>
export default {
  data() {
    return {  
      count: 1,
      anotherCount: 0,
    };
  },
  methods: {
    onClick() {
      this.count += 1;
    },
  },
  watch: {
    count(newValue) {
      this.anotherCount = newValue - 1;
    },
  },
}
</script>

### Vue3
<template>
  <div>{{ count }}</div>
  <div>{{ anotherCount }}</div>
  <button @click="onClick">
    增加 1
  </button>
</template>

<script setup>
import { ref, watch } from 'vue';

const count = ref(1);
const onClick = () => {
  count.value += 1;
};

const anotherCount = ref(0);

// 注意这里
// 需要在这里，
// 明确指定依赖的是 count 这个变量
watch(count, (newValue) => {
  anotherCount.value = newValue - 1;
})

</script>
### watch() 默认是懒侦听的，即仅在侦听源发生变化时才执行回调函数。

### 第一个参数是侦听器的源。
这个来源可以是以下几种：

一个函数，返回一个值
一个 ref
一个响应式对象
...或是由以上类型的值组成的数组
### 第二个参数是在发生变化时要调用的回调函数。
这个回调函数接受三个参数：新值、旧值，以及一个用于注册副作用清理的回调函数。
### 第三个可选的参数是一个对象

与 watchEffect() 相比，watch() 使我们可以：

懒执行副作用；
更加明确是应该由哪个状态触发侦听器重新执行；
可以访问所侦听状态的前一个值和当前值。
## watchEffect
### Vue2
無

### Vue3
<template>
  <div>{{ count }}</div>
  <div>{{ anotherCount }}</div>
  <button @click="onClick">
    增加 1
  </button>
</template>

<script setup>
import { ref, watchEffect } from 'vue';

const count = ref(1);
const onClick = () => {
  count.value += 1;
};

const anotherCount = ref(0);

// 注意这里
watchEffect(() => {
  // 会自动根据 count.value 的变化，
  // 触发下面的操作
  anotherCount.value = count.value - 1;
})

</script>

作者：Wetoria
链接：https://juejin.cn/post/7225267685763907621
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


## watchEffect()​
立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行。
### 第一个参数就是要运行的副作用函数。
这个副作用函数的参数也是一个函数，用来注册清理回调。
### 第二个参数是一个可选的选项
可以用来调整副作用的刷新时机或调试副作用的依赖。


## 生命周期
Vue3 里，除了将两个 destroy 相关的钩子，改成了 unmount，剩下的需要注意的，就是在 <script setup> 中，不能使用 beforeCreate 和 created 两个钩子。
###  setup 里，用 on 开头
如果你熟悉相关的生命周期，只需要记得在 setup 里，用 on 开头，加上大写首字母就行。
### 选项式 api 写法
<template>
  <div></div>
</template>

<script>
export default {
  beforeCreate() {},
  created() {},
  
  beforeMount() {},
  mounted() {},
  
  beforeUpdate() {},
  updated() {},
  
  // Vue2 里叫 beforeDestroy
  beforeUnmount() {},
  // Vue2 里叫 destroyed
  unmounted() {},
  
  // 其他钩子不常用，所以不列了。
}
</script>

作者：Wetoria
链接：https://juejin.cn/post/7225267685763907621
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 组合式 api 写法
<template>
  <div></div>
</template>


<script setup>
import {
  onBeforeMount,
  onMounted,

  onBeforeUpdate,
  onUpdated,

  onBeforeUnmount,
  onUnmounted,
} from 'vue'

onBeforeMount(() => {})
onMounted(() => {})

onBeforeUpdate(() => {})
onUpdated(() => {})

onBeforeUnmount(() => {})
onUnmounted(() => {})
</script>







「依賴」通常指的是响应式系统中的依赖追踪（Dependency Tracking）Vue 使用响应式系统来跟踪数据的变化，并在数据发生变化时更新相关的视图。当一个响应式数据被使用时，Vue 会自动追踪它，并建立起一个依赖关系。


## 作者：Wetoria
链接：https://juejin.cn/post/7225267685763907621
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
