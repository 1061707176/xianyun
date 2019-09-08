<template>
  <div class="search-form">
    <!-- 头部tab切换 -->
    <el-row type="flex" class="search-tab">
      <span
        v-for="(item, index) in tabs"
        :key="index"
        @click="handleSearchTab(item, index)"
        :class="{active: index === currentTab}"
      >
        <i :class="item.icon"></i>
        {{item.name}}
      </span>
    </el-row>
    <el-form class="search-form-content" ref="form" label-width="80px">
      <el-form-item label="出发城市">
        <el-autocomplete
        v-model="form.departCity"
            class="el-autocomplete"
          :fetch-suggestions="querySearchAsync"
          placeholder="请输入目标城市"
          @select="handleSelect"
        ></el-autocomplete>
      </el-form-item>
      <el-form-item label="到达城市">
                <el-autocomplete
                :fetch-suggestions="queryDestSearch"
                placeholder="请搜索到达城市"
                @select="handleDestSelect"
                class="el-autocomplete"
                v-model="form.destCity"
                ></el-autocomplete>
            </el-form-item>
            <el-form-item label="出发时间">
                <!-- change 用户确认选择日期时触发 -->
                <el-date-picker type="date" 
                v-model="form.departDate"
                placeholder="请选择日期" 
                style="width: 100%;"
                @change="handleDate">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="">
                <el-button style="width:100%;" 
                type="primary" 
                icon="el-icon-search"
                @click="handleSubmit">
                    搜索
                </el-button>
            </el-form-item>
            <div class="reverse">
                <span @click="handleReverse">换</span>
            </div>
    </el-form>
    
  </div>
</template>
<script>
import moment from 'moment'
export default {
  data() {
    return {
      tabs: [
        { name: "单程", icon: "iconfont icondancheng" },
        { name: "往返", icon: "iconfont iconshuangxiang" }
      ],
      currentTab: 0,
        cityname:'',


      form: {
        departCity: "",
        departCode:'',
        destCity:'',
        destCode:'',
        departDate:''
      }
    };
  },
  methods: {
    handleSearchTab(item, index) {
      this.currentTab = index;
      console.log(this.currentTab);
      if (index === 1) {
        this.$alert("目前不支持往返", "提示", {
          confirmButtonText: "确定",
          type: "error"
        });
        console.log(this.currentTab);
        this.currentTab = 0;
      }
    },
    querySearchAsync(val,cb) {
      // console.log(val)
      if(!val){
                // 传递空数组不会出现下拉框
                cb([]);
                return;
            }else{
       
        //  setTimeout(() => {
        this.form.departCity = val;
        this.$axios({
          url: "/cities",
          params: {name:val}
        }).then(res => {
            // console.log(res)
            let arr=res.data.data
            console.log(arr)
            let cbarr=[]
          arr.forEach(e=>{
              e.value=e.name.replace('市','')
              cbarr.push(e)
              });
            //   console.log(cbarr)
                  cb(cbarr)
          })
          }
        //  },200)
      
      //   console.log(this.search);
    },
    queryDestSearch(val,cb){ 
         if(!val){
                // 传递空数组不会出现下拉框
                cb([]);
                return;
            }else{
       
        //  setTimeout(() => {
        this.form.destCity = val;
        this.$axios({
          url: "/cities",
          params: {name:val}
        }).then(res => {
            // console.log(res)
            let arr=res.data.data
            console.log(arr)
            let cbarr=[]
          arr.forEach(e=>{
              e.value=e.name.replace('市','')
              cbarr.push(e)
              });
              console.log(cbarr)
                  cb(cbarr)
          })
          }
    },
    handleSelect(item){
        console.log(item)
        this.form.departCode=item.sort
    },
    handleDate(item){
// console.log(item)
 this.form.departDate = moment(item).format(`YYYY-MM-DD`);
 console.log(this.form.departDate)
    },
    handleDestSelect(item){
        this.form.destCode=item.sort
        // console.log(this.form.destCode,'~~~~~~')
    },
    handleSubmit(){
        let {departCity ,destCity,departDate} =this.form
        if(!departCity){
            this.$alert('出发城市不能为空','提示',{type:'error'});
            return 
        }
        if(!destCity){
            this.$alert('到达城市不能为空','提示',{type:'error'})
            return
        }
        if(!departDate){
            this.$alert('出发时间不能为空','提示',{type:'error'})
            return
        }
        this.$router.path({
            path:'/air/flights',
            query:this.form
        })
    },
    handleReverse(){
          const {departCity, departCode, destCity, destCode} = this.form;
        this.form.departCity=destCity
        this.form.departCode=destCode
        this.form.destCity=departCity
        this.form.destCode=departCode
    }
  },
  mounted() {}
};
</script>
<style scoped lang="less">
.search-form {
  border: 1px #ddd solid;
  border-top: none;
  width: 360px;
  height: 350px;
  box-sizing: border-box;
}

.search-tab {
  span {
    display: block;
    flex: 1;
    text-align: center;
    height: 48px;
    line-height: 42px;
    box-sizing: border-box;
    border-top: 3px #eee solid;
    background: #eee;
  }

  .active {
    border-top-color: orange;
    background: #fff;
  }

  i {
    margin-right: 5px;
    font-size: 18px;

    &:first-child {
      font-size: 16px;
    }
  }
}

.search-form-content {
  padding: 15px 50px 15px 15px;
  position: relative;

  .el-autocomplete {
    width: 100%;
  }
}

.reverse {
  position: absolute;
  top: 35px;
  right: 15px;

  &:after,
  &:before {
    display: block;
    content: "";
    position: absolute;
    left: -35px;
    width: 25px;
    height: 1px;
    background: #ccc;
  }

  &:after {
    top: 0;
  }

  &:before {
    top: 60px;
  }

  span {
    display: block;
    position: absolute;
    top: 20px;
    right: 0;
    font-size: 12px;
    background: #999;
    color: #fff;
    width: 20px;
    height: 20px;
    line-height: 18px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;

    &:after,
    &:before {
      display: block;
      content: "";
      position: absolute;
      left: 10px;
      width: 1px;
      height: 20px;
      background: #ccc;
    }

    &:after {
      top: -20px;
    }

    &:before {
      top: 20px;
    }
  }
}
</style>