<template>
  <div>
    <el-row type="flex" justify="space-between" class="banxin">
      <div class="logo">
        <img src="http://157.122.54.189:9093/images/logo.jpg" alt />
      </div>
      <el-row type="flex" class="navs">
        <nuxt-link to="/">首页</nuxt-link>
        <nuxt-link to="/post">旅游攻略</nuxt-link>
        <nuxt-link to="/hotel">酒店</nuxt-link>
        <nuxt-link to="/air">国内机票</nuxt-link>
      </el-row>
      <el-row>
        <nuxt-link to="/user/login" v-if="!$store.state.user.userInfo.token">登录/注册</nuxt-link>
         
        <el-dropdown v-if="$store.state.user.userInfo.token">
    <el-row type="flex" align="middle" class="el-dropdown-link">
        <nuxt-link to="#">
            <img :src="$axios.defaults.baseURL + $store.state.user.userInfo.user.defaultAvatar" id="img" />
            {{$store.state.user.userInfo.user.nickname}} 
        </nuxt-link>
        <i class="el-icon-caret-bottom el-icon--right"></i>
    </el-row>
    <!-- 其他代码... -->
</el-dropdown>
      </el-row>
    </el-row>
  </div>
</template>

<script>
export default {
    mounted(){
        console.log(this.$store.state.user.userInfo)
    }
};
</script>

<style lang="less" scoped>
.el-dropdown-link{
    a{
        padding: none !important;
    }
    #img{
        width: 32px ;
        height: 32px;
        border-radius: 50%;
        display: inline;
        vertical-align: middle ;
    }
        
    
}
.banxin {
  width: 1000px;
  margin: 0 auto;
  height: 60px;

  .navs {
    flex: 1;
    margin: 0 20px;
    .nuxt-link-exact-active {
      background-color: #409eff;
      color: #fff;
      &:hover {
        color: #fff;
        border: #fff;
      }
    }
    a {
      box-sizing: border-box;
      &:hover {
        color: #409eff;
        border-bottom: #409eff solid 5px;
      }
    }
  }
  a {
    display: block;
    padding: 0px 20px;
    height: 60px;
    line-height: 60px;
  }
  .logo {
    height: 60px;
    width: 156px;
    margin-top: 10px;
  }
  img {
    width: 100%;
    display: block;
  }
}
</style>