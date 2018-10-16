<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="index-i">
              <img src="/static/img/profile.png" alt="" class="tou">
              <div class="title">早上好，YY15844551！
                <div class="renzheng">
                  <div class="icon" style="background-image:url(/static/img/i1.png) ;" title="身份未认证"></div> |
                  <div class="icon active" style="background-image:url(/static/img/i2.png) ;" title="手机已认证"></div> |
                  <div class="icon" style="background-image:url(/static/img/i3.png) ;" title="银行卡未认证"></div>
                </div>
              </div>
            </div>
            <div class="my-zichang hei212">
              <div class="title">账户资产</div>
              <div class="clear"></div>
              <div class="money-list clearfix">
                <div class="w33">
                  <div class="name">YYC（枚）</div>
                  <div class="content" v-html="resetVal(4234.03)"></div>
                </div>
                <div class="w33">
                  <div class="name">USDT（枚）</div>
                  <div class="content" v-html="resetVal(5234.564321)"></div>
                </div>
                <div class="w33">
                  <div class="name">易用积分（个）</div>
                  <div class="content" v-html="resetVal(234.01)"></div>
                  <div class="hover-content">
                    <div class="clearfix">配送易用积分<span class="pull-right">30.00</span></div>
                    <div class="clearfix">激励易用积分<span class="pull-right">30.00</span></div>
                    <div class="clearfix">未到账易用积分<span class="pull-right">30.00</span></div>
                  </div>
                </div>
              </div>
            </div>
            <div class="my-zichang">
              <div class="title">团队概况</div>
              <div class="clear"></div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    团队总人数(人)
                  </div>
                  <div class="number">80</div>
                
                </div>
                <div class="right clearfix">
                  <div class="w33">
                    <div class="population">vip会员(人)</div>
                    <div class="s-number">60</div>
                  </div>
                  <div class="w33">
                    <div class="population">普通会员(人)</div>
                    <div class="s-number">10</div>
                  </div>
                  <div class="w33">
                    <div class="population">其他(人)</div>
                    <div class="s-number">10</div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    累计获得奖励易用积分数(个)
                  </div>
                  <div class="number" v-html="resetVal(234.03)"></div>
                
                </div>
                <div class="right clearfix">
                  <div class="w33">
                    <div class="population">今日获得(个)</div>
                    <div class="s-number" v-html="resetVal(234.02)"></div>
                  </div>
                  <div class="w33">
                    <div class="population">本月获得(个)</div>
                    <div class="s-number" v-html="resetVal(234.01)"></div>
                  </div>
                  <div class="w33">
                    <div class="population">其他(人)</div>
                    <div class="s-number">10</div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                  累计获得USDT数额(枚)
                  </div>
                  <div class="number">234.00</div>
                
                </div>
                <div class="right clearfix">
                  <div class="w33 w50">
                    <div class="population">今日获得(枚)</div>
                    <div class="s-number" v-html="resetVal(234.001233)"></div>
                  </div>
                  <div class="w33 w50">
                    <div class="population">本月获得(枚)</div>
                    <div class="s-number" v-html="resetVal(234.001233)"></div>
                  </div>
                </div>
              </div>
              <div class="team-list clearfix">
                <div class="left">
                  <div class="population">
                    团队总人数(人)
                  </div>
                  <div class="number">80</div>
                
                </div>
                <div class="right clearfix">
                  <div class="w33 w50">
                    <div class="population">今日业绩(个)</div>
                    <div class="s-number" v-html="resetVal(234.001233)"></div>
                  </div>
                  <div class="w33 w50">
                    <div class="population">本月业绩(个)</div>
                    <div class="s-number" v-html="resetVal(234.001233)"></div>
                  </div>
                </div>
              </div>
            </div>
            
          </div>

        </div>
      </div>
    </main-body>
  </div>
</template>
<style lang="scss" scoped>
  .bold {
    .el-breadcrumb__item:last-child .el-breadcrumb__inner {
      font-weight: 800;
    }
  }
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
      resetVal(number) {
        
        var mNumber = parseInt(number)
        number = number.toString()
        var point = number.split('.')[1]
        console.log(point)
        mNumber = mNumber.toString()
        if(mNumber.length <= 3)
          return (mNumber == '' ? '0' : mNumber) + '.' + point;
        else {
          var mod = mNumber.length % 3;
          var output = (mod == 0 ? '' : (mNumber.substring(0, mod)));
          for(var i = 0; i < Math.floor(mNumber.length / 3); i++) {
            if((mod == 0) && (i == 0))
              output += mNumber.substring(mod + 3 * i, mod + 3 * i + 3);
            else
              output += ',' + mNumber.substring(mod + 3 * i, mod + 3 * i + 3);
          }
          return(output + '.<span class="little-num">' + point + '</span>');
        }
        //      return `${val.parseInt}.<span class="little-word">${dian}</span>`
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