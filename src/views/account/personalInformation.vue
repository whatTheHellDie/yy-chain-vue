<template>
  <div class="body-grey">
    <main-body navIndex="1">
      <div slot="content">
        <div class="box-card2">
          <div class="box-body no-padding">
            <div class="index-i index-p">
              <img src="/static/img/profile.png" alt="" class="tou">
              <div class="title">
                <span class="hao">YY15844551！</span>
                <div class="level">会员身份：普通会员</div>
                <div class="level">推荐人：YY12345698</div>
              </div>
              <div class="change-title">[更换头像]</div>
            </div>
            <div class="account-top">
              <div class="title">账户信息</div>
              <dl>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      手机号码<span class="pin">159 **** 1421</span>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      实名认证<span class="pin active">未认证</span>
                  <div class="gu-btn">认证</div>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="right" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      登录密码<span class="pin">已设置</span>
                </dd>
                <dd class="clearfix">
                  <icon-svg name="warning" class="site-sidebar__menu-icon" style="vertical-align: -2px;"></icon-svg>
                                                      实名认证<span class="pin active">未设置</span>
                  <div class="gu-btn">设置</div>
                </dd>
              </dl>
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
      resetVal(number) {

        var mNumber = parseInt(number)
        number = number.toString()
        var point = number.split('.')[1]
        console.log(point)
        mNumber = mNumber.toString()
        if(mNumber.length <= 3)
          return(mNumber == '' ? '0' : mNumber) + '.' + point;
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