<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-order">
              <ul class="order-number">
                <li>
                  <div class="number">{{!totalNumberOrder ? '0' : totalNumberOrder}}</div>
                  <div class="content">订单总数（笔）</div>
                </li>
                <li>
                  <div class="number">{{!totalNumberUsdtAmount ? '0.0' : totalNumberUsdtAmount}}</div>
                  <div class="content">累计入股USDT数额（枚）</div>
                </li>
                <li>
                  <div class="number">{{!totalNumberYyiAmount ? '0.0' : totalNumberYyiAmount}}</div>
                  <div class="content">累计分配易用积分（万个）</div>
                </li>
              </ul>
            </div>
            <div class="bechoose-list rugu-list">
              <dl v-if="chooseContent.length">
                <dd class="clearfix" v-for="item,i in chooseContent">
                  <div class="clearfix">
                    <div class="title clearfix">入股订单编号：{{item.number}}
                      <span class="span active">已完成</span>
                    </div>
                    <div class="left">
                      <div class="yy-content">
                        <div class="yy-title">分配易用积分单价</div>
                        <div class="y-content w402 clearfix">
                          <span class="cny">CNY: {{item.sendYyiPrice*10000}}元/万个</span>
                          <span class="usdt">USDT: {{item.usdtPrice*10000}}枚/万个</span>
                        </div>
                      </div>
                      <div class="yy-content">
                        <div class="yy-title">数量</div>
                        <div class="y-content min60 clearfix">
                          <span>{{item.sendYyiQuantity}}</span>
                        </div>
                      </div>
                      <div class="yy-content">
                        <div class="yy-title">合计(USDT)</div>
                        <div class="y-content min230 clearfix">
                          <span class="default he-usdt">{{item.usdtAmount}}</span>
                        </div>
                      </div>
                    </div>
                    <div class="right">

                    </div>
                  </div>
                  <div class="not-pass xiadan">
                    <span>下单时间：{{item.createTime}}</span><span class="pull-right">支付时间：{{item.payTime}}</span>
                  </div>
                </dd>
              </dl>
              <div v-else><img src="/static/img/no-data.jpg" alt="" class="no-data" /></div>
            </div>

            <div class="block" style="text-align: center;margin: 30px 0 40px" v-if="chooseContent.length">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                background=""
                :current-page="pageNum"
                :page-size="pageSize"
                layout="prev, pager, next, jumper"
                :total="elementTotal">
              </el-pagination>
            </div>

          </div>
        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
.no-data{
  display: block;
  margin: 0 auto;
  padding: 130px 0;
}
</style>

<script>
  // import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data () {
      return {
        totalNumberYyiAmount: '0.000000',
        totalNumberOrder: '0',
        totalNumberUsdtAmount: '0',
        pageNum: 1,
        pageSize: 5,
        elementTotal: '',
        currentPage3: 5,
        activeNumber: 0,
        chooseContent: []
      }
    },
    components: {
      MainBody
    },
    activated () {
      this.loadList()
    },
    methods: {
      loadList (i) {
        this.activeNumber = i
        this.$http({
          url: this.$http.adornUrl('/shares/order/information'),
          method: 'post',
          data: this.$http.adornData({
            'pageNum': 1,
            'pageSize': 5,
            'status': 1
          })
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === '0000') {
            this.totalNumberYyiAmount = data.data.totalNumberYyiAmount
            this.totalNumberOrder = data.data.totalNumberOrder
            this.totalNumberUsdtAmount = data.data.totalNumberUsdtAmount
            this.pageNum = data.data.pageResponse.pageNum
            this.pageSize = data.data.pageResponse.pageSize
            this.elementTotal = data.data.pageResponse.elementTotal
            this.chooseContent = data.data.pageResponse.dataList
          } else {
            this.dataList = []
            this.elementTotal = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      handleSizeChange(val) {
        this.pageSize = val
        this.pageNum = 1
        this.loadList()
      },
      // 当前页
      handleCurrentChange (val) {
        this.pageNum = val
        this.loadList()
      },
      pay (id) {
      },
      cancelOrder (id) {
      },
      delOrder (id) {
      }
    },
    mounted () {
      //    var clipboard = new ClipboardJS('.copy');
      //
      //    clipboard.on('success', function(e) {
      //        e.clearSelection();
      //    });
      //    clipboard.on('error', function(e) {
      //        alert('该默认浏览器不支持点击复制,请长按选择复制钱包地址或选择分享二维码图片')
      //    });
    }
  }
</script>
