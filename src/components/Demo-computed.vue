<template>
  <div>
    姓:<input type="text" v-model="data.firstName" />
    <br> 名:<input
      type="text"
      v-model="data.lastName"
    />
    <br>

    全名:{{data.fullname}}
    <input type="text" v-model="data.myfullname">
  </div>
</template>
<script>
import { reactive,computed } from "vue";
export default {
  name: "DemoComputed",
  setup() {
    let data = reactive({
      firstName: "1",
      lastName: "2",
    });
    data.fullname = computed(()=>{
      return `${data.firstName}-${data.lastName}`
    })
    data.myfullname = computed({
      get(){
        return `${data.firstName}-${data.lastName}`
      },
      set(value){
        let arr = value.split('-')
        data.firstName = arr[0]
        data.lastName = arr[1]

      }
    })
    return {
      data,
    };
  },
};
</script>
<style lang="scss" scoped></style>
