<template>
  <div>
    <ul class="types">
      <li :class="value==='-' && 'selected'" @click="selectType('-')" >支出</li>
      <li :class="value==='+' && 'selected'" @click="selectType('+')">收入</li>
    </ul>
  </div>
</template>
<script lang="ts">
  import Vue from 'vue' 
  import { Component,Prop,Watch } from 'vue-property-decorator';
  @Component
  export default class Types extends Vue {
    //  type='-'//'-' 表示支出 ‘+’表示收入
     @Prop() readonly value!:string;
     // Prop 告诉Vue xxx不是data 是prop
     // number 告诉Vue xxx是个Number
     selectType(type:string){
      if(type!=='-' && type!=='+'){
        throw new Error('type is unkonwn')
      }
      this.$emit('update:value', type);
    }
    // @Watch('type')
    // onTypeChanged(value:string){
    //    this.$emit('update:value', value);
    // }
    // mounted(){
    //   if(this.xxx===undefined ){
    //     console.log("没有XXX")
    //   }else{
    //     console.log("this.xxx",this.xxx.toString());
    //   }
    // }
  }

// export default {
//   name: "Types",
//   props:['xxx'],
//   data(){
//     return{
//       type:'-' //'-' 表示支出 ‘+’表示收入
//     }
//   },
//   mounted(){
//     console.log(this.xxx)
//   },
//   methods:{
//     selectType(type){
//       if(type!=='-' && type!=='+'){
//         throw new Error('type is unkonw')
//       }
//       this.type=type
//     }
//   }
// };
</script>
<style lang="scss" scoped>
.types {
  background: #c4c4c4;
  display: flex;
  text-align: center;
  > li {
    width: 50%;
    line-height: 64px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    &.selected::after {
      content: "";
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 4px;
      background: #333;
    }
  }
}
</style>
