<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-order">
              <ul class="order-number">
                <li>
                  <div class="number">{{orderCount}}</div>
                  <div class="content">订单总数（笔）</div>
                </li>
                <li>
                  <div class="number">{{usdtTotal}}</div>
                  <div class="content">累计充值USDT数额（枚）</div>
                </li>
                <li>
                  <div class="number">{{usdtSurplus}}</div>
                  <div class="content">当前可用USDT余额（枚）</div>
                </li>
              </ul>
            </div>
            <div class="charge-list">
              <div class="choose-box">
                <div class="choose-list">
                  <div v-for="item,i in chooseList" class="box" @click="loadList(i)" :class="{active:i == activeNumber}"><span class="span">{{item.name}}</span></div>
                </div>
              </div>
            </div>
            <div class="bechoose-list">
              <dl>
                <dd class="clearfix" v-for="item,i in chooseContent">
                  <div class="clearfix">
                    <div class="title clearfix">充币订单编号：{{item.number}}
                      <span v-if="item.status==0" class="span active">待受理</span>
                      <span v-if="item.status==1" class="span active">已受理</span>
                      <span v-if="item.status==2" class="span">不受理</span>
                    </div>
                    <div class="left">
                      <div class="content">
                        <div class="number">交易号: <span class="hao">{{item.chargeNumber}}</span></div>
                        <div class="number">凭证: <span class="hao"><img :src="item.chargeVoucher" alt="" class="pin" /></span></div>
                        <div class="number">下单时间: <span class="hao">{{item.createTime}}</span></div>
                      </div>
                    </div>
                    <div class="gradient"></div>
                    <div class="right">
                      <div class="number number2">充币数额（USDT）：<br><span class="hao2">{{item.realChargeAmount}}</span></div>
                    </div>
                  </div>
                  <div class="not-pass" v-if="item.status==2">
                    <span>不受理原因：{{item.remark}}</span><span class="pull-right charge-btn" @click="reUpload(item.number,item.chargeNumber,item.chargeVoucher)">重新上传</span>
                  </div>
                </dd>
              </dl>
            </div>
            <div class="block" style="text-align: center;margin: 30px 0 40px">
              <el-pagination
                @size-change="sizeChangeHandle"
                @current-change="currentChangeHandle"
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
  .bechoose-list .gradient {
    background-image: url(/static/img/gradient2.png);
  }
</style>

<script>
  //import { getUUID } from '@/utils'
  import MainBody from '@/components/common/mainBody'
  export default {
    data() {
      return {
        pageNum: 1,
        pageSize: 5,
        elementTotal: '',
        currentPage3: 5,
        activeNumber: 0,
        orderNumber:'',
        index:0,
        orderCount:'',
        usdtTotal:0,
        usdtSurplus:'',
        chooseList: [{
            name: '全部'
          },
          {
            name: '待受理'
          },
          {
            name: '已受理'
          },
          {
            name: '不受理'
          },
        ],
        chooseContent: [{
            number: "",
            status: '', //0待受理，1已受理，2不受理
            chargeNumber: '',
            chargeVoucher: "/static/img/sub.png",
            createTime: '',//下单时间
            realChargeAmount: ''//充币数额
          }
        ]
      }
    },
    created () {
      this.loadList(0),
      this.getCount()
    },
    components: {
      MainBody
    },
    methods: {
      getCount(){
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/fund/au/person/chargeOrder'),
          method: 'post'
        }).then(({data}) =>{
          if (data.code === '0000') {
            this.orderCount = data.data.orderCount;
            this.usdtTotal = data.data.usdtTotal;
            this.usdtSurplus = data.data.usdtSurplus;
          }
        this.dataListLoading = false
        })
      },

      //i 0,待处理 1，受理，2 不受理 3 全部
      loadList(i) {
        this.dataListLoading = true
        this.activeNumber = i
        this.index = i
        this.$http({
          url: this.$http.adornUrl('/bs/fund/query/list/page'),
          method: 'post',
          data: this.$http.adornData({
            'pageNum': this.pageNum,
            'pageSize': this.pageSize,
            'accountType': i,
            'platform': 2
          })
        }).then(({data}) => {
          if (data && data.code === '0000') {
            console.log(data.data.dataList)
            this.chooseContent = data.data.dataList ;
            this.elementTotal = data.data.elementTotal ;
            this.dataListLoading = true;
          } else {
            this.dataList = []
            this.elementTotal = 0
          }
          this.dataListLoading = false;
        })
      },
      reUpload(id,number,chargeVoucher){
        this.$router.push({ name: 'reChargeCoin',
           query: {
             orderNumber: id ,
             chargeNumber :number,
             chargeVoucher : chargeVoucher
          }})
      },
      // 每页数
      sizeChangeHandle(val) {
        this.pageSize = val
        this.pageNum = 1
        this.loadList(this.index)
      },
      // 当前页
      currentChangeHandle(val) {
        this.pageNum = val
        this.loadList(this.index)
      },
    },

    mounted () {
      //    var clipboard = new ClipboardJS('.copy');
      //
      //    clipboard.on('success', function(e) {
      //        e.clearSelection()
      //    });
     //    clipboard.on('error', function(e) {
      //        alert('该默认浏览器不支持点击复制,请长按选择复制钱包地址或选择分享二维码图片')
      //    });
    }
  }
</script>
