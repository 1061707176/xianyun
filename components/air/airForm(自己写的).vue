<template>
  <div class="tab">
        
<el-row type="flex" >
    <el-col :span="12" v-for="(item,index) in tabs" :key="index" :class="{tabs, al:index===curr}" @click.native="ck(index)" >
        {{item.name}}
    </el-col>
</el-row>
<el-form   ref="ruleForm" label-width="100px" class="demo-ruleForm form" >
  <el-form-item label="活动名称" >
    <el-input ></el-input>
  </el-form-item>
   <el-form-item label="活动名称" >
    <el-input ></el-input>
  </el-form-item>
   <el-form-item label="活动时间" required>
    <el-col :span="24">
      <el-form-item >
        <el-date-picker type="date" placeholder="选择日期"  style="width: 100%;"></el-date-picker>
      </el-form-item>
    </el-col>
    
  </el-form-item>
    

</el-form>
  </div>
</template>

<script>
export default {
data(){
    return{
        tabs:[
            {name:'单程', icon:'iconfont icondancheng'},
            {name:'往返',icon:'iconfont iconshuangxiang'}
        ],
        curr:0,

        
    }
},
methods:{
    ck(index){
        this.curr=index
        console.log(this.curr)
        if(this.curr===1){
             this.$alert('目前不支持往返', '提示', {
          confirmButtonText: '确定',
          type:'error',
        
        });
        this.curr=0
        console.log(this.curr)
        }
    },

}

}
</script>

<style lang="less" scoped   >
.tab{
    width: 358px;
    text-align: center;
    vertical-align: middle;
    height: 48px;
    cursor: pointer;
    .tabs{
       height: 48px;
      background-color: #eeeeee;
     line-height: 48px
    }
    .al{
         border-top: solid 2px #ffa500;
         background-color: #fff;
         
    }
   
}
.form{
    padding: 15px 50px 15px 0
}
</style>