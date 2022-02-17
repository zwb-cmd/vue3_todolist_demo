<template>
  {{ a }}
  <button @click="a++">增加</button>
  {{data.a}}
  <button @click="data.a++">增加1</button>
</template>
<script>
import { ref,reactive,watch,watchEffect,shallowRef,shallowReadonly,shallowReactive,readonly,toRef,toRefs,toRaw,markRaw} from "vue";
// ref简单类型响应式数据
export default {
  name: "DemoWatch",
  setup() {
    let data = reactive({
      a:1,
      b:2,
    })
    let a = ref(0)
    let b = ref(0)
    /* // reactive-Warch
    watch(()=>data.a,(newValue,oldValue)=>{
      console.log(newValue,oldValue)
    })
    // 普通的ref
    watch(a,(newValue,oldValue)=>{
      console.log(newValue,oldValue)
    })
    watchEffect(()=>{
      console.log('111')
    }) */
    // 侦听多个源
    /* watch([a,()=>data.a],(newarr,oldarr)=>{
      console.log(newarr,oldarr)
    },{
      immediate:true
    }) */
    /* watch(data,(newValue,oldValue)=>{
      console.log(newValue,oldValue)
    },{
      deep:true
    }) */
    return {
      data,
      a,
      ...toRefs(data),
    };
  },
};
</script>
<style lang="scss" scoped></style>
