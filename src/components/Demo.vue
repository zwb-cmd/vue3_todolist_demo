<template>
  <h1>我的demo</h1>
  <h2>接收参数{{age}}</h2>
  <slot name="name"></slot>
  <button @click="showName">chufa</button>
  <slot name="footer"></slot>
</template>
<script>
import {reactive} from 'vue'
export default {
  name:'Demo',
  props:['age'],
  emits:['myname'],
  setup(props,context){
    let data = reactive({
      age:props.age
    })
    console.log(props)
    console.log(context)
    console.log(context.emit)
    console.log(context.attrs)
    console.log(context.slots)
    data.showName = ()=>{
      context.emit('myname')
    }
    return {
      ...data,
    }
  }
}
</script>
<style lang="scss" scoped>
  
</style>