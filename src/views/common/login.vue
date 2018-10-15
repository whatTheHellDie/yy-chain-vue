<template>
  <div>
    <my-header></my-header>
    <div class="login-wrap-box">
      <div class="wrap clearfix">
        <div class="login-main pull-right">
          <div class="login-title">
            <span class="tab cursor" :class="{active:checkStatus}" @click="$router.push({ name: 'login' })">登录</span><span class="tab cursor" :class="{active:!checkStatus}" @click="$router.push({ name: 'register' })">注册</span>
          </div>
          <el-form v-if="this.checkStatus" :model="dataForm" :rules="registerRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" status-icon>
            <el-form-item prop="userName">
              <label class="label" for="userName">会员编号 ：</label>
              <el-input v-model="dataForm.userName"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="password">登录密码 ：</label>
              <el-input v-model="dataForm.password" type="password"></el-input>
            </el-form-item>
            <el-form-item prop="captcha">
              <label class="label" for="captcha">图形验证 ：</label>
              <el-input v-model="dataForm.captcha"></el-input>
              <div class="captcha">FLJE </div>
            </el-form-item>
            <!--<el-form-item prop="captcha">
              <el-row :gutter="20">
                <el-col :span="14">
                  <el-input v-model="dataForm.captcha" placeholder="验证码">
                  </el-input>
                </el-col>
                <el-col :span="10" class="login-captcha">
                  <img :src="captchaPath" @click="getCaptcha()" alt="">
                </el-col>
              </el-row>
            </el-form-item>-->
            <div class="remember-pass clearfix">
              <el-checkbox v-model="rememberPass">记住账号</el-checkbox>
              <span class="cursor default pull-right" @click="$router.push({name:'forgetPassword'})">忘记密码 ？</span>

            </div>
            <el-form-item>
              <el-button class="login-btn-submit" type="primary" @click="dataFormSubmit()">登录</el-button>
              <div class="no-account">没有账号？去<span @click="$router.push({ name: 'register' })" class="cursor default">注册</span></div>
            </el-form-item>
          </el-form>

          <el-form v-else :model="registerForm" :rules="registerRule" ref="registerForm" @keyup.enter.native="dataFormSubmit()" status-icon>
            <el-form-item prop="userNumber">
              <label class="label" for="userNumber">会员编号 ：</label>
              <el-input v-model="registerForm.userNumber" readonly></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="password">登录密码 ：</label>
              <el-input v-model="registerForm.password" placeholder="请输入8-16位数字、字符组合密码" type="password"></el-input>
            </el-form-item>
            <el-form-item prop="rePassword">
              <label class="label" for="rePassword">确认密码 ：</label>
              <el-input v-model="registerForm.rePassword" placeholder="请输入8-16位数字、字符组合密码" type="password"></el-input>
            </el-form-item>
            <el-form-item prop="referrerNumber">
              <label class="label" for="referrerNumber">推荐会员 ：</label>
              <el-input v-model="registerForm.referrerNumber" placeholder="请输入推荐人帐号"></el-input>
            </el-form-item>
            <el-form-item prop="phone">
              <label class="label" for="phone">手机号码 ：</label>
              <el-input v-model="registerForm.phone" placeholder="请输入手机号码"></el-input>
            </el-form-item>
            <el-form-item prop="captcha">
              <label class="label" for="captcha">短信验证 ：</label>
              <el-input v-model="registerForm.captcha" placeholder="请输入短信验证码"></el-input>
              <el-button class="captcha captcha1">获取验证码</el-button>
            </el-form-item>
            <!--<el-form-item prop="captcha">
              <el-row :gutter="20">
                <el-col :span="14">
                  <el-input v-model="dataForm.captcha" placeholder="验证码">
                  </el-input>
                </el-col>
                <el-col :span="10" class="login-captcha">
                  <img :src="captchaPath" @click="getCaptcha()" alt="">
                </el-col>
              </el-row>
            </el-form-item>-->
            <el-form-item>
              <el-button class="login-btn-submit" type="primary" @click="register()">注册</el-button>
              <div class="no-account">已有账号，去<span @click="$router.push({ name: 'login' })" class="cursor default">登录</span></div>
            </el-form-item>
          </el-form>
        </div>

      </div>
    </div>
    <my-footer></my-footer>
  </div>
</template>

<script>
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data () {
      return {

        dataForm: {
          userName: '',
          password: '',
          uuid: '',
          captcha: ''
        },
        registerForm: {
          userNumber: '',
          password: '',
          rePassword: '',
          referrerNumber: '',
          phone: '',
          captcha: ''
        },
        registerRule: {
          userNumber: [{
            required: true,
            message: '帐号不能为空',
            trigger: 'blur'
          }],
          password: [{
            required: true,
            message: '密码不能为空',
            trigger: 'blur'
          }]

          //        captcha: [
          //          { required: true, message: '验证码不能为空', trigger: 'blur' }
          //        ]
        },
        rememberPass: false
        //      captchaPath: ''
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    computed:{
      checkStatus(){
        if (this.$route.name == 'register') {
          return false
        } else {
          return true
        }
      }
    },
    created() {
      //    this.getCaptcha()
      this.createUserNumber();
    },
    methods: {
      // 切换注册登录
      createUserNumber () {
        if (!this.checkStatus) {
          this.$http({
            url: this.$http.adornUrl('/id/create'),
            method: 'get'
          }).then(({data}) => {
            this.dataListLoading = false
            if (data && data.code === '0000') {
              this.registerForm.userNumber = data.data
            } else {
              this.$message.error(data.msg)
            }
          }).catch(({error}) => {
            this.dataListLoading = false
            this.$message.error(error)
          })
        }
      },
      // 提交表单
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/user/login'),
              method: 'post',
              data: this.$http.adornData({
                'userNumber': this.dataForm.userName,
                'loginPassword': this.dataForm.password
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({
                  name: 'index'
                })
              } else {
                //              this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      register () {
        this.$refs['registerForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/user/register'),
              method: 'post',
              data: this.$http.adornData({
                'userNumber': this.registerForm.userNumber,
                'referrerNumber': this.registerForm.referrerNumber,
                'loginPassword': this.registerForm.password,
                'phone': this.registerForm.phone,
                'captcha': this.registerForm.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({name: 'index'})
              } else {
                //              this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
      // 获取验证码
      //    getCaptcha () {
      //      this.dataForm.uuid = getUUID()
      //      this.captchaPath = this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm.uuid}`)
      //    }
    }
  }
</script>

<style lang="scss">

</style>
