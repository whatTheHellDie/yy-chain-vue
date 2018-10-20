<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="top-nav">
            <div aria-label="Breadcrumb" role="navigation" class="el-breadcrumb">
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">账户中心</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span>
              <span class="el-breadcrumb__item">
                <span role="link" class="el-breadcrumb__inner">个人信息</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i>
              </span> <span class="el-breadcrumb__item bold" aria-current="page">
                <span role="link" class="el-breadcrumb__inner" style="font-weight: 800;">重置支付密码</span>
                <i class="el-breadcrumb__separator el-icon-arrow-right"></i></span></div>
          </div>
          <div class="box-body min425">
            <div class="charge-coin set-password forget-password">
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">原支付密码</label><input type="password" class="form-control input" placeholder="请输入您绑定的手机号码"><span class="lh35 ml10 default" @click="$router.push({name:'forgetPaymentPassword'})">忘记原密码？</span>
                <span class="tip max-220"></span>
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">新支付密码</label><input type="password" class="form-control input" placeholder="请输入短信验证码">
                  <span class="tip"></span>
              </div>
              <div class="form-group combo-form">
                <label for="zhuan" class="col-sm-2 control-label">确认新支付密码</label><input type="password" class="form-control input" placeholder="6位数字">
                 <span class="tip"></span>
              </div>
              <span class="tip"></span>
              <div class="gu-btn" @click="resetPayPwd()">提交</div>
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
        activeNumber: 0,
        chooseList: [{
            name: '全部'
          },
          {
            name: '待支付'
          },
          {
            name: '已完成'
          },
          {
            name: '交易关闭'
          },
          {
            name: '交易取消'
          }
        ],
        chooseContent: [{
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 0, //0待支付，1已完成，2交易关闭，3交易取消
            cny: "1000",
            usdt: "0.954549",
            number: "10万个",
            total: "9.511152",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452"
          },
          {
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 1, //0待支付，1已完成，2交易关闭，3交易取消
            cny: "1000",
            usdt: "0.954549",
            number: "10万个",
            total: "9.511152",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452"
          },
          {
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 2, //0待支付，1已完成，2交易关闭，3交易取消
            cny: "1000",
            usdt: "0.954549",
            number: "10万个",
            total: "9.511152",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452"
          },
          {
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 3, //0待支付，1已完成，2交易关闭，3交易取消
            cny: "1000",
            usdt: "0.954549",
            number: "10万个",
            total: "9.511152",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452"
          },
        ]
      }
    },
    components: {
      MainBody
    },
    methods: {
      //重置支付密码
      resetPayPwd(){

      },
      loadList(i) {
        this.activeNumber = i;
      },
      pay(id) {

      },
      cancelOrder(id) {

      },
      delOrder(id) {

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
