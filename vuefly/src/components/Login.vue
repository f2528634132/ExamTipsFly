<template>
  <div class="login_div">
  <el-form :rules="rules" class="login-container" label-position="left" :model="loginForm"
           label-width="0px" v-loading="loading">
    <h3 class="login_title">ExamTips系统登录</h3>
    <el-form-item prop="username">
      <el-input type="text" v-model="loginForm.username" prefix-icon="el-icon-user"
                auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item prop="password">
      <el-input type="password" v-model="loginForm.password" prefix-icon="el-icon-lock"
                auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <el-radio v-model="type" label="0" >个人</el-radio>
    <el-radio v-model="type" label="1"  >管理员</el-radio>
    <el-form-item style="width: 100%">
      <el-button type="primary" style="width: 100%" @click="submitClick">登录</el-button>
    </el-form-item>
  </el-form>
  </div>
</template>

<script>

    import { mapMutations } from 'vuex';
  export default{
    data(){
      return {
          type: '1',
        rules: {
          username: [{required: true, message: '请输入用户名', trigger: 'blur'}],
          password: [{required: true, message: '请输入密码', trigger: 'blur'}]
        },
        checked: true,
        loginForm: {
          username: 'fly',
          password: '123456'
        },
         userToken:'',
        loading: false
      }
    },
    methods: {
        ...mapMutations(['changeLogin']),
      submitClick: function () {
        let _this = this;
        this.loading = true;
        // this.getRequest('/auth/login?userName='+this.loginForm.username+'&passWord='+this.loginForm.password+'&type='+this.radio).then(resp=> {
          this.getRequest(`/auth/login?userName=${this.loginForm.username}&passWord=${encodeURIComponent(this.utils.encrypt(this.loginForm.password.replace(/(^\s*)|(\s*$)/g, ""),"aA0fR0fA5rF3cH1h"))}&type=${this.type}`).then(resp=> {
          _this.loading = false;
          if (resp && resp.status == 200) {
            let data = resp.data;
              _this.userToken = data.data.token;
              this.GLOBAL.token = data.data.token;
              this.GLOBAL.id = data.data.id;
              this.GLOBAL.name = data.data.name;
              // 将用户token保存到vuex中
              // this.$store.commit('changeLogin', _this.userToken);
              _this.changeLogin({ Authorization: _this.userToken });
              if (0 === data.data.type){
                this.$router.push({
                    path: '/Home',
                })
            }else{
                this.$router.push({
                    path: '/Admin',
                    // query: {'name':data.data.name}

                })
            }
            // _this.$store.commit('login', data.obj);
            // var path = _this.$route.query.redirect;
            /*_this.$router
              .replace({path: path == '/' || path == undefined ? '/home' : path});*/
          }
        });
      }
    }
  }
</script>
<style>
  .login_div{
    background-color: azure;
    height: 100%;
  }
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    position: absolute;
    left: 50%;
    top: 40%;
    transform: translate(-50%,-50%);
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }
  .login_remember {
    margin: 0px 0px 35px 0px;
    text-align: left;
  }
  .el-radio {
    color: #606266;
    cursor: pointer;
    margin-right: 30px;
    margin-bottom: 20px;
    margin-left: 60px;
  }
  .el-input--prefix .el-input__inner {
    padding-left: 40px;
  }

</style>
