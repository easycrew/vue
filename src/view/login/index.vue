<template>
  <div class="login-container">
      <el-form class="card-box login-form" autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left">
        <h3 class="title">系统登录</h3>
        
        <el-form-item prop="username">
            <span class="svg-container svg-container_login">
                <icon-svg icon-class="user" />
            </span>
            <el-input name="username" type="text" v-model="loginForm.username" autoComplete="on" placeholder="邮箱" />
        </el-form-item>

        <el-form-item prop="password">
            <span class="svg-container">
                <icon-svg icon-class="password" />
            </span>
            <el-input name="password" :type="PwdType" @keyup.enter.native="handleLogin" v-model="loginForm.password" autoComplete="on" placeholder="密码"/>
            <span class="show-pwd" @click="showPwd">
                <icon-svg icon-class="eye"/>
            </span>
        </el-form-item>

        <el-button type="primary" style="width:100%;margin-bottom:30px; " :loading="loading" @click.native.prevent="handleLogin">登录</el-button>

        <el-button class="thirdparty-button" type="primary" @click="showDialog=true">打开第三方登录</el-button>
      </el-form>
      <el-dialog title="三方验证" :visible.sync="showDialog">
          登陆成功<br>
          <social-sign />
      </el-dialog>
  </div>
</template>
>
<script>
import { isvalidUsername } from '@/utils/validate'
import socialSign from './socialsignin'

export default {
  components: { socialSign },
  name: 'login',
  data () {
    const validateUsername = (rule, value, callback) => {
      if (!isvalidUsername(value)) {
        callback(new Error('请输入正确的用户名'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('密码不能小于6位'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'admin',
        password: '1111111'
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      pwdType: 'password',
      loading: false,
      showDialog: false
    }
  },
  method: {
    showPwd () {
      if (this.pwdType === 'password') {
        this.pwdType = ''
      } else {
        this.pwdType = 'password'
      }
    },
    handleLogin () {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$tore.dispatch('LoginByUsername', this.loginForm)
              .then(() => {
                this.loading = false
                this.$router.push({path: '/'})
              }).catch(() => {
                this.loading = false
              })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    afterQRScan () {

    },
    created () {

    },
    destroyed () {

    }
  }

}
</script>
