<template>
  <div class="body-grey">
    <my-header></my-header>
    <div class="box-card mb50 mt40">
      <h1>支付成功</h1>
      <div class="box-body">
        <div class="pay-status" v-if="status=='success'">
          <img src="/static/img/success.png"/><span class="title">恭喜您，您已成功支付订单！</span>
          <div class="to">
            <div class="gu-btn">查看订单</div>
            <div class="gu-btn back-index">返回首页</div>
          </div>
        </div>
      </div>
    </div>
    <div class="box-card mb50 mt40">
      <h1>支付失败</h1>
      <div class="box-body">
        <div class="pay-status" v-if="status=='success'">
          <img src="/static/img/failed.png"/><span class="title">抱歉，您的订单支付失败</span>
          <div class="to">
            <div class="gu-btn">重新下单</div>
            <div class="gu-btn back-index">返回首页</div>
          </div>
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
        status:"success"
      }
    },
    components: {
      MyHeader,
      MyFooter
    },
    created(){
    },
    methods: {}
  }
</script>
