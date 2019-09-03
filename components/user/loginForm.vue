<template>
  <el-form
    :model="Form"
    :rules="rules"
    ref="Form"
    
    class="form"
  >
    <el-form-item  prop="username" class="form-item">
      <el-input v-model="Form.username"></el-input>
    </el-form-item>
    <el-form-item  prop="passpass" class="form-item">
      <el-input v-model="Form.password"></el-input>
    </el-form-item>
<p>
    <nuxt-link to="#" class="form-text">忘记密码</nuxt-link>
</p>
    <el-form-item>
      <el-button type="primary" @click="login()" class="submit">登录</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
    data(){
        return{
            Form:{
                username: '',
                password: ''
        },
        rules:{
            username:[
               { required: true, message: '请输入用户名', trigger: 'blur' }
            ],
            password:[
               { required: true, message: '请输入密码   ', trigger: 'blur' }
            ]
        }
        }
    },
    methods:{

        login(){
            // console.log(this.Form)
            this.$refs['Form'].validate((valid)=>{
                if(valid){
                    this.$axios({
                url:'/accounts/login',
                method:'POST',
                data:this.Form
            }).then(res=>{
                  this.$message({
          message: '登录成功',
          type: 'success'
        });
            }).catch(err=>{
                console.dir(err)
                  this.$message.error('请输入正确的用户名和密码')
            })
                }
            })
            
        }
    }
};
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>