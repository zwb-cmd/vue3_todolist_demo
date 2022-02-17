<template>
  <Vue3Todolist />
</template>

<script>
import { reactive } from 'vue'
import Vue3Todolist from './components/vue3Todolist.vue';

export default {
  name: 'App',
  components:{
    Vue3Todolist,
  },
  setup(){
    let data = reactive({
      mydata:{
        a:1,
      },
      name:'',
      sex:'男',
      arr:[],
      
    })
    data.mydata.a = 1234


   /*  let p  = {
      a:1,
      b:2,
    }
    // vue3响应式
    new Proxy(p,{
      get(target,key){
        console.log(target,key)
        return Reflect.get(target,key)
      },
      set(target,key,value){
        console.log(target,key)
        return Reflect.set(target,key,value)
      },
      deleteProperty(target,key){
        console.log(target,key)
        delete Reflect.deleteProperty(target,key)
      }
    }) */
    data.showName =()=>{
        console.log('123123')
      }
    return {
      ...data
    }
  }
}
</script>

<style>

</style>
