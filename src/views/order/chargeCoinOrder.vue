<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="charge-order">
              <ul class="order-number">
                <li>
                  <div class="number">0</div>
                  <div class="content">订单总数（笔）</div>
                </li>
                <li>
                  <div class="number">0</div>
                  <div class="content">累计充值USDT数额（枚）</div>
                </li>
                <li>
                  <div class="number">0</div>
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
                    <div class="title clearfix">充币订单编号：{{item.orderNumber}}
                      <span v-if="item.status==0" class="span active">待受理</span>
                      <span v-if="item.status==1" class="span active">已受理</span>
                      <span v-if="item.status==2" class="span">不受理</span>
                    </div>
                    <div class="left">
                      <div class="content">
                        <div class="number">交易号: <span class="hao">{{item.tradeNumber}}</span></div>
                        <div class="number">凭证: <span class="hao"><img :src="item.img" alt="" class="pin" /></span></div>
                        <div class="number">下单时间: <span class="hao">{{item.time}}</span></div>
                      </div>
                    </div>
                    <div class="gradient"></div>
                    <div class="right">
                      <div class="number number2">充币数额（USDT）：<br><span class="hao2">{{item.usdt}}</span></div>
                    </div>
                  </div>
                  <div class="not-pass" v-if="item.status==2">
                    <span>不受理原因：{{item.reason}}</span><span class="pull-right charge-btn" @click="reUpload(item.id)">重新上传</span>
                  </div>
                </dd>
              </dl>
            </div>
            <div class="block" style="text-align: center;margin: 30px 0 40px">
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
        chooseContent: [/*{
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 0, //0待受理，1已受理，2不受理
            tradeNumber: "dsfdfsafdsaersxcsdfsdfsdfsdfsdfs",
            img: "/static/img/sub.png",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452"
          },
          {
            id: "sdfsdfsfsdfsdfsd2323",
            orderNumber: '2018083161408819',
            status: 2, //0待受理，1已受理，2不受理
            tradeNumber: "dsfdfsafdsaersxcsdfsdfsdfsdfsdfs",
            img: "/static/img/sub.png",
            time: "2018-09-22 11:00:02",
            usdt: "2.123452",
            reason: "XXXXXXXXXXXXXXXXXXX"
          }*/
        ]
      }
    },
    components: {
      MainBody
    },
    methods: {
      loadList(i) {
        console.log(i)
        this.activeNumber = i;
      },
      reUpload(id){
        this.$router.push({ name: 'reChargeCoin' })
      }
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
