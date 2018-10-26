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
                <dd class="clearfix" v-if="noAuthentic">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      实名认证<span class="pin active">{{authenticText}}</span>
                  <div class="gu-btn" @click="$router.push({name:'realNameAuthentication'})">认证</div>
                </dd>
                <dd class="clearfix" v-if="isAuthentic">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      实名认证<span class="pin">{{authenticText}}</span>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      登录密码<span class="pin">已设置</span>
                  <div class="gu-btn" @click="$router.push({name:'resetLoginPassword'})">重置</div>
                </dd>
                <dd class="clearfix" v-if="noSetPass">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      支付密码<span class="pin active">未设置</span>
                  <div class="gu-btn" @click="$router.push({name:'setPaymentPassword'})">设置</div>
                </dd>
                <dd class="clearfix" v-if="isSetPass">
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
        chooseContent: [],

        isAuthentic: true,  //是否已认证
        noAuthentic: false, //是否未认证
        isSetPass: false,    //是否已设置密码
        noSetPass: true,   //是否未设置密码
        authenticText: '未认证',  //'未认证'或'认证失败'文本显示
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
            if(!data.data) {
              return;
            }
            this.personInfo.userNumber = data.data.userNumber
            this.personInfo.userType = data.data.userType
            this.personInfo.referrerNumber = data.data.referrerNumber
            this.personInfo.phone = data.data.phone
            this.personInfo.auth = data.data.auth
            this.personInfo.payPwd = data.data.payPassword
            this.personInfo.phone = data.data.phone
          } else {
            // this.$message.error(data.msg)
            this.$message.error('刷新失败')
          }
        }).catch(({error}) => {
          this.dataListLoading = false
          this.$message.error(error)
        })
      },
      //显示数据时的验证
      showValidate(){
        if(!this.personInfo){
          return;
        }
        //未认证
        if(this.personInfo.auth == '1'){
          this.isAuthentic = false;
          this.noAuthentic = true;
          this.authenticText = '未认证';
        }
        //认证失败
        if(this.personInfo.auth == '4'){
          this.isAuthentic = false;
          this.noAuthentic = true;
          this.authenticText = '认证失败';
        }
        //认证中
        if(this.personInfo.auth == '2'){
          this.isAuthentic = true;
          this.noAuthentic = false;
          this.authenticText = '认证中...';
        }
        //已认证
        if(this.personInfo.auth == '3'){
          this.isAuthentic = true;
          this.noAuthentic = false;
          this.authenticText = '已认证';
        }
        if(this.personInfo.payPwd){
          this.noSetPass = false;
          this.isSetPass = true;
        }else{
          this.noSetPass = true;
          this.isSetPass = false;
        }
      }
    },
    mounted: function () {
      this.getPersonInfo();
    },
    updated: function () {
      this.showValidate();
    }
  }
</script>
