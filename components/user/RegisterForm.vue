<template>
  <el-form :model="form" ref="form" :rules="rules" class="form">
    <el-form-item class="form-item" prop="username" >
      <el-input placeholder="用户名手机" v-model="form.username"></el-input>
    </el-form-item>

    <el-form-item class="form-item" prop="captcha">
      <el-input placeholder="验证码" v-model="form.captcha">
        <template slot="append" >
          <el-button @click="handleSendCaptcha()">发送验证码</el-button>
        </template>
      </el-input>
    </el-form-item>

    <el-form-item class="form-item" >
      <el-input placeholder="你的名字" v-model="form.nickname"></el-input>
    </el-form-item>

    <el-form-item class="form-item" prop="password" >
      <el-input placeholder="密码" type="password" v-model="form.password"></el-input>
    </el-form-item>

    <el-form-item class="form-item" prop="checkpass">
      <el-input placeholder="确认密码" type="password" v-model="form.checkpass"></el-input>
    </el-form-item>

    <el-button class="submit" type="primary" @click="handleRegSubmit">注册</el-button>
  </el-form>
</template>
<script>
export default {
  data() {
      let validatePass=(rule,value,callback)=>{
            if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.form.checkPass !== '') {
            this.$refs.form.validateField('checkPass');
          }
          callback();
        }
      }
      let validatePass2=(rule,value,callback)=>{
         if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.form.password) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      }
    return {
        form:{
            username:'',
            password:'',
            checkpass:'',
            nickname:'',
            captcha:''

        },
        rules:{
             username:[ { required: true, message: '请输入用户名/手机', trigger: 'blur' }],
             password:[ { validator: validatePass, trigger: 'blur' }],
             captcha:[ { required: true, message: '请输入验证码', trigger: 'blur' }],
             nickname:[ { required: true, message: '请输入昵称', trigger: 'blur' }],
             checkpass:[   { validator: validatePass2, trigger: 'blur' }]
        }
    };
  },
  methods: {
    handleSendCaptcha() {
      this.$axios({
        url: "captchas",
        method: "POST",
        data: { tel: this.form.username }
      }).then(res => {
        console.log(res);
        this.$message({
          message: `验证码请求成功:${res.data.code}`,
          type: "success"
        });
      });
    },
    handleRegSubmit(){
       if(this.form.username.length===11){
            this.$refs.form.validate(valid=>{
            if(valid){
                let {checkpass,...popp}=this.form
             this.$axios({
            url:'accounts/register',
            method:'POST',
            data:popp
        }).then(res=>{
            console.log(res)
        })
            }
        })
       }else{
           this.$message.error('手机号码格式错误')
       }
    }

  }
};
</script>
<style scoped lang="less">
.form {
  padding: 25px;
}

.form-item {
  margin-bottom: 20px;
}

.form-text {
  font-size: 12px;
  color: #409eff;
  text-align: right;
  line-height: 1;
}

.submit {
  width: 100%;
  margin-top: 10px;
}
</style>