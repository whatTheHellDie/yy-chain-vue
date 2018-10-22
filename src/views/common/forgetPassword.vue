<template>
  <div>
    <my-header></my-header>
    <div class="login-wrap-box">
      <div class="wrap clearfix">
        <div class="login-main login-main2">
          <div class="login-title">忘记密码</div>
          <img :src="stepImg" alt="" class="block-progress" />
          <el-form v-if="step==1" :model="dataForm" ref="dataForm" @keyup.enter.native="dataFormSubmit(2)" status-icon>
            <el-form-item prop="userName">
              <label class="label" for="userNumber">会员编号 ：</label>
              <el-input v-model="dataForm.userNumber"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="phone">手机号码 ：</label>
              <el-input v-model="dataForm.phone"></el-input>
            </el-form-item>
           <el-form-item prop="captcha">
              <label class="label" for="captcha">短信验证 ：</label>
              <el-input v-model="dataForm.captcha" placeholder="请输入短信验证码"></el-input>
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
              <el-button class="login-btn-submit mb124" type="primary" @click="dataFormSubmit(2)">立即验证</el-button>
            </el-form-item>
          </el-form>
          <el-form v-else-if="step==2" :model="dataForm1" ref="dataForm" @keyup.enter.native="dataFormSubmit(2)" status-icon>
            <el-form-item class="fixed-forget" prop="newPassword">
              <label class="label" for="newPassword">设置新登录密码 ：</label>
              <el-input v-model="dataForm1.newPassword" type="password"></el-input>
            </el-form-item>
            <el-form-item class="fixed-forget" prop="repeatPassword">
              <label class="label" for="repeatPassword">确认新登录密码 ：</label>
              <el-input v-model="dataForm1.repeatPassword" type="password"></el-input>
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
              <el-button class="login-btn-submit mb124" type="primary" @click="dataFormSubmit(3)">提交</el-button>
            </el-form-item>
          </el-form>
          <div class="login-main2 pb124" v-else>
            <img src="/static/img/password-success.png" alt="" class="step3" />
            <el-button class="login-btn-submit mb124 fan" type="primary" @click="$router.push({name:'login'})">返回登录</el-button>
          </div>
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
        step: 1,
        computeTime:"获取验证码",
        dataForm: {
          userNumber: '',
          phone: '',
          captcha: ''
        },
        dataForm1: {
          newPassword: '',
          repeatPassword: ''
        },
        registerForm: {
          userName: 'FS00000001',
          password: '',
          uuid: '',
          captcha: ''
        },
        registerRule: {
          userName: [{
            required: true,
            message: '账号不能为空',
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
        rememberPass: false,
        //      captchaPath: ''
      }
    },
    computed: {
      stepImg () {
        return '/static/img/progress'+this.step+'.png'
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    created() {
      //    this.getCaptcha()
    },
    methods: {
      // 切换注册登录
      switchForm(status) {
        this.checkStatus = status;
        console.log(this.checkStatus)
      },
      // 提交表单
      dataFormSubmit () {
        if (this.step === 1) {
          this.valid()
        } else if (this.step === 2) {
          this.reset()
        }
      },
      // 获取验证码
      getCaptcha () {
        if(this.computeTime!="获取验证码"){
          //处于倒计时
          return false;
        }
        let phone = this.dataForm.phone
        if (phone == null || phone === '') {
          this.$message.error('请先填写手机号')
          return
        }
        this.$http({
          url: this.$http.adornUrl('/sms/sendMSM'),
          method: 'get',
          params: this.$http.adornParams({
            'phoneNumber': this.dataForm.phone,
            'countryCode': '86'
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.$message.success('验证码已发送');
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
      },
      valid (step) {
        let userNumber = this.dataForm.userNumber
        if (userNumber == null || userNumber === '') {
          this.$message.error('请先填写会员编号')
          return
        }
        let phone = this.dataForm.phone
        if (phone == null || phone === '') {
          this.$message.error('请先填写手机号')
          return
        }
        let captcha = this.dataForm.captcha
        if (captcha == null || captcha === '') {
          this.$message.error('请先填写验证码')
          return
        }
        this.$http({
          url: this.$http.adornUrl('/user/reset/pwd/valid'),
          method: 'post',
          data: this.$http.adornData({
            'userNumber': this.dataForm.userNumber,
            'phone': this.dataForm.phone,
            'captcha': this.dataForm.captcha
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            // this.$message.success('验证通过')
            this.step = 2
            this.stepImg = '/static/img/progress' + 2 + '.png'
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      reset (step) {
        let newPassword = this.dataForm1.newPassword
        if (newPassword == null || newPassword === '') {
          this.$message.error('请先填写新密码')
          return
        }
        let repeatPassword = this.dataForm1.repeatPassword
        if (repeatPassword == null || repeatPassword === '') {
          this.$message.error('请再次填写新密码')
          return
        }
        if (newPassword !== repeatPassword) {
          this.$message.error('两次输入的密码不一样')
          return
        }
        this.$http({
          url: this.$http.adornUrl('/user/set/longPwd'),
          method: 'post',
          params: this.$http.adornParams({
            'userNumber': this.dataForm.userNumber,
            'newPwd': this.dataForm1.repeatPassword
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.step = 3
            this.stepImg = '/static/img/progress' + 3 + '.png'
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
