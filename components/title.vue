<template>
  <div>
      <el-row type="flex" justify="space-between" class="banxin">
          <div class="logo">
              <img src="http://157.122.54.189:9093/images/logo.jpg" alt="">
          </div>
        <el-row type="flex" class="navs">
     <nuxt-link to="/">首页</nuxt-link> 
     <nuxt-link to="/post">旅游攻略</nuxt-link> 
     <nuxt-link to="/hotel">酒店</nuxt-link> 
     <nuxt-link to="/air">国内机票</nuxt-link> 
            
        </el-row>
        <el-row>
            <nuxt-link to="/user/login">登录/注册</nuxt-link>

        </el-row>

      </el-row>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>

.banxin{
    width: 1000px;
    margin: 0 auto;
    height: 60px;

    .navs{
        flex: 1;
       margin: 0 20px;
           .nuxt-link-exact-active{
               background-color: #409eff;
               color: #fff;
               &:hover{
                   color: #fff;
                   border: #fff;
                   
               }
           }
           a{
           box-sizing: border-box;
               &:hover{
                   color:#409eff;
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
    .logo{
        height: 60px;
        width: 156px;
        margin-top: 10px;
    }
    img{
        width: 100%;
        display: block;
    }
}
</style>