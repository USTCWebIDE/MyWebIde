/* eslint-disable no-undef */
/* eslint-disable camelcase */
<template>
 
  <div>
    <el-container style=" width:100%;
    height: 100%;
    background: yellow;
    position: absolute;">
     <el-header>
       <div style="display:flex;justify-content: space-between;">
         <div style="width:45%;" class="logohead"><img src="../../assets/img/box.png" width="30px" height="30px">  Sign Up</div>
         <div style="width:55%; padding-left: 25%; ">
           <div class="createProject">Create New Project</div>
        </div>
       </div>
     </el-header>
     <el-main > 
       <el-card style="background-color: rgb(21,21,21);height:60%;width: 60%;margin: auto;margin-top: 100px;">
         <div style="width: 100%;display: flex;justify-content: space-around;">
           <div style="width: 50%;">
            <div  style="color: #fff;padding: 70px;text-align: left;margin-left: 20px;">
              <h1><img src="../../assets/img/box.png" width="30px" height="30px">My WebIDE</h1>
             <h3><i class="el-icon-check" style="background-color: rgb(9,113,241);"></i><span class="h3list"> Code Anywhere</span></h3>
             <h3><i class="el-icon-check" style="background-color: rgb(9,113,241);"></i><span class="h3list"> Start Quickly</span></h3>
             <h3><i class="el-icon-check" style="background-color: rgb(9,113,241);"></i><span class="h3list"> Prototype Rapidly</span></h3>
            </div>
          </div>
           <div class="loginCard" >
            <h3 class="login-title">Sign In For More Function</h3>
            <div style="padding-top: 20px;padding-left: 20px;padding-right: 20px;">
              <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" status-icon>
                <el-form-item prop="userName">
                  <el-input v-model="dataForm.userName" placeholder="帐号"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                  <el-input v-model="dataForm.password" type="password" placeholder="密码"></el-input>
                </el-form-item>
                <el-form-item prop="captcha">
                  <el-row :gutter="20">
                    <el-col :span="12">
                      <el-input v-model="dataForm.captcha" placeholder="验证码">
                      </el-input>
                    </el-col>
                    <el-col :span="10" class="login-captcha">
                      <img :src="captchaPath" @click="getCaptcha()" alt="" width="100%" height="35px">
                    </el-col>
                  </el-row>
                </el-form-item>
                <el-form-item>
                  <el-button   class="loginButton" type="primary" @click="dataFormSubmit()">登录</el-button>
                  <el-button   class="signupButton" type="warning"@click="dialogFormVisible=true">注册</el-button>
                </el-form-item>
              </el-form>
              
            </div>

          </div>
         </div>
       </el-card>
     </el-main>
   </el-container>
 <div>
    <el-dialog title="用户注册" :visible.sync="dialogFormVisible"  width="30%" center >
      <el-form :model="signupForm" label-position="left" label-width="100px"   ref="signupForm" :rules="signupRule"
      
      @keyup.enter.native="signFormSubmit()"
      >
        <el-form-item label="用户名" width=150 prop="userName" >
          <el-input v-model="signupForm.userName"placeholder="请输入用户名" ></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
            <el-input v-model="signupForm.password"type="password"  placeholder="请输入密码"  ></el-input>
        </el-form-item>
        <el-form-item label="密码"  prop="passwordAgain">
          <el-input v-model="signupForm.passwordAgain" type="password" placeholder="再次输入密码" ></el-input>
      </el-form-item>
        <el-form-item :inline="true" prop="phone" label="手机号">
        
              <el-input v-model="signupForm.phone" placeholder="请输入手机号码"  ></el-input>
           

          <!-- <el-col :span="2">
              &nbsp
          </el-col> -->
        </el-form-item >     
          <el-form-item :inline="true" prop="checkPwd" label="验证码">
          <el-col :span="10" >
            <el-input v-model="signupForm.checkPwd" placeholder="验证码" ></el-input>
          </el-col>
          <el-col :span="10">
            <div >
              <el-button v-show="Verification" @click="handleClick" class="getCheckPwd" > 获取验证码 </el-button>
              <el-button v-show="!Verification" class="getCheckPwd">{{timer}}S后重新获取</el-button>
            </div>
          </el-col>
        </el-form-item>
         
      </el-form>

      
      <div slot="footer" class="dialog-footer">
        
        <el-button   @click="signFormSubmit" class="dialogSubmit">注 册</el-button>
        <el-button @click="dialogFormVisible = false">取 消</el-button>
      </div>
    </el-dialog>
  </div>
 
  </div>
 
