<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="index-i index-p">
              <img src="/static/img/profile.png" alt="" class="tou">
              <div class="title">
                <span class="hao">{{personInfo.userNumber}}</span>
                <div class="level">会员身份：{{personInfo.userType}}</div>
                <div class="level">推荐人：{{personInfo.referrerNumber}}</div>
              </div>
              <!-- <div class="change-title">[更换头像]</div> -->
            </div>
            <div class="account-top">
              <div class="title">账户信息</div>
              <dl>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      手机号码<span class="pin">{{personInfo.phone}}</span>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      实名认证<span class="pin active">未认证</span>
                  <div class="gu-btn" @click="$router.push({name:'realNameAuthentication'})">认证</div>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                  实名认证<span class="pin">已认证</span>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      登录密码<span class="pin">已设置</span>
                  <div class="gu-btn" @click="$router.push({name:'resetLoginPassword'})">重置</div>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      支付密码<span class="pin active">未设置</span>
                  <div class="gu-btn" @click="$router.push({name:'setPaymentPassword'})">设置</div>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                  支付密码<span class="pin">已设置</span>
                  <div class="gu-btn" @click="$router.push({name:'resetPaymentPassword'})">重置</div>
                </dd>
              </dl>
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
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        activeNumber: 0,
        personInfo: {
          userNumber: '',
          userType: '',
          referrerNumber: '',
          phone: '',
          auth: '',
          loginPwd: '',
          payPwd: ''
        },
        chooseContent: []
      }
    },
    components: {
      MainBody
    },
    methods: {
      getPersonInfo () {
        this.$http({
          url: this.$http.adornUrl('/user/query/one'),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.personInfo.userNumber = data.data.userNumber
            this.personInfo.userType = data.data.userType
            this.personInfo.referrerNumber = data.data.referrerNumber
            this.personInfo.phone = data.data.phone
            this.personInfo.auth = data.data.auth
            this.personInfo.phone = data.data.phone
            this.personInfo.phone = data.data.phone
          } else {
            // this.$message.error(data.msg)
            this.$message.error('提交失败')
          }
        }).catch(({error}) => {
          this.dataListLoading = false
          this.$message.error(error)
        })
      }
    },
    mounted: function () {
      this.getPersonInfo()
    }
  }
</script>
