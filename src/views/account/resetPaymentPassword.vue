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
                <span role="link" class="el-breadcrumb__inner" style="font-weight: 800;">重置支付密码</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span></div>
          </div>
          <div class="box-body min425">
            <div class="charge-coin set-password forget-password">
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">原支付密码</label>
                <input type="password" class="form-control input" placeholder="请输入原密码" v-model="oldPwd"><span class="lh35 ml10 default" @click="$router.push({name:'forgetPaymentPassword'})">忘记原密码？</span>
                <span class="tip max-220"></span>
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">新支付密码</label>
                <input type="password" class="form-control input" placeholder="请输入新密码" v-model="newPwd">
                <span class="tip"></span>
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">确认新支付密码</label>
                <input type="password" class="form-control input" placeholder="6位数字" v-model="repeatPwd">
                 <span class="tip"></span>
              </div>
              <span class="tip"></span>
              <div class="gu-btn" @click="resetPayPwd()">提交</div>
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
    components: { MainBody },
    data() {
      return {
        oldPwd: '',
        newPwd: '',
        repeatPwd: ''
      }
    },
    methods: {
      //重置支付密码
      resetPayPwd() {
        this.$http({
          url: this.$http.adornUrl('/user/reset/pwd'),
          method: 'post',
          params: this.$http.adornParams({
            'oldPwd': this.oldPwd,
            'newPwd': this.newPwd,
            'type': 2 //支付密码
          })
        }).then(({data}) => {
          if (data && data.code === '0000') { 
            // window.location.href='/personalInformation';
            this.$message.success('修改密码成功')  
            this.$router.replace({
              name: 'personalInformation'
            })
          } else {
            this.$message.error(data.msg)
          }
        })
      },
    },
    mounted() {
    }
  }
</script>
