<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-order">
              <ul class="order-number">
                <li>
                  <div class="number">2</div>
                  <div class="content">订单总数（笔）</div>
                </li>
                <li>
                  <div class="number">20.213511</div>
                  <div class="content">累计入股USDT数额（枚）</div>
                </li>
                <li>
                  <div class="number">20</div>
                  <div class="content">累计分配易用积分（万个）</div>
                </li>
              </ul>
            </div>
            <div class="bechoose-list rugu-list">
              <dl>
                <dd class="clearfix" v-for="item,i in chooseContent">
                  <div class="clearfix">
                    <div class="title clearfix">入股订单编号：{{item.orderNumber}}
                      <span class="span active">已完成</span>
                    </div>
                    <div class="left">
                      <div class="yy-content">
                        <div class="yy-title">分配易用积分单价</div>
                        <div class="y-content w402 clearfix">
                          <span class="cny">CNY: {{item.cny}}元/万个</span>
                          <span class="usdt">USDT: {{item.usdt}}枚/万个</span>
                        </div>
                      </div>
                      <div class="yy-content">
                        <div class="yy-title">数量</div>
                        <div class="y-content min60 clearfix">
                          <span>{{item.number}}</span>
                        </div>
                      </div>
                      <div class="yy-content">
                        <div class="yy-title">合计(USDT)</div>
                        <div class="y-content min230 clearfix">
                          <span class="default he-usdt">{{item.total}}</span>
                        </div>
                      </div>
                    </div>
                    <div class="right">

                    </div>
                  </div>
                  <div class="not-pass xiadan">
                    <span>下单时间：2018-09-22 11:00:02</span><span class="pull-right">支付时间：2018-09-22 11:00:02</span>
                  </div>
                </dd>
              </dl>
            </div>
            <div class="m-page mb40">
              <ul class="pagination">
                <li>
                  <a href="javascript:void(0)">&lt;</a>
                </li>
                <li class="active">
                  <a href="javascript:void(0)">1</a>
                </li>
                <li>
                  <a href="javascript:void(0)">2</a>
                </li>
                <li>
                  <a href="javascript:void(0)">3</a>
                </li>
                <li>
                  <a href="javascript:void(0)">4</a>
                </li>
                ...
                <li>
                  <a href="javascript:void(0)">5</a>
                </li>
                <li>
                  <a href="javascript:void(0)">&gt;</a>
                </li>
              </ul>
              <div class="turn-to">
                跳至<input type="text" value="1">页
                <a href="javascript:void(0)" class="btn btn-turn">跳转</a>
              </div>
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