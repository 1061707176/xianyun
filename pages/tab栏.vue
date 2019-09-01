<template>
  <div>


      <span v-for="(arr,index) in ['首页','傻事','趣事'] " :key="index" @click="har(index)" :class="{active:i===index}">{{arr}}</span>
      


      <ul v-show="i===0">
          <li>新闻一号</li>
          <li>二号</li>
          <li>三号</li>
      </ul>
      <ul v-show='i===1'>
          <li>趣事</li>
          <li>二号</li>
          <li>三号</li>
      </ul>
      <ul v-show="i===2">
          <li>啥事</li>
          <li>二号</li>
          <li>三号</li>
      </ul>
  </div>
</template>

<script>
export default {
data(){
    return{
        i:0
    }
},
methods:{
    har(index){
        this.i=index
    }
}
}
</script>

<style lang="less" scoped>
.active{
    color:red
}
</style>