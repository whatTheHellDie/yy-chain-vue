<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card order-page">
      <h1>入股</h1>
      <div class="box-body box-body0">
        <div class="left">
          <div class="qi">第 <span>{{sharesData.roundCurrent}}</span> / {{sharesData.roundAmountCurrent}} 期</div>
          <div class="intro">每期配送完毕，自动进入下一期，单价上涨0.1%。</div>
          <div class="number">
            <span class="span">{{sharesData.roundSendYyiBalance}}</span> 万个
            <div class="mt14">本期剩余配送易用积分</div>
          </div>
          <div class="time-price">
            <div class="title1">当前价格</div>
            <div class="content1"><span class="span1">CNY：<span class="default">{{sharesData.cnyPriceMyria}}</span>元/万个</span>
              <span class="span">USDT约：<span class="default">{{sharesData.usdtPriceMyria}}</span>枚/万个
              <div class="propp-tip">
                <img src="/static/img/tip.png" class="m" />
                <img src="/static/img/biaozhun.png" class="img" />
              </div>
              </span>
            </div>
          </div>
        </div>
        <div class="order-gradient"></div>
        <div class="right">
          <div>账户USDT余额：<span class="default">{{sharesData.userUsdt}}</span>
            <el-button type="primary" @click="$router.push({name: 'chargeCoin'})" class="charge-btn">充币</el-button>
          </div>
          <div class="default1 mt11">起配积分数额：{{sharesData.startYyiQuantity}}万个</div>
          <div class="add_one clearfix">
            <!--<div class="add reduce" @click="subOrAdd(0)">-</div><input type="text"  v-model="yyiQuantity"><div class="pull-left wan">万</div><div class="add" @click="subOrAdd(1)">+</div>-->
            <div class="add" @click="subOrAdd(0)">-</div><input type="text" readonly="readonly" v-model="yyiQuantity"><div class="pull-left wan">万</div><div class="add" @click="subOrAdd(1)">+</div>
          </div>
          <div style="padding-left: 92px;">约支付USDT(枚)：<span class="default">{{sharesData.payUsdtAmount}}</span></div>
          <div class="gu-btn cursor" @click="setShares()">立即入股</div>
        </div>
      </div>
    </div>
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
.wan{width: 27px;line-height: 50px;}
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
        //console.log(value)
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
      },
      setShares () {
        // 初始化定时支付时间
        sessionStorage.setItem('totalTime', 300)
        // 跳转到支付页面
        this.$router.push({
          name: 'buyPackage-Pay',
          query: {
            stageCurrent: this.sharesData.stageCurrent,
            roundCurrent: this.sharesData.roundCurrent,
            yyiQuantity: this.yyiQuantity,
            payUsdtAmount: this.sharesData.payUsdtAmount
          }
        })
      }

    },
    created: function () {
      this.getSharesViweData()
    }
  }
</script>
