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
              <span class="wang">忘记密码？</span>
            	<div class="shu-tip">请输入6位数字支付密码</div>
            	<div class="gu-btn" @click="pay()">确认支付</div><!--<span class="please">请在 <span class="time">120S</span> 内完成支付！</span>-->
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
  //import { getUUID } from '@/utils'
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data () {
      return {
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
      handleChange(value) {
        console.log(value);
      },
      pay () {
        this.$http({
          url: this.$http.adornUrl('/shares/pay'),
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
            //跳到支付成功页面
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
    created: function () {
      this.stageCurrent = this.$route.query.stageCurrent
      this.roundCurrent = this.$route.query.roundCurrent
      this.yyiQuantity = this.$route.query.yyiQuantity
      this.payUsdtAmount = this.$route.query.payUsdtAmount
    }
  }
</script>
