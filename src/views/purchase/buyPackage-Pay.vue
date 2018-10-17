<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card order-page">
      <h1>订单详情</h1>
      <div class="box-body no-padding">
        <div class="buy-detail">
          <dl>
          	<dd><label>交易数额（USDT）：</label><span class="span">0.12345123枚</span></dd>
          	<dd><label>配送易用积分：</label><span class="span">1万个</span></dd>
          </dl>
        </div>
        <div class="buy-detail2">
        <h1>支付订单</h1>
          <dl>
            <dd><label>需支付USDT(枚)：</label><span class="span">0.951115</span></dd>
            <form class="m-form" method="post">
            	<input type="password" class="input" /> <span class="wang">忘记密码？</span>
            	<div class="shu-tip">请输入6位数字支付密码</div>
            	<div class="gu-btn">确认支付</div><span class="please">请在 <span class="time">120S</span> 内完成支付！</span>
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