<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card order-page">
      <h1>订单详情</h1>
      <div class="box-body no-padding">
        <div class="buy-detail">
          <dl>
          	<dd><label>交易数额（USDT）：</label><span class="span">{{payUsdtAmount}}枚</span></dd>
          	<dd><label>配送易用积分：</label><span class="span">{{yyiQuantity}}万个</span></dd>
          </dl>
        </div>
        <div class="buy-detail2">
        <h1>支付订单</h1>
          <dl>
            <dd><label>需支付USDT(枚)：</label><span class="span">{{payUsdtAmount}}</span></dd>
            <form class="m-form" method="post">
            	<input type="password" v-model="payPassword" class="input" />
              <span class="wang" @click="resetPaymentPassword()">忘记密码？</span>
            	<div class="shu-tip">请输入6位数字支付密码</div>
            	<div class="gu-btn" @click="pay()">确认支付</div><span class="please">请在 <span class="time">{{totalTime}}S</span> 内完成支付！</span>
            </form>
          </dl>
        </div>
      </div>
    </div>
    <my-footer></my-footer>
  </div>
</template>
<style lang="scss" scoped>

</style>

<script>
  // import { getUUID } from '@/utils'
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data () {
      return {
        totalTime: sessionStorage.getItem('totalTime'),
        stageCurrent: 0,
        roundCurrent: 0,
        yyiQuantity: 0,
        payUsdtAmount: 0,
        payPassword: ''
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    methods: {
      countdown () {
        // sessionStorage.setItem('totalTime', this.totalTime)

        var computeTime = sessionStorage.getItem('totalTime')
        var time = setInterval(() => {
          if (computeTime === 0) {
            // 关闭定时器
            clearInterval(time)

            // 清空时间
            sessionStorage.setItem('totalTime', 0)
            // 跳转到首页
            this.$router.push({
              name: 'buyPackage'
            })
            return
          }
          this.totalTime = computeTime
          computeTime--
          sessionStorage.setItem('totalTime', computeTime)
        }, 1000)
      },
      pay () {
        var stageCurrent = Number(this.stageCurrent)
        if (stageCurrent === 1) { // 第一阶段

          this.$http({
            url: this.$http.adornUrl('/shares/firstStagePay'),
            method: 'post',
            data: this.$http.adornData({
              'currentStage': this.stageCurrent,
              'currentRound': this.roundCurrent,
              'sendYyiQuantity': this.yyiQuantity,
              'payUsdtAmount': this.payUsdtAmount,
              'payPassword': this.payPassword
            })
          }).then(({data}) => {
            if (data && data.code === '0000') {
              this.$message.success(data.msg)
              // 跳到支付成功页面
              this.$router.push({
                name: 'buyPackageStatus',
                query: {
                  status: 0
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })

        } else if (stageCurrent === 2) { // 第二阶段

          this.$http({
            url: this.$http.adornUrl('/shares/secondStagePay'),
            method: 'post',
            data: this.$http.adornData({
              'currentStage': this.stageCurrent,
              'currentRound': this.roundCurrent,
              'sendYyiQuantity': this.yyiQuantity,
              'payUsdtAmount': this.payUsdtAmount,
              'payPassword': this.payPassword
            })
          }).then(({data}) => {
            if (data && data.code === '0000') {
              this.$message.success(data.msg)
              // 跳到支付成功页面
              this.$router.push({
                name: 'buyPackageStatus',
                query: {
                  status: 0
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })

        }

      },
      resetPaymentPassword () {
        // 跳到重置支付密码页面
        this.$router.push({
          name: 'resetPaymentPassword'
        })
      }
    },
    created: function () {
      this.stageCurrent = this.$route.query.stageCurrent
      this.roundCurrent = this.$route.query.roundCurrent
      this.yyiQuantity = this.$route.query.yyiQuantity
      this.payUsdtAmount = this.$route.query.payUsdtAmount
      // 倒计时
      this.countdown()
    }
  }
</script>
