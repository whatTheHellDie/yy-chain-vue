<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card order-page">
      <h1>入股</h1>
      <div class="box-body box-body0">
        <div class="left">
          <div class="qi">第 <span>35</span> / 100 期</div>
          <div class="intro">每期配送完毕，自动进入下一期，单价上涨0.1%。</div>
          <div class="number">
            <span class="span">1000</span> 万个
            <div class="mt14">本期剩余配送易用积分</div>
          </div>
          <div class="time-price">
            <div class="title1">当前价格</div>
            <div class="content1"><span class="span1">CNY：<span class="default">3000</span>元/万个</span>
              <span class="span">USDT约：<span class="default">2.125221</span>枚/万个
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
          <div>账户USDT余额：<span class="default">6.000251</span>
            <el-button type="primary" class="charge-btn">充币</el-button>
          </div>
          <div class="default1 mt11">起配积分数额：1万个</div>
          <div class="add_one clearfix">
            <div class="add reduce">-</div><input type="text" value="10"><div class="add">+</div>
          </div>
          <div style="padding-left: 92px;">约支付USDT(枚)：<span class="default">0.951115</span></div>
          <div class="gu-btn cursor">立即入股</div>
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
   
</style>

<script>
  //import { getUUID } from '@/utils'
  import MyHeader from '@/components/common/header'
  import MyFooter from '@/components/common/footer'
  export default {
    data() {
      return {
        num1: 1
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    methods: {
      handleChange(value) {
        console.log(value);
      }
    }
  }
</script>