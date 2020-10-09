<template>
  <div class="about">
    <!-- 模板中读取ref对象时, 会自动读取其value显示 -->
    <h2>{{count}}</h2>
    <button @click="increment">加1</button>
  </div>
</template>

<script lang="ts">
  import {ref} from 'vue'
  export default {

    beforeCreate () {
      console.log('beforeCreate()')
    },

    /* 
    在beforeCreate()之前执行的回调函数
    不能通过this来访问组件实例对象
    一般用来提供供模板使用的响应式数据和函数(更新数据)
    */
    setup () {
       console.log('setup()', this)
      // 定义一个响应式Ref对象
      const count = ref(1) // 引用对象, 内部包含了一个value属性来存储数据
      console.log(count, count.value)

      // 用于更新响应式数据的函数
      const increment = () => {
        count.value += 1
      }

      return { // 返回对象中的所有属性和方法, 模板可以直接访问
        count,
        increment
      }
    }
  }
</script>
