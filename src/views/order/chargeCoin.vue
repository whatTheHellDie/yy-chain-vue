<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <h1>充币</h1>
          <div class="box-body">
            <div class="charge-coin">
              <div class="blue title">第一步：转账</div>
              <form class="form-horizontal" role="form">
                <div class="form-group combo-form clearfix">
                  <div class="way">方式一：扫描收款二维码
                    <div class="please">请用您的个人数字钱包扫描二维码进行转账（LoveBird）。<br>
                      <div class="erwei">

                      </div>
                    </div>
                  </div>
                  <div class="way">方式二：收款方钱包地址
                    <div class="please">请您复制收款方数字钱包地址，在您的数字钱包中进行转账支付（LoveBird）。<br>
                      <div class="copy text-right" v-clipboard:copy="copyMessage">复制钱包地址</div>
                      <div class="clear"></div>
                      <div class="wallet-address">
                        <div>钱包地址</div>
                        <div>{{copyMessage}}</div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="blue title">第二步 填写转账凭证</div>
                <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">转账数额</label><input type="text" class="form-control input" placeholder="请输入转账LoveBird数额">
                </div>
                <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">交易号</label><input type="text" class="form-control input" placeholder="请在钱包记录页面复制URL粘贴在这里">
                </div>
                
                <div class="form-group combo-form">
                  <label for="zhuan" class="col-sm-2 control-label">上传凭证</label>
                  <div class="col-sm-4 pin">
                    凭证为转账支付成功的订单详情图片，需包含以下四项内容：转账金额、转账用户钱包地址、收款方钱包地址、交易时间。
                    <div class="clearfix">
                      <div class="tou-s">
                        <img src="/static/img/sub.png" class="img" />
                        <div class="btn-sub">上传图片</div>
                      </div>
                    </div>
                  </div>

                </div>

                    <div class="gu-btn">提交</div>

              </form>
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
    data() {
      return {
        copyMessage:"LjoeXwCevS6RxdBjfLKBF5EQDLT2B1uRMWsdfsdf",
        num1: 1
      }
    },
    components: {
      MainBody
    },
    methods: {
      handleChange(value) {
        console.log(value);
      }
    },
    mounted() {
      //    var clipboard = new ClipboardJS('.copy');
      //
      //    clipboard.on('success', function(e) {
      //        e.clearSelection();
      //        
      //    });
      //    
      //    clipboard.on('error', function(e) {
      //        alert('该默认浏览器不支持点击复制,请长按选择复制钱包地址或选择分享二维码图片')
      //    });
    }
  }
</script>