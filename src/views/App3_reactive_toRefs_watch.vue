<template>
  <div class="about">
    <h2>state.msg: {{msg}}</h2>
    <h2>state.numbers[1]: {{numbers[1]}}</h2>
    <h2>state.person.name: {{person.name}}</h2>
    <h2>count: {{count}}</h2>
    <button @click="update">更新</button>
  </div>
</template>

<script lang="ts">
  import {reactive, toRefs, ref, watch} from 'vue'

  interface State {
    msg: string;
    numbers: number[];
    person: {
      name?: string;
    };
  }

  export default {

    
    setup () {

      const count = ref(0)

      // state对象是reactive中原始对象的代理对象
      // 一旦操作代理对象的属性, 内部操作的是原始对象的属性
      const state: State = reactive({
        msg: 'abc',
        numbers: [1, 2, 3],
        person: {
          // name: 'tom'
        },

        update: () => {
          state.msg += '--'
          state.numbers[1] += 1  // 根据下标替换数组中的元素: 在vue2中不会自动更新, 但在vue3会自动
          state.person.name += '++'  // 给响应式对象添加一个新属性:  在vue2中不会自动更新, 但在vue3会自动
          count.value++
        }
      })

      // 问题: reactive响应式对象一旦解构出来的属性不是响应式
      // 解决: 使用toRefs将对象中的每个属性都变为ref对象
      const stateRef = toRefs(state) // 将对象中的每个属性都变为ref对象

      // 监视一个ref
      watch(count, (newValue, oldValue) => {
        console.log('watch count', newValue, oldValue)
        document.title = count.value + ''
      })
      // 监视reactive对象中的某个属性: msg  注意: 指定返回它的getter
      watch(() => state.msg, (newValue, oldValue) => {
        console.log('watch msg', newValue, oldValue)
      })

      // 监视多个数据
      watch([count, () => state.msg, stateRef.msg, state], (values) => {
        console.log('watch msg & count', values)
      })

      state.msg
      
       
      return {
        // ...state
        ...stateRef,
        count
      }
    }
  }
</script>
