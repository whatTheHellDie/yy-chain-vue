<template>
  <div>
    <my-header></my-header>
    <div class="login-wrap-box">
      <div class="wrap clearfix">
        <div class="login-main login-main2">
          <div class="login-title">忘记密码</div>
          <img :src="stepImg" alt="" class="block-progress" />
          <el-form v-if="step==1" :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit(2)" status-icon>
            <el-form-item prop="userName">
              <label class="label" for="userName">会员编号 ：</label>
              <el-input v-model="dataForm.userName"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="password">手机号码 ：</label>
              <el-input v-model="dataForm.password" type="password"></el-input>
            </el-form-item>
           <el-form-item prop="captcha">
              <label class="label" for="captcha">短信验证 ：</label>
              <el-input v-model="dataForm1.captcha" placeholder="请输入短信验证码"></el-input>
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
              <el-button class="login-btn-submit mb124" type="primary" @click="dataFormSubmit(2)">立即验证</el-button>
            </el-form-item>
          </el-form>
          <el-form v-else-if="step==2" :model="dataForm1" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit(2)" status-icon>
            <el-form-item prop="password">
              <label class="label" for="password">设置新登录密码 ：</label>
              <el-input v-model="dataForm.password" type="password"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <label class="label" for="password">确认新登录密码 ：</label>
              <el-input v-model="dataForm.password" type="password"></el-input>
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
              <el-button class="login-btn-submit mb124" type="primary" @click="dataFormSubmit(3)">立即验证</el-button>
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
    data() {
      return {
        step:1,
        
        dataForm: {
          userName: '',
          password: '',
          uuid: '',
          captcha: ''
        },
        dataForm1: {
          userName: 'FS00000001',
          password: '',
          uuid: '',
          captcha: ''
        },
        dataRule: {
          userName: [{
            required: true,
            message: '帐号不能为空',
            trigger: 'blur'
          }],
          password: [{
            required: true,
            message: '密码不能为空',
            trigger: 'blur'
          }],

          //        captcha: [
          //          { required: true, message: '验证码不能为空', trigger: 'blur' }
          //        ]
        },
        rememberPass: false,
        //      captchaPath: ''
      }
    },
    computed:{
      stepImg(){
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
      //切换注册登录
      switchForm(status) {
        this.checkStatus = status;
        console.log(this.checkStatus)
      },
      // 提交表单
      dataFormSubmit(step) {
        console.log(step)
        this.step=step;
        this.stepImg='/static/img/progress'+step+'.png'
//      this.$refs['dataForm'].validate((valid) => {
//        if(valid) {
//          this.$http({
//            url: this.$http.adornUrl('/sys/login'),
//            method: 'post',
//            data: this.$http.adornData({
//              'username': this.dataForm.userName,
//              'password': this.dataForm.password,
//              //              'uuid': this.dataForm.uuid,
//              //              'captcha': this.dataForm.captcha
//            })
//          }).then(({
//            data
//          }) => {
//            if(data && data.code === 0) {
//              this.$cookie.set('token', data.token)
//              this.$router.replace({
//                name: 'home'
//              })
//            } else {
//              //              this.getCaptcha()
//              this.$message.error(data.msg)
//            }
//          })
//        }
//      })
      },
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