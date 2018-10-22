<template>
  <div>
    <my-header></my-header>
    <div class="login-wrap-box">
      <div class="wrap clearfix">
        <div class="login-main pull-right">
          <div class="login-title">
            <span class="tab cursor" :class="{active:checkStatus}" @click="$router.push({ name: 'login' })">登录</span><span class="tab cursor" :class="{active:!checkStatus}" @click="$router.push({ name: 'register' })">注册</span>
          </div>
          <el-form v-if="this.checkStatus" :model="loginForm" :rules="loginFormRule" ref="loginForm" @keyup.enter.native="login()" status-icon>
            <el-form-item prop="userNumber">
              <label class="label" for="userNumber">会员编号 ：</label>
              <el-input v-model="loginForm.userNumber"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="password">登录密码 ：</label>
              <el-input v-model="loginForm.password" type="password"></el-input>
            </el-form-item>
            <!--<el-form-item prop="captcha">-->
              <!--<label class="label" for="captcha">图形验证 ：</label>-->
              <!--<el-input v-model="loginForm.captcha"></el-input>-->
              <!--<div class="captcha">FLJE </div>-->
            <!--</el-form-item>-->
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
              <!--<el-checkbox v-model="rememberPass">记住账号</el-checkbox>-->
              <span class="cursor default pull-right" @click="$router.push({name:'forgetPassword'})">忘记密码 ？</span>

            </div>
            <el-form-item>
              <el-button class="login-btn-submit" type="primary" @click="login()">登录</el-button>
              <div class="no-account">没有账号？去<span @click="$router.push({ name: 'register' })" class="cursor default">注册</span></div>
            </el-form-item>
          </el-form>

          <el-form v-show="!this.checkStatus" :model="registerForm" :rules="registerRule" ref="registerForm" @keyup.enter.native="register()" status-icon>
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
              <el-input v-model="registerForm.referrerNumber" placeholder="请输入推荐人账号"></el-input>
            </el-form-item>
            <el-form-item prop="phone">
              <label class="label" for="phone">手机号码 ：</label>
              <el-input v-model="registerForm.phone" placeholder="请输入手机号码"></el-input>
            </el-form-item>
            <el-form-item prop="captcha">
              <label class="label" for="captcha">短信验证 ：</label>
              <el-input v-model="registerForm.captcha" placeholder="请输入短信验证码"></el-input>
              <el-button class="captcha captcha1" :class="{active:computeTime!='获取验证码'}" @click="getCaptcha()">{{computeTime}}</el-button>
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
      var checkPhone = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('手机号不能为空'));
        } else {
          const reg = /^1[3|4|5|7|8|6|9][0-9]\d{8}$/
          console.log(reg.test(value));
          if (reg.test(value)) {
            callback();
          } else {
            return callback(new Error('请输入正确的手机号'));
          }
        }
      }; 
      var validateReffer = (rule, value, callback) => {
        
      }; 
      
      var checkPass = (rule, value, callback) => {
        const reg = /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{8,16}$/
          console.log(reg.test(value));
          if (reg.test(value)) {
            callback();
          } else {
            return callback(new Error('请输入8-16位数字、字符组合密码'));
          }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.registerForm.password) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        computeTime:'获取验证码',
        loginForm: {
          userNumber: 'YY',
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
        loginFormRule: {
          userNumber: [{
            required: true,
            message: '会员编号不能为空',
            trigger: 'blur'
          },
          {
           required: true,
           min:10,
           message:'会员编号长度应为10位',
           trigger: 'blur'}
          ],
          password: [{
            required: true,
            message: '密码不能为空',
            trigger: 'blur'
          }
          ],
//        captcha: [{
//          required: true,
//          message: '验证码不能为空',
//          trigger: 'blur'
//        }]
        },
        registerRule: {
          userNumber: [{
            required: true,
            message: '账号不能为空',
            trigger: 'blur'
          }],
          password: [{
            required: true,
            message: '密码不能为空',
            trigger: 'blur'
          },
          {
            required: true,
            min:8,
            max:16,
           message:'请输入8-16位，数字、字符组合密码',
           trigger: 'blur'
          },
          { required: true,validator: checkPass, trigger: 'blur' },
          ],
          rePassword: [{
            required: true,
            message: '确认密码不能为空',
            trigger: 'blur'
          },
          {
            required: true,
            min:8,
            max:16,
           message:'请输入8-16位，数字、字符组合密码',
           trigger: 'blur'
          },
          { validator: validatePass2, trigger: 'blur' },
          ],
          referrerNumber: [{
            required: true,
            message: '推荐人不能为空',
            trigger: 'blur'
          }],
          phone: [{
            // required: true,
            // message: '电话不能为空',
            validator: checkPhone,
            trigger: 'blur'
          }],
          captcha: [
            {  required: true,message: '验证码不能为空', trigger: 'blur' }
          ]
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
        this.createUserNumber();
    },
    watch: {
      '$route' (to, from) {
        // 对路由变化作出响应...
        this.createUserNumber();
      }
    },
    methods: {
      // 切换注册登录
      createUserNumber () {
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
      },
      // 登录
      login () {
        this.$refs['loginForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/user/login'),
              method: 'post',
              data: this.$http.adornData({
                'userNumber': this.loginForm.userNumber,
                'loginPassword': this.loginForm.password
              })
            }).then(({data}) => {
              if (data && data.code === '01201') {
                this.$cookie.set('token', data.data.token)
                this.$router.replace({
                  name: 'index'
                })
                window.sessionStorage.removeItem('userNumber')
                window.sessionStorage.removeItem('auth')
                window.sessionStorage.setItem('auth', data.data.auth )
                window.sessionStorage.setItem('userNumber', data.data.userNumber)
                this.$message.success('登录成功')
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      register () {
        this.$refs['registerForm'].validate((valid) => {
          if (valid) {
            
            if (this.registerForm.password != this.registerForm.rePassword){
              this.$message.error('两次输入的密码不一致');
              return;
            } 

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
              if (data && data.code === '01107') {
                // alert(1)
                this.$message.success(data.msg)
                // alert(2)
                this.$router.push({name: 'login'})
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      // 获取验证码
      getCaptcha () {
        if(this.computeTime!="获取验证码"){
          //处于倒计时
          return false;
        }
        let phone = this.registerForm.phone
        if (phone == null || phone === '') {
          this.$message.error('请先填写手机号')
          return
        }
        this.$http({
          url: this.$http.adornUrl('/sms/sendMSM'),
          method: 'get',
          params: this.$http.adornParams({
            'phoneNumber': this.registerForm.phone,
            'countryCode': '86'
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.$message.success('验证码发送成功')
            var second=60;
            var that=this;
            this.computeTime="剩余"+second--+"秒"
        var time=setInterval(() => {
          if(second==0){
            console.log(this.computeTime)
            that.computeTime="获取验证码"
            clearInterval(time)
            return false;
          }
             this.computeTime="剩余"+second+"秒"
             second--;
        },1000)
          } else {
            
            this.$message.error(data.msg)
          }
        })
      }
    }
  }
</script>

<style lang="scss">

</style>
