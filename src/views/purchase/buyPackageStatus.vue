<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card mb50">
      <h1>详细介绍</h1>
      <div class="box-body">
        <div class="xiangxi-intro">
          <ul>
          	<li>1、购买多少价值的股份，立即配送等额价值的配送易用积分。</li>
          	<li>2、赠送等价值的激励易用积分。</li>
          </ul>
        </div>
      </div>
    </div>
    <my-footer></my-footer>
  </div>
</template>
<style lang="scss" scoped>
  .order-gradient {
    width: 1px;
    height: 302px;
    display: inline-block;
    vertical-align: top;
    background-image: url(/static/img/gradient.png);
  }

</style>

<script>
  //import { getUUID } from '@/utils'
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data () {
      return {
        sharesData: {
          cnyPriceMyria: '',
          payUsdtAmount: '',
          roundAmountCurrent: '',
          roundCurrent: '',
          roundSendYyiBalance: '',
          stageCurrent: '',
          startYyiQuantity: '',
          usdtPriceMyria: '',
          userUsdt: ''
        },
        yyiQuantity: 0
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    methods: {
      handleChange (value) {
        console.log(value)
      },
      getSharesViweData () {
        this.$http({
          url: this.$http.adornUrl('/shares/getSharesViweData'),
          method: 'get'
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.sharesData.cnyPriceMyria = data.data.cnyPriceMyria
            this.sharesData.payUsdtAmount = data.data.payUsdtAmount
            this.sharesData.roundAmountCurrent = data.data.roundAmountCurrent
            this.sharesData.roundCurrent = data.data.roundCurrent
            this.sharesData.roundSendYyiBalance = data.data.roundSendYyiBalance
            this.sharesData.stageCurrent = data.data.stageCurrent
            this.sharesData.startYyiQuantity = data.data.startYyiQuantity
            this.sharesData.usdtPriceMyria = data.data.usdtPriceMyria
            this.sharesData.userUsdt = data.data.userUsdt
            this.yyiQuantity = data.data.startYyiQuantity
            console.log(this.sharesData.stageCurrent);
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      subOrAdd (flag) {
        if (flag === 0) {//减
          this.yyiQuantity = Number(this.yyiQuantity) - 1
        }

        if (flag === 1) {//加
          this.yyiQuantity = Number(this.yyiQuantity) + 1
        }

        if (this.yyiQuantity <= this.sharesData.startYyiQuantity) {
          this.yyiQuantity = Number(this.sharesData.startYyiQuantity)
        }
        var that = this
        this.$http({
          url: this.$http.adornUrl('/shares/clickAddOrSub'),
          method: 'post',
          params: this.$http.adornParams({
            'currentStage': that.sharesData.stageCurrent,
            'currentRound': that.sharesData.roundCurrent,
            'yyiQuantity': that.yyiQuantity
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            this.sharesData.payUsdtAmount = data.data
          } else {
            this.$message.error(data.msg)
          }
        })
      }

    },
    created: function () {
      this.getSharesViweData()
    }
  }
</script>
