<template>
<div class="login_container" >
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>

            <el-form ref='loginFormRef' :model='form' :rules="loginrules" label-width="0px" class='login_form'>
  <el-form-item  prop='username'>
    <el-input prefix-icon="el-icon-user-solid" v-model="form.username"></el-input>
  </el-form-item>
    <el-form-item prop='password' >
    <el-input type="password" prefix-icon="el-icon-s-goods" v-model='form.password'></el-input>
  </el-form-item>
      <el-form-item class='btns'>
  <el-button type="primary" @click='formsubmit'>登录</el-button>
    <el-button type="info" @click='resetform'>重置</el-button>
  </el-form-item>

            </el-form>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        username: 'admin',
        password: '123456'
      },
      loginrules: {
        username: [
          { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 6 到 10 个字符', trigger: 'blur' }],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }]
      }
    }
  },
  methods: {
    resetform () {
      this.$refs.loginFormRef.resetFields()
    },
   formsubmit () {
      this.$refs.loginFormRef.validate(async validata => {
       if (!validata) return this.$message.error('请检查输入')
       const { data: ret } = await this.$http.post('login',this.form)
        if (ret.meta.status !== 200) return this.$message.error('登录失败')
       this.$message.success('登陆成功')
        window.sessionStorage.setItem('token',ret.data.token)
        this.$router.push('/home')
      })
  //      window.sessionStorage.setItem('token','ret.data.token')
  //      this.$router.push('/home')
    },
 submit: function (e) {
      alert(e)
  }
  },
  created () {

  },
  mounted () {
    const that = this
    document.onkeydown = function (e) {
      const ev = document.all ? window.event : e
      if (ev.keyCode === 13) {
        that.formsubmit()
      }
    }
  }
}
</script>

<style lang="less" scoped>
.login_container{
    background-color: #2b4b6b;
    height: 100%;
    .login_box{
        width: 450px;
        height: 300px;
        background-color: #fff;
        position:absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        .avatar_box{
         height: 130px;
         width: 130px;
         border: 1px #eee solid;
         border-radius: 50%;
         padding: 10px;
         box-shadow: 0 0 10px #ddd;
         position: absolute;
         left: 50%;
         transform: translate(-50%,-50%);
         background-color: #fff;
         img{
             width: 100%;
             height: 100%;
            border-radius: 50%;
            background-color: #eee;
         }
        }
    }
    .login_form{
        position: absolute;
        bottom:0;
        width: 100%;
        box-sizing: border-box;
        padding: 0 20px;
    }
    .btns{
        display: flex;
        justify-content: flex-end;
    }
}
</style>
