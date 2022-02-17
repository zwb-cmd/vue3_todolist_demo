import { ref,reactive,watch,watchEffect,computed,shallowRef,shallowReadonly,shallowReactive,readonly,toRef,toRefs,toRaw,markRaw,provide,inject } from "vue";
// ref简单类型响应式数据
// reactive简单类型响应式数据
// watch侦听
// watchEffect使用谁侦听谁
// computed计算属性
// shallowRef浅层ref
// shallowReactive浅层reactive
// readonly只读
// shallowReadonly浅层只读
// toRef拿到对象上的对象值（响应式）
// toRefs拿到对象上（响应式）
// toRaw响应式数据变成普通数据
// markRaw标记响应式数据变成普通数据（不影响页面展示变化）
// provide定义组件共享数据
// inject使用组件共享数据