</template>

<script>
  import { getUUID } from '@/utils'
  
  export default {
    data () {
      var validatePass = (rule, value, callback) => {
        if (value.length < 6 || value.length > 16) {
          callback(new Error('请保证密码长度在6-16位'))
        }
        if (value === '') {
          callback(new Error('请输入密码'))
        } else if (!(/^[a-zA-Z]\w{5,15}$/).test(value)) {
          callback(new Error('密码必须以字母开头,只能包含字符、数字和下划线'))
        } else {
          if (this.signupForm.passwordAgain !== '') {
            this.$refs.signupForm.validateField('passwordAgain')
          }
          callback()
        }
      }
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'))
        } else if (value !== this.signupForm.password) {
          callback(new Error('两次输入密码不一致!'))
        } else {
          callback()
        }
      }
      var validatePhone = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入手机号！'))
        } else if (!(/^[1][3,4,5,6,7,8,9][0-9]{9}$/.test(value))) {
          callback(new Error('请输入合法的手机号'))
        } else callback()
      }
      var checkCode = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入验证码！'))
        } else if (!((/^\d{5}$/).test(value))) {
          callback(new Error('验证码不合法'))
        } else callback()
      }
      var validateUserName = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入用户名'))
        } else if (!(/^[a-zA-Z][a-zA-Z0-9_-]{3,15}$/).test(value)) {
          callback(new Error('用户名必须以字母开始,可包含数字,长度在4-16位'))
        } else callback()
      }
      return {
        Verification: true,
        timer: 60,
        dialogFormVisible: false,
        dataForm: {
          userName: '',
          password: '',
          uuid: '',
          captcha: ''
        },
        signupForm: {
          userName: '',
          password: '',
          passwordAgain: '',
          phone: '',
          checkPwd: ''
        },
        dataRule: {
          userName: [
            { required: true, message: '帐号不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' }
          ],
          captcha: [
            { required: true, message: '验证码不能为空', trigger: 'blur' }
          ]
        },
        signupRule: {
          userName: [
            { required: true, validator: validateUserName, trigger: 'blur' }
          ],
          password: [
            { required: true, validator: validatePass, trigger: 'blur' }
          ],
          passwordAgain: [
          { required: true, validator: validatePass2, trigger: 'blur' }
          ],
          phone: [
          { required: true, validator: validatePhone, trigger: 'blur' }
          ],
          checkPwd: [
          { validator: checkCode, required: true, trigger: 'blur' }
          ]
        },
        captchaPath: ''
  
      }
    },
    created () {
      this.getCaptcha()
    },
    methods: {
  
      handleClick () {
        let phone = this.signupForm.phone
        if ((/^[1][3,4,5,6,7,8,9][0-9]{9}$/.test(phone))) {
          this.Verification = false      // 点击button改变v-show的状态
          let auth_timer = setInterval(() => {  // 定时器设置每秒递减
            this.timer--        // 递减时间
            if (this.timer <= 0) {
              this.Verification = true    // 60s时间结束还原v-show状态并清除定时器
              clearInterval(auth_timer)
              this.timer = 60
            }
          }, 1000)
          this.$http({
            url: this.$http.adornUrl('/sys/getSignUpPhoneCheck?phone=' + phone),
            method: 'post'
  
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.$message.success('发送短信验证码成功，请查收')
            } else this.$message.error(data.msg)
          })
        } else if (phone === '') return this.$message.error('请输入手机号')
        else return this.$message.error('手机号不合法')
      },
      signFormSubmit () {
        // console.log('进入校验环节')
        this.$refs['signupForm'].validate((valid) => {
          if (valid) {
            console.log('检查成功！可以开始注册')
            this.$http({
              url: this.$http.adornUrl('/sys/user/signup'),
              method: 'post',
              data: this.signupForm
  
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message.success('注册成功！')
                this.dialogFormVisible = false
                this.$refs['signupForm'].resetFields()
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/sys/login'),
              method: 'post',
              data: this.$http.adornData({
                'username': this.dataForm.userName,
                'password': this.dataForm.password,
                'uuid': this.dataForm.uuid,
                'captcha': this.dataForm.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                window.sessionStorage.clear()
                window.sessionStorage.setItem('loginUserId', data.loginUserId)
                this.$router.replace({ name: 'home' })
              } else {
                this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      // 获取验证码
      getCaptcha () {
        this.dataForm.uuid = getUUID()
        // this.$http({
        //   url: this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm.uuid}`),
        //   method: 'get'
        // }).then(({ data }) => {
        //   this.captchaPath = data
        // })
        this.captchaPath = this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm.uuid}`)
      }
    }
  }
</script>

<style lang="scss" >
  .getCheckPwd{
    width: 150px;
     
    background-color: orange;color: white;
  }
  .dialogSubmit{
    color: white;
    background-color: rgb(9,113,241);
  }
  .dialogSubmit:hover{
    color: white;
    
    background-color:rgb(58,141,244);
  }
  .signupCard{
    display: none;
    width: 50%;
    background-color: #fff;
    border-radius: 5px;
  }
  .loginCard{
    display: block;
    width: 50%;
    background-color: #fff;
    border-radius: 5px;
  }
  .signupButton{
      background-color: orange;
      color:white ;
      width: 150px;
      height: 35px;
  }
  .loginButton{
      
      background-color: rgb(9,113,241);
      color: #fff;
      width: 150px;
      height: 35px;
    }
    .loginButton:hover{
      background-color: rgb(58,141,244);
    }
  .logohead:hover{
    cursor: pointer;
  }
  .createProject{
    width: 150px;
    margin-top: 15px;
    height: 30px;
    line-height: 30px;
    background-color: rgb(9,113,241);
    
  }
  .createProject:hover{
    background-color: rgb(58,141,244);
    cursor: pointer;
  }
    .h3list{
      color: rgb(153,153,153);
    }
    .el-header {
    background-color: rgb(21,21,21);
    color: #fff;
    text-align: center;
    line-height: 60px;
    height: 50pt;
    border-bottom: 1px solid gray;
  }
  .el-main {
    background-color: rgb(21,21,21);
    color: #fff;
    text-align: center;
    height: 645px;
  }
  .site-wrapper.site-page--login {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(38, 50, 56, .6);
    overflow: hidden;
    &:before {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      content: "";
      background-image: url(~@/assets/img/login_bg.jpg);
      background-size: cover;
    }
    .site-content__wrapper {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      padding: 0;
      margin: 0;
      overflow-x: hidden;
      overflow-y: auto;
      background-color: transparent;
    }
    .site-content {
      min-height: 100%;
      padding: 30px 500px 30px 30px;
    }
    .brand-info {
      margin-left: 40%;
      margin-top: 25%;
      color: #fff;
    }
    .brand-info__text {
      margin:  0 0 22px 0;
      font-size: 48px;
      font-weight: 400;
      text-transform : uppercase;
    }
    .brand-info__intro {
      margin: 10px 0;
      font-size: 16px;
      line-height: 1.58;
      opacity: .6;
    }
    .login-main {
      position: absolute;
      top: 0;
      right: 0;
      padding: 150px 60px 180px;
      width: 470px;
      min-height: 100%;
      background-color: #fff;
    }
    .login-title {
      font-size: 16px;
    }
    .login-captcha {
      overflow: hidden;
      > img {
        width: 100%;
        cursor: pointer;
      }
    }
    .login-btn-submit {
      width: 100%;
      margin-top: 38px;
      
    }
   
  } 
  
</style>
