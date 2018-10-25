<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="top-nav">
            <div aria-label="Breadcrumb" role="navigation" class="el-breadcrumb">
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">账户中心</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span>
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">个人信息</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span> <span class="el-breadcrumb__item bold" aria-current="page">
                <span role="link" class="el-breadcrumb__inner" style="font-weight: 800;">忘记支付密码</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span></div>
          </div>
          <div class="box-body min425">
            <div class="charge-coin set-password forget-password">
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">手机号码</label>
                <input type="text" class="form-control input" placeholder="请输入您绑定的手机号码" v-model="phone">
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">短信验证码</label>
                <input type="text" class="form-control input" placeholder="请输入短信验证码" v-model="captcha">
                <!-- <div class="btn-sub">获取验证码</div> -->
                <div class="btn-sub" :class="{active:computeTime!='获取验证码'}" @click="getCaptcha()">{{computeTime}}</div>
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">新支付密码</label>
                <input type="password" class="form-control input" placeholder="6位数字" v-model="newPwd">
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">确认支付密码</label>
                <input type="password" class="form-control input" placeholder="6位数字" v-model="repeatPwd">
              </div>
              <div class="gu-btn" @click="update()">提交</div>
            </div>
          </div>

        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>

</style>

<script>
  //import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
      return { 
        computeTime: '获取验证码',
        phone: '',
        captcha: '',
        newPwd: '',
        repeatPwd: ''
      }
    },
    components: {
      MainBody
    },
    methods: {
      update(){
        const regPhone = /^1[3|4|5|7|8|6|9][0-9]\d{8}$/
        if (!regPhone.test(this.phone)){
          this.$message.error('请输入正确的手机号')
          return;
        }

        if (this.newPwd != this.repeatPwd){
          this.$message.error('两次密码输入不一致')
          return;
        }

        const regPsw = /^[0-9]\d{5}$/ 
        if (!regPsw.test(this.newPwd) || !regPsw.test(this.repeatPwd)){
          this.$message.error('请输入6位数字密码')
          return;
        }

        if (!this.captcha){
          this.$message.error('请输入验证码')
          return;
        }
        
        this.$http({
          url: this.$http.adornUrl('/user/reset/pwd'),
          method: 'post',
          params: this.$http.adornParams({
            'oldPwd': '',
            'newPwd': this.newPwd,
            'type' : 2,
            'phone': this.phone,
            'captcha': this.captcha
          }) 
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.$message.success('重置密码成功')
            this.$router.push({name: 'personalInformation'})
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      // 获取验证码
      getCaptcha () {
        if(this.computeTime!="获取验证码"){
          //处于倒计时
          return false;
        }
        let phone = this.phone
        if (phone == null || phone === '') {
          this.$message.error('请先填写手机号')
          return
        }
        this.$http({
          url: this.$http.adornUrl('/sms/sendMSM'),
          method: 'get',
          params: this.$http.adornParams({
            'phoneNumber': this.phone,
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
    },
    mounted() { 
    }
  }
</script>